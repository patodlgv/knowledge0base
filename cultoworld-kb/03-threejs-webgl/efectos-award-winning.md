# Efectos WebGL de referencia award-winning (y cómo se logran)

> **Tier: PREMIUM** · **Costo de perf: ALTO** · Investigado de case studies de Codrops y análisis de sitios ganadores (Unseen, Lusion, Bruno Simon, Active Theory). Cada efecto indica si tiene aproximación ligera.

## 1. Orbe/nube de partículas reactiva al mouse (el de Culto)

Miles de puntos en GPU, empuje con inercia. Patrón base validado:

```js
const COUNT = 5000;
const positions = new Float32Array(COUNT * 3);
for (let i = 0; i < COUNT; i++) {
  const r = 1.5, t = Math.acos(2 * Math.random() - 1), p = Math.random() * Math.PI * 2;
  positions[i*3]   = r * Math.sin(t) * Math.cos(p);
  positions[i*3+1] = r * Math.sin(t) * Math.sin(p);
  positions[i*3+2] = r * Math.cos(t);
}
const geo = new THREE.BufferGeometry();
geo.setAttribute("position", new THREE.BufferAttribute(positions, 3));

const mat = new THREE.ShaderMaterial({
  uniforms: { uTime: { value: 0 }, uMouse: { value: new THREE.Vector3() },
              uColor: { value: new THREE.Color("#C5A46D") } },
  vertexShader: /* glsl */`
    uniform float uTime; uniform vec3 uMouse;
    void main(){
      vec3 p = position;
      float d = distance(p, uMouse);
      p += normalize(p - uMouse) * smoothstep(1.0, 0.0, d) * 0.4;
      vec4 mv = modelViewMatrix * vec4(p, 1.0);
      gl_PointSize = 2.5 * (1.0 / -mv.z);
      gl_Position = projectionMatrix * mv;
    }`,
  fragmentShader: /* glsl */`
    uniform vec3 uColor;
    void main(){
      if (length(gl_PointCoord - 0.5) > 0.5) discard;
      gl_FragColor = vec4(uColor, 1.0);
    }`,
  transparent: true, blending: THREE.AdditiveBlending, depthWrite: false,
});
scene.add(new THREE.Points(geo, mat));

// mouse con inercia → uniform (en el tick):
// smoothMouse.lerp(targetMouse, 0.1); mat.uniforms.uMouse.value.copy(smoothMouse);
```

## 2. Imágenes DOM → planes WebGL con distorsión por scroll (Unseen/Locomotive)

Cada `<img>` se replica como plane sincronizado (`getBoundingClientRect` + scroll de Lenis). La velocidad del scroll entra como uniform que curva el plane:

```glsl
// vertex shader — curvatura por velocidad
uniform float uVelocity;
varying vec2 vUv;
void main() {
  vUv = uv;
  vec3 pos = position;
  pos.y += sin(uv.x * 3.14159) * uVelocity * 0.02;   // curva tipo bandera
  gl_Position = projectionMatrix * modelViewMatrix * vec4(pos, 1.0);
}
```
```glsl
// fragment — RGB shift proporcional a la velocidad
uniform sampler2D uTexture; uniform float uVelocity;
varying vec2 vUv;
void main() {
  float r = texture2D(uTexture, vUv + vec2(uVelocity * 0.001, 0.0)).r;
  vec2 gb = texture2D(uTexture, vUv).gb;
  gl_FragColor = vec4(r, gb, 1.0);
}
```

**Aproximación LIGERA:** skew por velocidad con GSAP (`02-animacion-premium/scrolltrigger-avanzado.md` §4) — 80% de la sensación, 0 KB de WebGL.

## 3. Hover con flowmap (la imagen ondula donde pasa el cursor)

El helper `Flowmap` de OGL escribe la velocidad del mouse en una textura off-screen que decae; el fragment shader desplaza UVs con esa textura, con offset distinto por canal RGB (aberración cromática direccional). Referencia: Codrops "Mouse Flowmap Deformation with OGL".

Nota: OGL (~30 KB) es la vía del plan intermedio para UN efecto shader sin cargar Three.js completo. Sitios ganadores que lo usan: Tux Creative, 21 TSI.

**Aproximación LIGERA:** filtro SVG `feTurbulence` + `feDisplacementMap` animando `scale` al hover (Codrops "Image Distortion Effects with SVG Filters") — vigilar perf, solo desktop.

## 4. Transición de imagen con displacement map (galerías premium)

Dos texturas + un mapa B/N que controla el `mix()`:

```glsl
uniform sampler2D uTex1, uTex2, uDisp;
uniform float uProgress;
varying vec2 vUv;
void main() {
  float d = texture2D(uDisp, vUv).r;
  vec2 uv1 = vUv + uProgress * d * 0.3;
  vec2 uv2 = vUv - (1.0 - uProgress) * d * 0.3;
  gl_FragColor = mix(texture2D(uTex1, uv1), texture2D(uTex2, uv2), uProgress);
}
```
`gsap.to(mat.uniforms.uProgress, { value: 1, duration: 1.2, ease: "power2.inOut" })`.

## 5. Reveal on-scroll de imágenes WebGL (galería tipo SOTD 2026)

ScrollTrigger anima `uProgress` 0→1; el fragment lo usa contra noise con `smoothstep` para un dissolve orgánico. Patrón completo en Codrops: "Building a Scroll-Revealed WebGL Gallery with GSAP, Three.js, Astro and Barba.js" (feb 2026). Clave arquitectónica: **un solo canvas fijo** persiste entre páginas (Barba solo cambia el DOM; los planes se remapean).

**Aproximación LIGERA:** `clipPath: inset()` + scale interna (ver `01/scroll-reveals.md` §5) — cubre el 80% percibido.

## 6. Mundo 3D navegable (Bruno Simon)

Coche + física + matcaps. Componentes: Three.js + física WASM (Rapier/Cannon) en mundo paralelo sincronizado por frame + matcaps para todo el look. Fuente completa MIT: **github.com/brunosimon/folio-2025**. Es un build de semanas; se estudia, no se clona.

## 7. Fluido on-drag (Unseen) — el jefe final

Simulación Stable-Fluids en GPU (campos de velocidad/divergencia/presión) inyectando fuerza en el segmento mouse-anterior → mouse-actual; la imagen ondula como líquido. Advertencia honesta: build iterativo multi-día con GLSL custom. No prometer one-shot.

## 8. Texto dentro de WebGL

`troika-three-text`: tipografía SDF como geometría 3D — puede ondular/distorsionarse con shaders. Tiene parser de fuentes en Worker (no bloquea main thread). Solo cuando el texto ES parte del efecto; recuerda: texto en canvas = invisible para Google.

## Física como lenguaje (la alternativa 2D que gana premios)

**Nod Coding Bootcamp** ganó SOTD + Developer Award SIN WebGL 3D: Matter.js (física 2D) + GSAP + Lottie. Elementos que caen, chocan y se arrastran. Sincronización: `body.position/angle` → `transform` del DOM cada frame.
→ Arma secreta para dar "feel award" a un proyecto intermedio sin Three.js (Matter.js ≈ 87 KB — evaluar budget).

## Reglas transversales

1. GSAP anima uniforms igual que DOM → UNA timeline orquesta canvas + HTML (coherencia = jurado contento).
2. Todo efecto necesita su fallback estático bello (ver `lazy-load-fallbacks.md`).
3. Efectos con textura de mouse (flowmap, fluido): resolución del sim buffer a 128–256px, no full-res.
4. Nuevo código shader para el sitio insignia: considerar TSL (compila a WebGPU + WebGL).
