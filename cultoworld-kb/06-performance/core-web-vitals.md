# Core Web Vitals: LCP, CLS, INP

> **Tier: LIGERO (crítico) / PREMIUM (con matices)** · Fuente: web.dev. Se evalúa al **percentil 75 de usuarios reales** (CrUX) — tu laptop con fibra no cuenta.

## Umbrales

| Métrica | Bueno | Pobre | Meta interna Culto (ligero) |
|---|---|---|---|
| **LCP** (carga del elemento más grande) | < 2.5s | > 4s | **< 2.0s** |
| **INP** (respuesta a interacciones) | ≤ 200ms | > 500ms | ≤ 160ms |
| **CLS** (estabilidad visual) | ≤ 0.1 | > 0.25 | ≤ 0.05 |

Solo ~56% de los sitios pasan las tres. INP es la más fallada (~43% de sitios).

## LCP — hacer aparecer lo principal YA

El LCP casi siempre es la imagen o el `<h1>` del hero. Descomponer: TTFB → descubrimiento del recurso → descarga → render.

1. **La imagen hero se descubre en el HTML inicial** (no en CSS `background-image`, no inyectada por JS):
```html
<img src="hero.avif" fetchpriority="high" width="1200" height="800" alt="…">
<!-- o si es background obligado: -->
<link rel="preload" as="image" href="hero.avif" fetchpriority="high">
```
2. **NUNCA `loading="lazy"` en el LCP** (error #1 en la práctica).
3. **CSS crítico inline** en `<head>`; diferir el resto. Next.js con App Router + Tailwind ya lo resuelve razonablemente.
4. **Preconnect** a orígenes de assets: `<link rel="preconnect" href="https://res.cloudinary.com">`.
5. **SSG/ISR** para páginas de marketing (HTML completo desde el server/CDN; TTFB mínimo).
6. Fuentes: preload de las 2 críticas + `font-display: swap` (si el LCP es texto, la fuente lo bloquea).
7. CDN (Vercel/Cloudflare ya lo dan).

### GSAP y el LCP (la regla de oro de la fábrica)
El elemento LCP debe estar pintado y visible sin esperar JS:
- Animar el hero con `gsap.from()` → el HTML ya está renderizado; GSAP lo mueve DESPUÉS del paint. ✅
- `opacity: 0` inicial en CSS sobre el `<h1>`/imagen hero → el navegador puede no contarlo como pintado hasta que JS corra. ❌ (para el resto de secciones bajo el fold sí es aceptable.)

## CLS — que nada salte

1. `width` + `height` (o `aspect-ratio`) en TODA imagen/video/iframe/embed.
2. Reservar espacio para contenido tardío (banners, mapas, reviews de terceros): contenedor con `min-height`.
3. No insertar nada ARRIBA de contenido existente después del load.
4. Fuentes con fallback métrico (next/font o `size-adjust`, ver `05-tipografia`).
5. Animaciones que SÍ mueven layout (acordeones): animar `grid-template-rows: 0fr → 1fr` o transform, jamás `height: auto` con reflow por frame.
6. `min-height: 100svh` en heros (no `100vh`, que salta con la barra del navegador móvil).

## INP — que responda al toque

1. **Menos JS.** El presupuesto del tier ligero (~130 KB) existe por esto.
2. Romper tareas largas (>50ms): hidratación pesada, listeners que calculan mucho.
3. `content-visibility: auto` en secciones bajo el fold (el navegador se salta su render inicial):
```css
.section-below { content-visibility: auto; contain-intrinsic-size: auto 600px; }
```
4. Handlers de scroll/pointer ligeros: GSAP `quickTo`/`quickSetter` para updates por frame; nada de `getBoundingClientRect` dentro de loops sin cache.
5. Third-parties (analytics, chats, pixels): cargar tras interacción o con `strategy="lazyOnload"` (next/script). El chat-widget del cliente es el asesino silencioso del INP.
6. Premium: física/decode a Web Workers; el main thread es sagrado.

## Medición (el orden correcto)

1. **PageSpeed Insights** → sección "Real user data" (CrUX) = el veredicto.
2. **Lighthouse** (lab) para debuggear, con throttling móvil.
3. DevTools → Performance panel → grabar interacción para cazar INP.
4. En producción: `web-vitals` (npm) enviando a analytics:
```js
import { onLCP, onCLS, onINP } from "web-vitals";
onLCP(console.log); onCLS(console.log); onINP(console.log);
```

## Presupuesto por tier

| Recurso | LIGERO | PREMIUM |
|---|---|---|
| HTML | < 50 KB | < 80 KB |
| CSS | < 50 KB | < 80 KB |
| JS inicial (gzip) | **< 130 KB** (Next runtime + GSAP caben) | < 300 KB inicial; escena lazy |
| Imagen hero | < 150 KB | fallback < 150 KB; texturas lazy |
| Fuentes | < 100 KB (2 familias, subset) | < 120 KB |
| **Total primera carga** | **< 500 KB** | < 1 MB antes del canvas |
