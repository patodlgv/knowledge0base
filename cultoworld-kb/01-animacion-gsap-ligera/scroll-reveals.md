# Scroll reveals con ScrollTrigger (el pan de cada día del tier ligero)

> **Tier: LIGERO** (aplican también en premium) · **Costo de perf: BAJO** — solo transform/opacity, ScrollTrigger sin `scrub` ni `pin` es casi gratis.
> Fuente: docs oficiales ScrollTrigger + premium-web skill (patrones validados en sitios de Culto).

## Los easings de la casa

| Uso | Easing | Sensación |
|---|---|---|
| Entradas (reveals, fades) | `power3.out` | Rápido → aterriza suave. El default premium. |
| Entradas más discretas | `power2.out` | Menos drama, para cards y listas |
| Hero / momento clave | `expo.out` | Muy explosivo al inicio, elegante |
| Elementos juguetones (iconos, badges) | `back.out(1.4)` | Pequeño rebote de personalidad |
| Salidas | `power2.in` | Acelera al salir |
| Scrub / parallax | `none` | El scroll ES el easing |

Regla: **entradas con `.out`, salidas con `.in`, scrub sin easing.** Duraciones: 0.6–0.9s entradas; micro-interacciones 0.2–0.4s.

## 1. Reveal básico al entrar al viewport

```js
gsap.from(".reveal", {
  y: 40, opacity: 0, duration: 0.8, ease: "power3.out",
  scrollTrigger: {
    trigger: ".reveal",
    start: "top 80%",        // cuando el top del elemento cruza el 80% del viewport
    toggleActions: "play none none none"  // solo una vez, no re-anima al subir
  }
});
```

`toggleActions: "play none none reverse"` si quieres que se esconda al scrollear arriba (más "vivo", úsalo con moderación).

## 2. Cascada de cards (stagger) — cada card se dispara sola

Para grids largos usa `ScrollTrigger.batch` (mejor que un solo trigger para todo el grid):

```js
ScrollTrigger.batch(".card", {
  start: "top 85%",
  onEnter: (batch) => gsap.to(batch, {
    y: 0, opacity: 1, stagger: 0.08, duration: 0.7, ease: "power2.out", overwrite: true
  })
});
gsap.set(".card", { y: 40, opacity: 0 });  // estado inicial
```

Grid corto (≤6 items) — versión simple:

```js
gsap.from(".card", {
  y: 40, opacity: 0, stagger: 0.08, duration: 0.7, ease: "power2.out",
  scrollTrigger: { trigger: ".grid", start: "top 80%" }
});
```

Variantes de stagger con carácter:
```js
stagger: { each: 0.08, from: "center" }   // desde el centro hacia afuera
stagger: { each: 0.06, from: "random" }   // aleatorio (grids grandes)
stagger: { amount: 0.5 }                   // total repartido entre todos
```

## 3. Timeline de sección (título → texto → CTA, coreografiado)

Una timeline por sección = una sola coreografía, un solo trigger:

```js
const tl = gsap.timeline({
  defaults: { ease: "power3.out", duration: 0.7 },
  scrollTrigger: { trigger: ".section", start: "top 75%" }
});
tl.from(".section h2",   { y: 50, opacity: 0 })
  .from(".section p",    { y: 30, opacity: 0 }, "-=0.4")   // empalma 0.4s
  .from(".section .cta", { y: 20, opacity: 0 }, "-=0.3")
  .from(".section img",  { scale: 1.06, opacity: 0, duration: 1 }, "<");  // "<" = junto con el anterior
```

El **position parameter** (`"-=0.4"`, `"<"`, `"+=0.2"`) es lo que hace ver la animación diseñada y no genérica.

## 4. Reveal de titular línea por línea (el look premium)

Con SplitText (gratis desde 3.13, con accesibilidad integrada):

```js
import { SplitText } from "gsap/SplitText";
gsap.registerPlugin(SplitText);

const split = SplitText.create(".hero h1", { type: "lines", mask: "lines" });
// mask:"lines" envuelve cada línea en un wrapper con overflow:hidden → efecto cortina
gsap.from(split.lines, {
  yPercent: 110, duration: 0.9, ease: "power3.out", stagger: 0.12
});
```

Sin SplitText (manual, cero dependencias extra):

```html
<h1><span class="line-mask"><span class="hero-line">Marcas que la gente</span></span>
    <span class="line-mask"><span class="hero-line">sigue.</span></span></h1>
<style>.line-mask { display: block; overflow: hidden; }</style>
```
```js
gsap.from(".hero-line", { yPercent: 110, duration: 0.9, ease: "power3.out", stagger: 0.12 });
```

## 5. Imagen con máscara-cortina (reveal editorial)

```html
<div class="img-reveal"><img src="…" alt="…"></div>
<style>.img-reveal { overflow: hidden; } .img-reveal img { display: block; }</style>
```
```js
const tl = gsap.timeline({ scrollTrigger: { trigger: ".img-reveal", start: "top 80%" } });
tl.from(".img-reveal",     { clipPath: "inset(0 100% 0 0)", duration: 1, ease: "power3.inOut" })
  .from(".img-reveal img", { scale: 1.3, duration: 1, ease: "power3.out" }, "<");
```

## Versión React de todo lo anterior

Envolver en `useGSAP` con `scope` (ver `setup-gsap.md`); el código interno es idéntico:

```tsx
useGSAP(() => {
  gsap.from(".card", { y: 40, opacity: 0, stagger: 0.08,
    scrollTrigger: { trigger: ".grid", start: "top 80%" } });
}, { scope: container });
```

## Reglas de performance para mantener <2s

1. Reveals NO afectan LCP si el hero content es visible de inicio o usa `.from()` (el HTML ya está pintado).
2. NUNCA `opacity: 0` en CSS al elemento LCP (el `<h1>` del hero) — Google no lo cuenta como pintado. Anima con `.from()` inmediato tras hidratar, o solo el resto de la página.
3. Máximo ~1 ScrollTrigger por sección (usa timelines, no un trigger por elemento). `batch` para grids.
4. `will-change: transform` solo si detectas jank, y quítalo al terminar (GSAP lo maneja bien solo).

## Cuándo SÍ / Cuándo NO

- ✅ Toda sección debajo del fold de todo sitio ligero: es lo que lo hace sentirse vivo.
- ✅ `toggleActions` simple (play una vez) para conversión — el visitante no debe esperar la animación 2 veces.
- ❌ NO revelar el contenido crítico del hero con retraso largo (>0.3s tras load).
- ❌ NO usar `scrub`/`pin` aquí — eso es tier premium (ver `02-animacion-premium/`).
