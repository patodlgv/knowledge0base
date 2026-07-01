# Contadores animados, skeleton loaders y transiciones de sección/página

> **Tier: LIGERO** (aplican en ambos) · **Costo de perf: BAJO**

## 1. Contador animado (stats que suben al entrar en viewport)

```html
<span class="counter" data-target="1479">0</span>
```
```js
gsap.utils.toArray(".counter").forEach((el) => {
  const target = parseFloat(el.dataset.target);
  const obj = { val: 0 };
  gsap.to(obj, {
    val: target, duration: 1.6, ease: "power2.out",
    scrollTrigger: { trigger: el, start: "top 85%" },
    onUpdate: () => { el.textContent = Math.round(obj.val).toLocaleString("es-MX"); }
  });
});
```

Con snap (versión más simple, oficial GSAP): `gsap.to(obj, { val: target, snap: { val: 1 }, ... })`.
Evitar CLS: reserva el ancho final con `min-width: Xch` en el span.

## 2. Skeleton loader (CSS puro — no gastes JS aquí)

```css
.skeleton {
  position: relative; overflow: hidden;
  background: color-mix(in srgb, var(--ink) 8%, transparent);
  border-radius: 4px; min-height: 1em;
}
.skeleton::after {
  content: ""; position: absolute; inset: 0;
  background: linear-gradient(90deg, transparent, rgb(255 255 255 / .35), transparent);
  transform: translateX(-100%);
  animation: shimmer 1.4s infinite;
}
@keyframes shimmer { to { transform: translateX(100%); } }
@media (prefers-reduced-motion: reduce) { .skeleton::after { animation: none; } }
```

Regla anti-CLS: el skeleton debe tener EXACTAMENTE las dimensiones del contenido final (`aspect-ratio` para imágenes, `height` en líneas de texto).

## 3. Transición entre secciones (separadores con carácter)

Panel de color que barre al cambiar de sección (cortina):

```js
gsap.utils.toArray(".section-divider").forEach((div) => {
  gsap.fromTo(div,
    { scaleY: 0, transformOrigin: "top" },
    { scaleY: 1, ease: "none",
      scrollTrigger: { trigger: div, start: "top bottom", end: "top 30%", scrub: true } });
});
```

O el clásico cambio de tema (fondo claro → oscuro) por sección:

```js
gsap.utils.toArray("[data-theme-trigger]").forEach((sec) => {
  ScrollTrigger.create({
    trigger: sec, start: "top 50%", end: "bottom 50%",
    onToggle: (self) => self.isActive &&
      document.body.setAttribute("data-theme", sec.dataset.themeTrigger)
  });
});
```
```css
body { transition: background-color .6s ease, color .6s ease; }
body[data-theme="dark"] { background: var(--navy); color: var(--ivory); }
```
Este truco hace sentir el sitio "diseñado" con costo casi cero.

## 4. Transición de página

### Next.js (tier ligero) — overlay de salida/entrada

Para la fábrica: una cortina simple con GSAP en un template compartido. Mantenerlo sutil (300–500ms) — más largo castiga la navegación:

```tsx
// app/template.tsx — se re-monta en cada navegación
"use client";
import { useRef } from "react";
import gsap from "gsap";
import { useGSAP } from "@gsap/react";

export default function Template({ children }: { children: React.ReactNode }) {
  const ref = useRef<HTMLDivElement>(null);
  useGSAP(() => {
    gsap.from(ref.current, { opacity: 0, y: 16, duration: 0.45, ease: "power2.out" });
  });
  return <div ref={ref}>{children}</div>;
}
```

### View Transitions API (CSS casi puro, progressive enhancement)

Para sitios MPA estáticos; si el navegador no la soporta, simplemente no hay transición (cero costo):

```css
@view-transition { navigation: auto; }
::view-transition-old(root) { animation: fade-out .3s ease both; }
::view-transition-new(root) { animation: fade-in .35s ease both; }
@keyframes fade-out { to { opacity: 0; } }
@keyframes fade-in  { from { opacity: 0; } }
```

### Premium — transiciones cinemáticas

Cortinas full-screen, morphs de imagen entre páginas (FLIP), preloaders narrativos → ver `02-animacion-premium/`. En premium se usa Barba.js o router custom + GSAP.

## Cuándo SÍ / Cuándo NO

- ✅ Contadores en secciones de prueba social/stats (números específicos convierten).
- ✅ Skeletons SOLO donde hay datos async reales. Contenido estático no lleva skeleton — llévalo directo en el HTML.
- ✅ Cambio de tema por sección: firma visual gratis para demos.
- ❌ NO preloader/pantalla de carga en el tier ligero (un sitio de <2s no necesita pantalla de espera; solo agrega espera).
- ❌ NO transiciones de página >500ms en sitios de conversión.
