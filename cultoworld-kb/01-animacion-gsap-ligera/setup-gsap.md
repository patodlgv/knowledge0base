# Setup de GSAP (vanilla y React/Next)

> **Tier: AMBOS** · **Costo de perf: BAJO** (core + ScrollTrigger ≈ 45 KB min+gzip)
> Fuente: docs oficiales gsap.com (v3.15). GSAP es **100% gratis desde abril 2025**, incluidos TODOS los plugins antes de pago (SplitText, ScrollSmoother, MorphSVG, DrawSVG…). Sin tokens ni licencias.

## Instalación

```bash
npm install gsap          # vanilla y React
npm install @gsap/react   # solo React/Next: el hook useGSAP
```

CDN (para demos HTML sueltos):
```html
<script src="https://cdn.jsdelivr.net/npm/gsap@3/dist/gsap.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3/dist/ScrollTrigger.min.js"></script>
```

## Vanilla — registro y patrón base

```js
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);   // SIEMPRE registrar antes de usar
```

## React/Next — useGSAP (el patrón oficial, úsalo SIEMPRE)

`useGSAP()` reemplaza a `useEffect` y limpia automáticamente todos los tweens y ScrollTriggers al desmontar (evita animaciones duplicadas con React StrictMode). Fuente: gsap.com/resources/React.

```tsx
"use client";                          // obligatorio en App Router
import { useRef } from "react";
import gsap from "gsap";
import { useGSAP } from "@gsap/react";
import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(useGSAP, ScrollTrigger);

export default function Section() {
  const container = useRef<HTMLDivElement>(null);

  useGSAP(() => {
    // todo lo creado aquí se revierte solo al desmontar
    gsap.from(".card", { y: 40, opacity: 0, stagger: 0.08 });
  }, { scope: container });            // scope: los selectores solo buscan dentro del container

  return <div ref={container}>…</div>;
}
```

### Animaciones disparadas por eventos (click, hover) → contextSafe

Lo creado DESPUÉS del mount (en un handler) no se limpia solo. Envuélvelo:

```tsx
const { contextSafe } = useGSAP({ scope: container });

const onClick = contextSafe(() => {
  gsap.to(".box", { rotation: 180 });
});
```

## Evitar FOUC (flash de contenido sin estilo/animación)

Con `gsap.from()` el contenido es visible aunque JS falle — preferirlo sobre esconder con CSS. Si necesitas ocultar antes de animar, hazlo condicionado a JS:

```html
<script>document.documentElement.classList.add("js")</script>
<style>.js .reveal { visibility: hidden; }</style>
```
```js
gsap.set(".reveal", { visibility: "visible" });  // dentro de la timeline, antes del from
```

## Reduced motion (no negociable, va en TODO proyecto)

```js
// gsap.matchMedia — el patrón oficial
const mm = gsap.matchMedia();

mm.add("(prefers-reduced-motion: no-preference)", () => {
  // TODAS las animaciones van aquí dentro
  gsap.from(".hero-line", { yPercent: 100, stagger: 0.12 });
});
// si el usuario pide menos movimiento, nada se anima y todo queda en estado final
```

`gsap.matchMedia()` también sirve para animaciones distintas por breakpoint:

```js
mm.add({
  isMobile: "(max-width: 767px)",
  isDesktop: "(min-width: 768px)",
  reduce: "(prefers-reduced-motion: reduce)"
}, (ctx) => {
  const { isMobile, reduce } = ctx.conditions;
  if (reduce) return;                 // nada de movimiento
  gsap.from(".card", { y: isMobile ? 20 : 60, opacity: 0, stagger: 0.08 });
});
```

## Cuándo SÍ / Cuándo NO

- ✅ SIEMPRE en ambos tiers. GSAP es el estándar y su costo es bajo.
- ✅ Cargar solo los plugins que uses (cada plugin se importa por separado).
- ❌ NO mezclar con Framer Motion/Motion One en el mismo proyecto (peso redundante).
- ❌ NO animar en `useEffect` pelón en React — usa `useGSAP` o tendrás fugas y dobles animaciones.
