# Lazy-load del canvas, detección de capacidad y fallbacks

> **Tier: PREMIUM** · **Costo de perf: reduce el ALTO a tolerable** — esto es lo que permite que un sitio WebGL no muera en móvil.

## Principio

El HTML/CSS del sitio pinta PRIMERO (hero estático con la imagen de marca); el canvas llega después y se desvanece encima. Nunca bloquear el primer paint con 2 MB de JS+assets.

## 1. Import dinámico + IntersectionObserver

```js
// main.js — el bundle inicial NO incluye Three.js
const mount = document.querySelector("#webgl-mount");

const io = new IntersectionObserver(async ([entry]) => {
  if (!entry.isIntersecting) return;
  io.disconnect();
  const { initScene } = await import("./scene.js");   // chunk separado (Vite lo separa solo)
  const scene = await initScene(mount);
  gsap.to("#webgl-fallback", { opacity: 0, duration: 0.8 });  // crossfade imagen → canvas
}, { rootMargin: "200px" });

io.observe(mount);
```

```html
<div id="webgl-mount">
  <img id="webgl-fallback" src="/img/hero-static.avif" alt="" fetchpriority="high">
  <!-- el canvas se inserta aquí -->
</div>
```

El fallback estático ES el LCP → optimizado como cualquier imagen hero.

## 2. Detección de capacidad (decidir si cargar WebGL siquiera)

```js
function canAffordWebGL() {
  if (matchMedia("(prefers-reduced-motion: reduce)").matches) return false;
  const gl = document.createElement("canvas").getContext("webgl2");
  if (!gl) return false;
  if (navigator.deviceMemory && navigator.deviceMemory < 4) return false;   // Chrome/Android
  if (navigator.hardwareConcurrency && navigator.hardwareConcurrency < 4) return false;
  if (navigator.connection?.saveData) return false;
  return true;
}

if (canAffordWebGL()) { /* import dinámico */ }
else { /* la imagen estática se queda; opcional: video corto en loop */ }
```

Escalonar en vez de todo-o-nada:

```js
const quality = !gl ? "static"
  : (navigator.deviceMemory ?? 8) < 4 ? "low"     // DPR 1, 1/3 de partículas, sin postprocesado
  : (navigator.deviceMemory ?? 8) < 8 ? "medium"  // DPR 1.5
  : "high";                                        // DPR 2, todo encendido
```

## 3. Pausar cuando no se ve (fps y batería)

```js
let running = true;
new IntersectionObserver(([e]) => { running = e.isIntersecting; }).observe(canvas);
document.addEventListener("visibilitychange", () => { running = !document.hidden; });

function tick(now) {
  requestAnimationFrame(tick);
  if (!running) return;          // el loop existe pero no renderiza
  // ...update + render
}
```

## 4. Degradación en caliente (watchdog de fps)

Si el dispositivo mintió y va a 20fps, bajar calidad en vivo:

```js
let slowFrames = 0;
function watchdog(delta) {
  if (delta > 1 / 25) slowFrames++; else slowFrames = Math.max(0, slowFrames - 1);
  if (slowFrames > 60) {                       // ~2.5s consistentemente lento
    renderer.setPixelRatio(1);
    particles.count = Math.floor(particles.count / 2);
    slowFrames = 0;
  }
}
```

## 5. Fallbacks por nivel

| Situación | Fallback |
|---|---|
| Sin WebGL2 / GPU débil | Imagen estática AVIF (el arte del hero, exportado del render) |
| `prefers-reduced-motion` | Imagen estática, sin excepciones |
| GPU media | Escena con DPR 1, menos partículas, sin post-FX |
| Canvas falla en runtime (context lost) | `canvas.addEventListener("webglcontextlost", ...)` → volver a la imagen |

```js
canvas.addEventListener("webglcontextlost", (e) => {
  e.preventDefault();
  dispose();
  fallbackImg.style.opacity = 1;
});
```

## Cuándo SÍ / Cuándo NO

- ✅ TODO proyecto WebGL sin excepción — es la diferencia entre "premium" y "no abre en mi cel".
- ✅ El fallback estático debe ser BELLO (un render/still del efecto), no un gris de disculpa.
- ❌ No detectar solo por user-agent/`isMobile` — hay Androids con GPU brutal y laptops viejas; medir capacidad.
