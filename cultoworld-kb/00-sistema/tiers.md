# Sistema de Tiers de CultoWorld

> **Tier: AMBOS** · Este archivo define las reglas del juego. Léelo (o pásalo a Claude Code) antes de cualquier proyecto.

## Los dos tiers — ambos con alma

La diferencia entre tiers NO es "con animación vs sin animación". Es el **peso** de las técnicas. Un demo del tier ligero sin movimiento no vende; un sitio premium que carga en 8s tampoco.

| | TIER LIGERO (fábrica de demos) | TIER PREMIUM ($14,500+ / sitio insignia) |
|---|---|---|
| **Objetivo** | Convertir, rankear, escalar a cientos | Impresionar, ganar el contrato, awards |
| **Presupuesto de carga** | **< 2s LCP** en Android gama media + 4G | < 4s con preloader elegante; 60fps una vez cargado |
| **Stack** | Next.js (App Router) + TS + Tailwind + GSAP | Vite + Three.js crudo + GSAP + Lenis |
| **JS total (comprimido)** | ~130 KB budget (GSAP core+ScrollTrigger ≈ 45 KB) | Sin límite duro, pero assets comprimidos (DRACO/KTX2) |
| **SEO** | No negociable (SSR/SSG, contenido en DOM) | Se sacrifica a propósito (como Unseen) |
| **Fuentes** | 2 familias máx, woff2, next/font | Igual + posible texto en WebGL (Troika) |

## Qué SÍ lleva el tier ligero (esto es lo que lo hace irresistible)

- GSAP core + ScrollTrigger (reveals, stagger, timelines de sección)
- Micro-interacciones en hover/focus (GSAP o CSS puro)
- Parallax ligero **solo con transform**
- Contadores animados, skeleton loaders, transiciones de sección
- Heros con carácter: split-text reveal, máscaras, grids asimétricos, "3D falso" con CSS perspective
- SplitText (ya es gratis) para reveals de titular línea por línea

## Qué está PROHIBIDO en el tier ligero (mata el LCP en móvil)

- ❌ Three.js / WebGL / canvas pesado (~150 KB antes de dibujar un pixel)
- ❌ Scroll-scrub de secuencias de frames/imágenes (estilo Apple)
- ❌ Video de fondo pesado (> 1 MB) o autoplay sin poster
- ❌ ScrollSmoother/Lenis en la versión base (opcional en plan 2 si el budget aguanta)
- ❌ Librerías de animación adicionales (Framer Motion + GSAP = redundante)

## Qué SÍ lleva el premium (y solo el premium)

- Three.js: matcaps, partículas, shaders GLSL custom
- ScrollTrigger con `pin` + `scrub` para narrativas cinemáticas
- Lenis (smooth scroll), transiciones de página tipo SPA
- Texto en WebGL (troika-three-text) donde el efecto lo pida
- Web Workers + WASM para trabajo pesado

## Regla de decisión (1 pregunta)

**"¿Este sitio vende por Google/velocidad o vende impresionando?"**
- Por Google/velocidad → LIGERO. Cuando dudes, elige ligero: el 3D se puede fingir con CSS+GSAP (ver `01-animacion-gsap-ligera/parallax-ligero.md` §3D falso).
- Impresionando a quien ya llegó → PREMIUM.

## Etiquetas usadas en todo el KB

Cada técnica lleva este encabezado:

```
Tier: LIGERO | PREMIUM | AMBOS
Costo de perf: BAJO (<5 KB, sin impacto LCP) | MEDIO (afecta INP/fps si se abusa) | ALTO (afecta LCP, solo premium)
```

## Datos duros para vender el tier ligero al cliente

- Cada +1s de carga ≈ −7% conversiones; una página de 1s convierte ~3× más que una de 5s.
- < 2s de carga ≈ 9% bounce; > 5s ≈ 38% bounce.
- Solo ~56% de los sitios pasan los tres Core Web Vitals — pasarlos ya es diferenciador.
