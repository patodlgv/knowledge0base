# CultoWorld — Knowledge Base de Desarrollo Web Premium

Repositorio de conocimiento ACCIONABLE para producir sitios de alta calidad en dos tiers. Todo el código viene de documentación oficial (GSAP v3.15, Three.js r170+, web.dev, MDN) o de patrones validados en proyectos/case studies — con fuente indicada.

## Cómo usarlo con Claude Code

- **Contexto por tarea:** pasa el archivo del tema + `00-sistema/tiers.md` (las reglas del juego).
- **Nuevo demo de la fábrica:** `tiers.md` + `01-animacion-gsap-ligera/heros-ligeros.md` + `07-conversion-ux/estructura-por-negocio.md` + la paleta y par tipográfico del nicho.
- **Sitio premium:** `tiers.md` + carpetas 02 y 03 completas.
- **Antes de entregar:** `00-sistema/checklist-pre-entrega.md` como criterio de terminado.

## Convenciones

Cada archivo declara: **Tier** (LIGERO/PREMIUM/AMBOS), **Costo de perf** (BAJO/MEDIO/ALTO), y secciones **Cuándo SÍ / Cuándo NO**. Los snippets son copiables tal cual; en React envolver GSAP en `useGSAP` (ver `01/setup-gsap.md`).

## Índice

### 00-sistema
- `tiers.md` — Definición de ambos tiers, prohibiciones, presupuestos, regla de decisión
- `checklist-pre-entrega.md` — QA de performance/a11y/SEO/conversión

### 01-animacion-gsap-ligera ⭐ (el alma de la fábrica)
- `setup-gsap.md` — Instalación, useGSAP, matchMedia, reduced-motion, FOUC
- `scroll-reveals.md` — Reveals, stagger, batch, timelines de sección, SplitText, easings de la casa
- `micro-interacciones.md` — Botones (CSS y GSAP), magnetic, tilt 3D falso, menú toggle, cursor
- `parallax-ligero.md` — Capas por scroll y mouse, marquee scrubbed, solo transform
- `contadores-skeletons-transiciones.md` — Contadores, skeletons CSS, tema por sección, transiciones de página
- `heros-ligeros.md` — 6 patrones de hero por tipo de negocio con asignación por nicho

### 02-animacion-premium
- `scrolltrigger-avanzado.md` — Lenis+ScrollTrigger, pin/scrub, scroll horizontal, stacking, skew por velocidad
- `scroll-narratives.md` — Scrollytelling, preloaders narrativos, cámara sobre curva, pacing

### 03-threejs-webgl (SOLO premium)
- `fundamentos-produccion.md` — Escena, color management (r152+), ticker delta-time, matcaps, disposal, DRACO/KTX2, WebGPU/TSL
- `lazy-load-fallbacks.md` — Import dinámico, detección de capacidad, degradación, fallbacks
- `efectos-award-winning.md` — Partículas, distorsión por scroll, flowmap, displacement, con aproximaciones ligeras

### 04-color
- `paletas-por-nicho.md` — 9 nichos con psicología del color y tokens listos
- `accesibilidad-y-escalas.md` — WCAG, escalas OKLCH, color-mix, extracción desde logo/foto

### 05-tipografia
- `pares-por-negocio.md` — Pares por nicho (Google Fonts + Fontshare + fundidoras)
- `escalas-fluid-y-carga.md` — Escala modular, clamp(), next/font, subsetting, fallback métrico

### 06-performance
- `core-web-vitals.md` — LCP/CLS/INP con fixes y presupuesto por tier
- `imagenes-y-assets.md` — AVIF/WebP, next/image, sharp pipeline, code splitting, lista negra móvil
- `gsap-sin-sacrificar-cwv.md` — La tesis de la fábrica: animación rica + <2s (reglas operativas)

### 07-conversion-ux
- `estructura-por-negocio.md` — Secuencias de secciones por nicho con benchmarks
- `ctas-formularios-prueba-social.md` — WhatsApp CTA, forms accesibles, testimonios, antes/después

### 08-referencias
- `analisis-award-winning.md` — Técnicas de Awwwards/Unseen/Lusion/Obys/Bruno con matriz ligero↔premium, tendencias 2025-26, metodología F12

## Hechos clave verificados (julio 2026)

- **GSAP es 100% gratis** (todos los plugins) desde abril 2025; versión actual 3.15; hook oficial React: `useGSAP` de `@gsap/react`.
- **Three.js r152+**: output sRGB por default; texturas de color requieren `colorSpace = SRGBColorSpace`; ACES filmic para look cinematográfico.
- **INP** reemplazó a FID; es la métrica más fallada (~43% de sitios).
- **CSS scroll-driven animations** y **View Transitions** ya son utilizables como progressive enhancement.

## Cómo crecer este KB

Una técnica entra cuando: (1) se vio en 3+ sitios ganadores o resolvió un proyecto real, (2) tiene código probado o de fuente oficial, (3) queda etiquetada con tier + costo + cuándo no usarla. Principios sobre copias.

## Añadidos (julio 2026, ronda 2)

### 01 · `hero-handoff.md` — protocolo para heros hechos a mano: la fábrica construye todo menos el hero y deja HeroSlot + HERO-BRIEF.md con propuesta de storytelling lista. Para proyectos top.
### 07 · `psicologia-conversion.md` — Cialdini, sesgos conductuales, psicología del precio, confianza (Stanford), mitos desmentidos. El "por qué" de todo 07.
### 07 · `copywriting-mx.md` / `seo-local-mx.md` / `cro-avanzado.md` — copy es-MX, GBP+schema, multiplicadores con datos.
### 09-marketing (nueva) · `funnel-y-ads.md` — funnel MX completo (CTWA, nurture, retargeting, métricas, infra de página). `seo-completo-2026.md` — clusters, técnico, GEO/AEO (ser citado por las IAs).
### 08 · `proceso-end-to-end-award.md` — de brief a SOTD + mapa competitivo MX/LATAM.
