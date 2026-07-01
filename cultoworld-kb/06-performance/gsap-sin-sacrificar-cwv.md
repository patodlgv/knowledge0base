# Animación GSAP rica SIN sacrificar Core Web Vitals

> **Tier: LIGERO** · La tesis de la fábrica: un sitio puede sentirse vivo Y cargar en <2s. Estas son las reglas que lo hacen compatible.

## 1. GSAP no bloquea el LCP si se carga bien

GSAP core + ScrollTrigger ≈ 45 KB gzip — cabe en el presupuesto. Lo que importa es CUÁNDO corre:

- **Next.js:** GSAP vive en client components hoja; el HTML del hero llega por SSR y pinta antes de que GSAP hidrate. El orden natural ya es correcto.
- **Vanilla:** `<script type="module">` (deferred por default) al final; jamás GSAP en `<head>` bloqueante.

## 2. La regla del elemento LCP

```js
// ✅ BIEN: el h1 está pintado (SSR), GSAP lo anima apenas hidrata
gsap.from(".hero-line", { yPercent: 110, duration: 0.9, stagger: 0.12 });

// ❌ MAL: CSS lo esconde hasta que JS llegue → LCP tardío + página rota sin JS
/* .hero-line { opacity: 0 } en el stylesheet */
```

Si necesitas estado inicial oculto (para evitar el "flash" de 1 frame), hazlo SOLO bajo el fold, o condicionado a JS (`html.js .reveal { visibility: hidden }` — ver `01/setup-gsap.md`).

## 3. Solo transform y opacity

GPU-composited, no disparan layout ni paint:

```js
// ✅ x, y, xPercent, yPercent, scale, rotation, opacity, clipPath (composited en moderno)
// ❌ width, height, top, left, margin, padding, fontSize, boxShadow por frame
```

Excepción práctica: `clip-path` para máscaras está bien; `filter: blur()` animado por scroll NO (paint caro).

## 4. Animaciones que no cuentan como CLS

CLS solo penaliza movimientos INESPERADOS de layout. Los transforms no mueven layout → los reveals GSAP bien hechos dan **CLS 0**. Cuidado con:
- Animar la ALTURA de un contenedor al cargar (eso sí es CLS) → reservar el espacio final y animar el contenido con transform adentro.
- `gsap.from(el, { height: 0 })` en algo above-the-fold: prohibido.

## 5. INP: los patrones baratos por frame

```js
// updates continuos (cursor, parallax de mouse): quickTo, no gsap.to por evento
const xTo = gsap.quickTo(".cursor", "x", { duration: 0.4, ease: "power3" });
const yTo = gsap.quickTo(".cursor", "y", { duration: 0.4, ease: "power3" });
addEventListener("mousemove", (e) => { xTo(e.clientX); yTo(e.clientY); });
```

- Un solo `gsap.ticker` para todo lo lerp-eado (nunca N loops rAF).
- `ScrollTrigger.batch` en grids grandes (menos triggers = menos trabajo por scroll).
- No leer layout (`getBoundingClientRect`, `offsetWidth`) dentro de handlers por frame — cachear en resize.

## 6. Carga diferida de plugins pesados

SplitText solo se necesita para el hero → cargarlo no bloquea nada si el texto ya está visible:

```js
// vanilla — split diferido con fuentes listas (evita split con métrica equivocada)
document.fonts.ready.then(async () => {
  const { SplitText } = await import("gsap/SplitText");
  gsap.registerPlugin(SplitText);
  // … reveal del hero
});
```

## 7. CSS scroll-driven animations: reveals con CERO JS (progressive enhancement)

Para demos ultra-ligeros, los reveals básicos ya se pueden hacer nativos (corren fuera del main thread; Chrome/Edge/Firefox, llegando a Safari):

```css
@supports (animation-timeline: view()) {
  .reveal-css {
    animation: enter linear both;
    animation-timeline: view();
    animation-range: entry 0% entry 60%;
  }
  @keyframes enter { from { opacity: 0; transform: translateY(40px); } }
}
```

Estrategia fábrica: CSS nativo para reveals simples + GSAP para lo coreografiado. Donde no hay soporte, el contenido simplemente aparece (correcto).

## 8. Test de humo (antes de entregar cada demo)

1. DevTools → Performance → CPU 4x + Network Slow 4G → recargar. ¿El h1 aparece < 2s?
2. Scrollear rápido de arriba a abajo: ¿algún frame > 50ms? (barras rojas)
3. Lighthouse móvil: Performance ≥ 90 con las animaciones ACTIVAS.
4. Apagar JS: ¿el sitio se lee completo? (SSR + `.from()` lo garantizan)

## Resumen operativo

| Quiero | Hago | Nunca |
|---|---|---|
| Hero animado | SSR + `gsap.from` al hidratar | `opacity:0` en CSS al LCP |
| Reveals de secciones | ScrollTrigger sin scrub, `batch` en grids | un trigger por elemento |
| Parallax | transform + scrub | background-attachment, filter por frame |
| Cursor/mouse FX | quickTo + un ticker | gsap.to en cada mousemove |
| Split de texto | SplitText tras `fonts.ready` | split antes de fuentes (métrica rota) |
