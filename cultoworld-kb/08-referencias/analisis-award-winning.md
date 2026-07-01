# Análisis de sitios award-winning: técnicas concretas y reproducibles

> **Tier: se indica por técnica** · Investigado julio 2026: ganadores Awwwards/FWA/CSSDA 2024–2026, case studies de Codrops, y análisis de Unseen/Lusion/Bruno Simon/Obys/Locomotive. Fuentes al final.

## El stack ganador (patrón dominante en SOTD)

**Framework (Nuxt/Next/Astro) + GSAP (ScrollTrigger, SplitText, Flip) + Lenis + WebGL (Three.js / R3F / OGL) + Barba.js o View Transitions.**

Casos verificados: Gen-02 Portfolio (SOTD 2025, WebGPU custom), KOKI-KIKO (Nuxt+GSAP+Three), Tux Creative (Nuxt+GSAP+OGL), **Nod Coding Bootcamp** (SOTD + Developer Award con GSAP+Lottie+Matter.js — **sin WebGL 3D**: se puede ganar sin Three.js), 21 TSI (OGL), Lusion v3, Bruno Simon (Three+física+matcaps), Obys (tipografía+grid+scroll pesado, mayormente sin WebGL).

## Matriz: qué se adapta al tier ligero y qué no

### ✅ Adaptables a LIGERO tal cual (GSAP/CSS, ya en este KB)

| Técnica | Se ve en | Dónde está el código |
|---|---|---|
| Texto revelado por líneas con máscara | Obys, casi todo SOTD tipográfico | `01/scroll-reveals.md` §4 |
| Marquee infinito reactivo al scroll | Locomotive, Obys | `02/scrolltrigger-avanzado.md` §5 (versión no-reactiva es 100% ligera) |
| Cursor custom con lerp + estados | Locomotive, Studio Freight | `01/micro-interacciones.md` §7 |
| Botones magnéticos + elastic | Locomotive y derivados | `01/micro-interacciones.md` §4 |
| Menú con roll de caracteres | Obys, Locomotive | `01/heros-ligeros.md` patrón F |
| Parallax multicapa "pesado" | Obys (scroll "smooth and heavy") | `01/parallax-ligero.md` |
| Reveal de imagen con clip-path + scale | aproximación del reveal WebGL | `01/scroll-reveals.md` §5 |
| Kinetic type con variable fonts | Typography Principles (Obys) | abajo §técnica destacada |
| Cambio de tema por sección | decenas de SOTD | `01/contadores-skeletons-transiciones.md` §3 |

### ⚠️ Adaptables con APROXIMACIÓN (el original es WebGL)

| Original (premium) | Aproximación ligera | Fidelidad |
|---|---|---|
| Distorsión de imagen por velocidad de scroll (shader) | skewY por `getVelocity()` — `02/scrolltrigger-avanzado.md` §4 | ~80% |
| Hover flowmap (ondula bajo el cursor) | SVG `feTurbulence`+`feDisplacementMap` al hover (solo desktop, vigilar perf) | ~50% |
| Galería con reveal shader | clip-path + scale interna | ~80% |
| Grid deformable GPGPU | escala/traslación de celdas por distancia al cursor (DOM) | ~40% |
| Mundo 3D | "3D falso": perspective + tilt + parallax | otra cosa, pero vende |

### ❌ Exclusivas de PREMIUM

Fluido on-drag (Unseen), partículas GPGPU (Lusion), mundo navegable con física (Bruno), texto-en-WebGL distorsionable (Troika), WebGPU/TSL compute. → `03-threejs-webgl/efectos-award-winning.md`.

## Técnicas destacadas con implementación

### Kinetic type con variable fonts (LIGERO — carácter enorme, peso mínimo)

Titular cuyo peso/ancho respira con el scroll (Typography Principles de Obys, +1M visitas):

```css
h1.kinetic { font-family: "Fraunces", serif; font-variation-settings: "wght" 300; }
```
```js
gsap.to(".kinetic", {
  fontVariationSettings: '"wght" 700', ease: "none",
  scrollTrigger: { trigger: ".kinetic", start: "top 80%", end: "top 30%", scrub: true }
});
```
Costo: la fuente variable ya cargada; el ajuste re-rasteriza texto (OK en titulares, no en párrafos largos).

### Física 2D como lenguaje (Nod — la prueba de que se gana sin 3D)

Matter.js: tags/stickers que caen, chocan y se arrastran. Sync por frame `body.position` → `transform`. ~87 KB — **plan intermedio**, no fábrica base; para UN momento memorable (la sección de servicios donde los tags caen al viewport).

### Page transitions seamless (INTERMEDIO/PREMIUM)

Barba.js + GSAP: fetch del HTML siguiente en background, hooks `leave`/`enter` devuelven timelines. El nivel dios: elemento compartido con **Flip** (la thumbnail del proyecto SE CONVIERTE en el hero de la página siguiente):

```js
// patrón Flip para elemento compartido
const state = Flip.getState(".project-thumb");
// … Barba reemplaza el DOM; la imagen ahora es .project-hero
Flip.from(state, { targets: ".project-hero", duration: 1, ease: "power3.inOut", absolute: true });
```
En premium con canvas: el canvas es FIJO y persiste entre páginas; solo se remapean los planes (patrón de la galería Codrops GSAP+Three+Astro+Barba, 2026).
Alternativa nativa ligera: View Transitions API (`01/contadores-skeletons-transiciones.md` §4).

### Preloader narrativo (PREMIUM)

Contador 0–100 con progreso real de assets + wipe → `02/scroll-narratives.md`. En ligero NO va (nada que precargar).

## Tendencias 2025–2026 (dónde apuntar)

1. **WebGPU production-ready** (Safari 26 completó el soporte; Three r171+ `WebGPURenderer`). Ya hay SOTD 100% WebGPU (Gen-02). Compute 10–100× para partículas/física.
2. **TSL**: shaders en JS que compilan a WGSL + GLSL con fallback — la vía para código shader nuevo del sitio insignia (guía: Maxime Heckel, "Field Guide to TSL and WebGPU").
3. **CSS scroll-driven animations** (`animation-timeline: view()/scroll()`): reveals/parallax/progress SIN JS, fuera del main thread → regalo directo para la fábrica (`06/gsap-sin-sacrificar-cwv.md` §7).
4. **View Transitions cross-document**: mata la necesidad de Barba en sitios MPA simples.
5. **OGL (~30 KB)** como tier intermedio de WebGL: UN shader contenido sin cargar Three completo (21 TSI, Tux).
6. **Física juguetona** (Matter 2D, Rapier 3D) como diferenciador táctil.
7. **Concepto > efectos**: los jurados 2026 premian narrativa/tipografía/arte-dirección sobre spectacle genérico. Un demo de la fábrica con GRAN tipografía + 3 movimientos bien elegidos compite contra sitios con 10 shaders sin idea.

## Cómo estudiar un sitio nuevo (metodología F12)

1. **Consola**: `window.gsap && gsap.version`, `typeof THREE`, `window.lenis`, `__NUXT__`/`__NEXT_DATA__` → stack en 30 segundos.
2. **Network**: filtrar JS grande (three, gsap, lenis en nombres de chunks); WOFF2 → identificar fuentes (o extensión WhatFont).
3. **Elements**: buscar `<canvas>` (WebGL o no), `data-` attributes que delatan librerías (`data-scroll`, `data-cursor`), `:root {}` para robar el sistema de tokens (contar valores repetidos = el sistema real).
4. **Performance**: grabar 5s de scroll → ver qué animan (layers verdes = composited bien hecho).
5. **Fold durable al KB**: si una técnica se repite en 3+ sitios ganadores, merece archivo aquí.

## Fuentes

- Codrops case studies: tympanus.net/codrops/tag/case-study/ — en particular: Nod Coding Bootcamp (nov 2024), Stefan Vitasović 2025, 21 TSI (jul 2025), Obys (mar/may 2026), galería GSAP+Three+Astro+Barba (feb 2026), "How to Animate WebGL Shaders with GSAP" (oct 2025), experimento Rapier de Unseen (sep 2025)
- gsap.com/docs (SplitText masks, ScrollTrigger tips & mistakes)
- github.com/darkroomengineering/lenis · lenis.darkroom.engineering
- Bruno Simon: caso de estudio en Medium + github.com/brunosimon/folio-2025 (MIT)
- awwwards.com/sites/lusion-v3 · annuals.awwwards.com/categories/site-of-the-year
- typographyprinciples.obys.agency
- blog.maximeheckel.com/posts/field-guide-to-tsl-and-webgpu
- github.com/luruke/awesome-casestudy (lista curada de case studies WebGL)
