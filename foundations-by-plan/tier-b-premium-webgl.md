# Tier B — Premium WebGL (el nivel award)

> Clientes $14,500+ y el sitio insignia de Culto. La experiencia ES el producto. Aquí se compite contra los mejores del mundo — y el jurado califica: **Design 40% · Usability 30% · Creativity 20% · Content 10%**.

## Stack

**Vite + Three.js crudo** (como Unseen/Bruno/Rogier — sin React) o Next+R3F si el proyecto lo pide · GSAP + Lenis · shaders iterados con Tweakpane/lil-gui · DRACO/KTX2 · deploy estático.
Referencias del vault: `cultoworld-kb/02-03`, `ui-architectures/bruno-singleton-core.md`, `performance/` (workers, DRACO, bundle splitting), `animations/lerp-scroll-inertia.md`.

## El proceso end-to-end (destilado de case studies de ganadores)

### 1. Concepto (la fase que decide el award)
- La dirección de arte se roba de FUERA de la web: cine, industrial, arquitectura, música. (Rogier de Boevé: Blade Runner 2049/Dune → paleta, UI y sound design. Locomotive: glifos de trenes → tipografía custom.)
- Moodboard de imágenes/video como único artefacto estable. Los ganadores individuales diseñan EN el navegador, no en Figma.
- **El copy se escribe antes de diseñar** (método Obys) y hay UN principio de diseño rector nombrable en una frase.

### 2. Prototipo del riesgo (antes de construir nada más)
El efecto WebGL central se prototipa AISLADO con parámetros en vivo (Tweakpane). "Con WebGL hay un ida y vuelta continuo — la idea nunca sale exactamente como la pensaste." Si el efecto no emociona en el playground, no va a emocionar en el sitio.

### 3. Construcción (orden verificado)
1. Layout estático + sistema tipográfico + responsive base (HTML semántico aunque haya canvas — el contenido real vive en DOM).
2. Integración del efecto + arquitectura (singleton, un ticker delta-time, módulos).
3. Motion de sistema: preloader, transiciones de página, reveals — mismo lenguaje de easing en TODO.
4. Micro-interacciones, sonido (con toggle), easter eggs, 404 creativa — al final.

### 4. Polish/QA para jurado (Usability = 30% del score)
- 60fps en una laptop promedio, no en tu GPU. Ocultar objetos WebGL fuera de vista.
- Degradación móvil/GPU débil + fallback estático BELLO (`03/lazy-load-fallbacks.md`).
- El preloader es lo primero que ve el jurado: pulido obsesivo.
- Cohesión de easing/timing en cada transición (una timeline maestra orquesta canvas + DOM).

### 5. Submission
- Awwwards: ~18 jurados, se eliminan los 3 scores más desviados; HM ~6.5 → SOTD ~7.0 → SOTM (voto público) → Annual. Ventana: 3 meses tras aprobación.
- Después del award: making-of pitcheado a Codrops (visibilidad global) + post técnico propio.

## Tiempos y equipo realistas

- Portfolio propio award-tier: 3-6 meses de noches/fines de 1-2 personas. La vía más barata al award.
- Proyecto cliente award-tier: 3-6 meses, equipo 3-8 (dirección de arte, diseño, 1-2 creative devs, motion/3D).
- Shaders finos (fluidos tipo Unseen): builds iterativos multi-día cada uno. No se prometen one-shot.

## El contexto competitivo (por qué vale la pena)

México tiene UN SOTD registrado en perfiles top (EMME & CO) y cero SOTM. El referente LATAM es basement.studio (Argentina): ~35 personas, clientes MrBeast/Vercel, y su jugada replicable — open source (scrollytelling, shader toolkit) → reputación dev global → clientes USA con tarifas USA. **Con 1 SOTD, Culto es top-2 de México.**

## Anti-patrones del tier B

Contenido de texto atrapado en canvas (invisible para Google Y para el jurado que evalúa Content 10%) · efectos sin concepto (spectacle genérico ya no gana en 2026: los jurados premian narrativa) · full DPR · N loops rAF · sin fallback · sound sin toggle · prometer fluid sim en una semana.
