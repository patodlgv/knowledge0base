# 🧭 PLAN MAESTRO — CultoWorld

> El documento rector: qué es el negocio, cómo se usa este vault, y la ruta al nivel award. Todo lo demás en el repo sirve a este plan.

## El negocio en una línea

Fábrica de sitios ligeros que convierten (el pan de cada día) + capacidad premium WebGL award-tier (el diferenciador que impresiona y sube precios).

## Los dos motores

| Motor | Producto | Precio | Volumen | KB que lo alimenta |
|---|---|---|---|---|
| **Fábrica** | Landings/sitios tier A-C, <2s, con alma GSAP | accesible | cientos | `cultoworld-kb/01, 04-07` + `foundations-by-plan/tier-a*` |
| **Premium** | Experiencias WebGL cinematográficas | $14,500+ | pocos, escogidos | `cultoworld-kb/02-03` + `ui-architectures/` + `performance/` + `tier-b-premium-webgl.md` |

El sitio propio de Culto es del motor premium: es el portafolio que vende ambos.

## Mapa del vault (dónde está cada cosa)

1. **Decidir tier** → `cultoworld-kb/00-sistema/tiers.md` (regla de 1 pregunta)
2. **Planear proyecto** → `_system/planning-template.md`
3. **Elegir skin** → `design-foundations/skins-catalog.md` (si no hay, crearla con `color-system.md` + `typography-system.md` y registrarla)
4. **Estructura que convierte** → `cultoworld-kb/07-conversion-ux/` (+ copywriting-mx, seo-local-mx)
5. **Construir** → `foundations-by-plan/tier-*.md` como guía + snippets de `cultoworld-kb/01-03` y `animations/`, `ui-architectures/`
6. **Entregar** → `_system/delivery-checklist.md`

## La ruta al nivel award (estrategia verificada, julio 2026)

**El dato:** en los perfiles más premiados de Awwwards para México hay UN solo SOTD (EMME & CO) y CERO SOTM. Argentina tiene a basement.studio (SOTDs, nominaciones a Studio of the Year). México está vacío en la cima → **con 1 SOTD, CultoWorld es top-2 nacional instantáneo.**

**Criterios del jurado Awwwards (oficiales):** Design 40% · Usability 30% · Creativity 20% · Content 10%. Usability pesa 30%: un sitio bello que va a 20fps en la laptop del jurado NO gana. El score se forma con ~18 jurados, eliminando los 3 votos más desviados. Escalera: Honorable Mention (~6.5) → SOTD (~7.0) → SOTM (voto público) → Annual.

**La jugada (probada por los ganadores individuales):**
1. Portfolio/sitio propio award-tier — 3-6 meses de trabajo nocturno de 1-2 personas, costo casi cero. Es la vía más barata a un award.
2. Submission a Awwwards (Design+Usability = 70% del score).
3. Making-of pitcheado a Codrops (aceptan case studies; visibilidad global masiva).
4. 1-2 librerías open source pequeñas (el modelo basement: scrollytelling, shader toolkit → reputación dev global → clientes de EE.UU. con tarifas de EE.UU. desde LATAM).

## El proceso end-to-end de un sitio award (destilado de case studies)

1. **Concepto:** la dirección de arte se roba de FUERA de la web (cine, trenes, arquitectura — Rogier de Boevé partió de Blade Runner 2049; Locomotive de glifos ferroviarios). El copy se escribe ANTES de diseñar (método Obys).
2. **Diseño:** Figma para layout/tipo/sistema; el motion NO se prototipa en Figma — referencias en video + prototipos directos en código.
3. **Riesgo primero:** el efecto WebGL central se prototipa AISLADO antes de construir el sitio (con Tweakpane/lil-gui para iterar a ojo).
4. **Orden:** layout estático → motion de sistema (transiciones, reveals) → micro-interacciones/sonido/easter eggs al final.
5. **QA:** degradación móvil/GPU débil, cohesión de easing, preloader pulido (lo primero que ve el jurado), 404 creativa, toggle de sonido.

## Reglas de oro del negocio

1. La fábrica paga las cuentas; el premium construye la marca. No confundir los motores.
2. Cada proyecto real alimenta el vault (skin nueva, patrón nuevo, error documentado). El vault crece construyendo, no acumulando teoría.
3. Velocidad ES conversión (+1s ≈ −7%): la fábrica jamás sacrifica <2s por un efecto.
4. Todo demo debe verse VIVO (GSAP ligero siempre) — un demo estático no vende.
5. Vender hacia afuera cuando el nivel lo permita: el arbitraje LATAM→USA es la jugada de basement y aplica idéntico desde México.
