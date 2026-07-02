# Proceso end-to-end de un sitio award + el mapa competitivo MX/LATAM

> **Tier: PREMIUM** (el proceso) / **AMBOS** (la estrategia) · Investigado de case studies documentados: Codrops 2024-2026, Awwwards case studies, procesos publicados de Locomotive, Obys, 14islands, Rogier de Boevé, basement.studio. La versión operativa vive en `foundations-by-plan/tier-b-premium-webgl.md`; aquí está el detalle y las fuentes.

## Las 6 fases (lo que documentan los ganadores)

### 1. Concepto — donde se gana el award
- Dirección de arte robada de FUERA de la web: Rogier de Boevé (SOTD 2024) partió de Blade Runner 2049/Dune/Mad Max → paleta, UI y sound design. Locomotive (SOTM 2023) partió de glifos impresos en trenes → tipografía custom PP Locomotive New (con Pangram Pangram) en lugar de logo.
- Obys (Studio of the Year 2023): todo el equipo en el kickoff, un "principio de diseño" rector nombrable, y **copy antes de diseño**.
- Los ganadores individuales 2024-2026 tienden a saltarse Figma: moodboard + diseñar en el navegador (el look depende de shaders que Figma no representa).

### 2. Diseño
Figma = tablero de layout/tipo/sistema y control con cliente (Obys reporta por Slack con entregables). El motion NO se prototipa en Figma: referencias en video, CodePen/sandbox, o After Effects para transiciones clave. El handoff real en estudios chicos: diseñador y dev son la misma persona o están sentados juntos — conversación continua, no archivo.

### 3. Stack (casos verificados)
- Rogier de Boevé: Astro + Three.js + Alien.js + GSAP + Lenis + Howler.js (audio) + Vite. Sin CMS a propósito.
- Federico Pian (SOTD 2024): Nuxt + GSAP + TresJS.
- Ecosistema React (basement, 14islands): Next.js + R3F + GSAP + CMS headless (BaseHub/Sanity).
- Constantes: **GSAP + Lenis prácticamente obligatorios**; SSG/híbrido; CMS solo si el cliente lo necesita.

### 4. Orden de construcción
1. **El efecto central PRIMERO, aislado** (es el mayor riesgo): Rogier iteró su grid de cubos con alphas por fila+cubo antes de tener sitio. Controles en vivo (Tweakpane/lil-gui/Leva).
2. Layout estático + sistema tipográfico + responsive.
3. Motion de sistema: transiciones, reveals, scroll (Locomotive: "letter shuffle" + pixel lazy-loader — detalles chicos que suman cohesión).
4. Micro-interacciones, sonido, easter eggs al final (la L.I.S.A. de Locomotive: falso asistente con Text-to-Speech).

### 5. QA para jurado
Performance real (ocultar objetos fuera de vista, degradación móvil), responsive completo, cohesión de easing, preloader pulido, hover/focus, 404 creativa, sonido con toggle.

### 6. Submission Awwwards
- **Design 40% · Usability 30% · Creativity 20% · Content 10%** (criterios oficiales).
- ~18 jurados mínimo; se eliminan los 3 scores más desviados; votación 5 días. HM ~6.5 → SOTD ~7.0 → SOTM (voto público) → Annual. Elegibilidad: 3 meses tras aprobación. Submission tiene fee.

## Tiempos/equipo reales

- Portfolio personal SOTD: 3-6 meses de noches/fines de UNA persona (la vía barata al award).
- Cliente award-tier: 3-6 meses, equipo 3-8. (Cartier Yearbook de 14islands: 32 artículos, 3 idiomas, WebGL → SOTD+FWA+CSSDA.)
- Locomotive: <30 personas, +100 awards desde 2018.

## El mapa MX/LATAM (julio 2026)

**México — la cima está vacía:** perfiles más premiados: EMME & CO (5 HM, **1 SOTD** — el único), RNR Creative Studio (4 HM), Montalvo Wire (2 HM), David Langarica (1 HM). CERO SOTM mexicanos. El jurado es global y ciego al país: la barrera es de ejecución, no de geografía. **Con 1 SOTD, Culto es top-2 nacional; con 1 SOTM, el primero.**

**El modelo a copiar — basement.studio (Argentina):** ~35 personas, clientes MrBeast/Vercel/Ranboo, 2 Webbys + SOTDs + nominaciones anuales. Su jugada: R&D → open source (scrollytelling, shader toolkit, contribuciones a Geist de Vercel) + BaseHub (CMS propio escindido como empresa) → reputación dev global → clientes USA con tarifas USA desde Buenos Aires. **El arbitraje aplica idéntico desde Monterrey.**

Otros: Aerolab (AR, producto/UX), /nk.studio (AR, emergente). Ojo: Hello Monday NO es LATAM (danesa/NY, adquirida por DEPT) — pero es benchmark: 126 FWAs, 60 Awwwards.

## La jugada de Culto (resumen ejecutable)

1. Sitio propio award-tier (3-6 meses, noches). 2. Submission. 3. Making-of a Codrops (aceptan pitches). 4. 1-2 micro-librerías open source. 5. Clientes USA.

## Fuentes

- tympanus.net/codrops/2024/07/26/case-study-rogier-de-boeve-portfolio-2024/
- tympanus.net/codrops/2024/10/02/case-study-federico-pian-portfolio-2024/
- tympanus.net/codrops/2025/11/24/…14islands… · tympanus.net/codrops/tag/case-study/
- awwwards.com/case-study-reinventing-locomotive-r.html
- awwwards.com/about-evaluation/ (criterios y pesos oficiales)
- awwwards.com/winner-list/Mexico · awwwards.com/basementstudio · github.com/basementstudio
- fionazeerak.com/case-study/obys-agency/ · basehub.com/basementstudio
