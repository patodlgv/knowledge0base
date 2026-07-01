# Three.js en producción — fundamentos

> **Tier: PREMIUM (exclusivo)** · **Costo de perf: ALTO** — Three.js ≈ 150 KB min+gzip antes de dibujar un pixel. "WebGL ligero" no existe; las técnicas de aquí hacen que lo PESADO corra suave.
> Stack premium de Culto: **Vite + Three.js crudo** (sin React — como Unseen/Bruno/Lusion) + GSAP + Lenis.

## Esqueleto de escena con color management correcto

Desde r152, Three.js gestiona color automáticamente: `outputColorSpace` ya es `SRGBColorSpace` por default y los colores/texturas se trabajan en lineal internamente (fuente: threejs.org/manual color-management). Lo que SÍ hay que configurar:

```js
import * as THREE from "three";

const canvas = document.querySelector("#scene");
const renderer = new THREE.WebGLRenderer({ canvas, antialias: true, alpha: true });
renderer.setPixelRatio(Math.min(devicePixelRatio, 2));   // CLAVE: DPR sin cap mata fps en retina
renderer.setSize(innerWidth, innerHeight);

// Tone mapping para look cinematográfico (escenas con luces/PBR):
renderer.toneMapping = THREE.ACESFilmicToneMapping;
renderer.toneMappingExposure = 1.0;
// (Con matcaps/colores planos, dejar NoToneMapping — los matcaps ya vienen "horneados")

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(45, innerWidth / innerHeight, 0.1, 100);
camera.position.z = 5;

// Texturas de COLOR deben marcarse sRGB; mapas de datos (normal, roughness) NO:
const tex = new THREE.TextureLoader().load("/img/foto.jpg");
tex.colorSpace = THREE.SRGBColorSpace;

addEventListener("resize", () => {
  camera.aspect = innerWidth / innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(innerWidth, innerHeight);
  renderer.setPixelRatio(Math.min(devicePixelRatio, 2));
});
```

## Un solo loop con delta-time (patrón Bruno Simon)

Misma velocidad a 60 o 120fps; el clamp evita "teletransportes" al volver de otra pestaña:

```js
let last = performance.now();
const MAX_DELTA = 1 / 30;

function tick(now) {
  const delta = Math.min((now - last) / 1000, MAX_DELTA);
  last = now;

  // UN solo lugar actualiza todo; los uniforms de tiempo se comparten
  app.update(delta);                       // cada sistema recibe delta
  material.uniforms.uTime.value += delta;
  renderer.render(scene, camera);
  requestAnimationFrame(tick);
}
requestAnimationFrame(tick);
```

Prohibido: múltiples loops rAF, o animar dentro de listeners. Todo pasa por el ticker. Si conviven GSAP y Three: `gsap.ticker.add(tick)` en lugar del rAF propio.

## Arquitectura: singleton + módulos de responsabilidad única

```js
export class App {
  static instance;
  constructor() {
    if (App.instance) return App.instance;
    App.instance = this;
    this.renderer = /* … */;
    this.world = new World();      // un archivo = un sistema: World, Cursor, Audio, Loader…
    this.cursor = new Cursor();
  }
  update(delta) { this.world.update(delta); this.cursor.update(delta); }
}
// en cualquier módulo: const app = App.instance;
```

## Matcap: 3D que se ve caro casi gratis

En vez de luces reales (caras), muestrea una esfera pre-iluminada. El truco #1 de Bruno Simon y Unseen:

```js
const matcap = new THREE.TextureLoader().load("/matcaps/gold.png");
matcap.colorSpace = THREE.SRGBColorSpace;
const mesh = new THREE.Mesh(
  new THREE.IcosahedronGeometry(1, 8),
  new THREE.MeshMatcapMaterial({ matcap })
);
```
Banco gratuito: github.com/nidorx/matcaps (600+).

## Disposal (obligatorio en SPAs / React)

WebGL fuga memoria GPU si no limpias:

```js
function dispose() {
  cancelAnimationFrame(rafId);
  scene.traverse((obj) => {
    obj.geometry?.dispose();
    if (obj.material) {
      const mats = Array.isArray(obj.material) ? obj.material : [obj.material];
      mats.forEach((m) => {
        Object.values(m).forEach((v) => v?.isTexture && v.dispose());
        m.dispose();
      });
    }
  });
  renderer.dispose();
}
```

## Assets: DRACO + KTX2 (o el tier premium se autodestruye)

- **DRACO** comprime geometría (decodifica en Web Worker/WASM): `GLTFLoader` + `DRACOLoader`.
- **KTX2/Basis** texturas comprimidas EN VRAM (no solo en red).
- Pipeline offline: `gltf-transform optimize modelo.glb salida.glb --compress draco --texture-compress webp`.
- Presupuesto: modelo hero < 2 MB, texturas ≤ 1024px salvo justificación.

## Performance checklist WebGL

- [ ] DPR cap 2 (en escenas caras, 1.5)
- [ ] Un solo rAF con delta clamp
- [ ] Matcap antes que luces reales; máximo 2–3 luces si son necesarias
- [ ] `renderer.info.render.calls` < ~100 draw calls (fusionar geometrías, InstancedMesh para repetidos)
- [ ] Pausar el loop cuando el canvas no está en viewport (`IntersectionObserver`) y en `document.hidden`
- [ ] Probar en un Android medio: si no da 60fps, bajar DPR/partículas ANTES de embarcar

## WebGPU / TSL (dónde va el futuro)

Three.js r171+ trae `WebGPURenderer` production-ready; Safari 26 (2025) completó el soporte universal. TSL (Three.js Shading Language) escribe shaders en JS que compilan a WGSL y GLSL con fallback automático. Para efectos nuevos del sitio insignia, considerar TSL — future-proof. Referencia: guía de Maxime Heckel "Field Guide to TSL and WebGPU".

## Cuándo SÍ / Cuándo NO

- ✅ Sitio insignia de Culto y clientes premium donde la experiencia ES el producto.
- ❌ JAMÁS en la fábrica. Sensación 3D en ligero = CSS perspective + GSAP ("3D falso") o un shader puntual con OGL (~30 KB) en plan intermedio.
- ⚠️ Aceptar honestamente: shaders finos (fluidos tipo Unseen) son builds iterativos de días, no one-shots.
