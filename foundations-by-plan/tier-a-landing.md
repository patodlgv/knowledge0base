# Tier A — Landing de conversión (la máquina de leads)

> El producto estrella de la fábrica. Una página, un objetivo, <2s. Se ve viva e irresistible, y cada decisión sirve a UNA acción.

## Stack

Next.js (App Router) + TS + Tailwind + GSAP (core + ScrollTrigger + SplitText, todos gratis) + next/font + Vercel. **CERO WebGL, cero Lenis, cero video pesado.**

## Receta paso a paso

1. **Planear** con `_system/planning-template.md` — headline escrito ANTES de diseñar.
2. **Skin** del catálogo (`design-foundations/skins-catalog.md`) → tokens en `globals.css`.
3. **Estructura** del nicho (`cultoworld-kb/07-conversion-ux/estructura-por-negocio.md`). Landing de campaña: SIN nav.
4. **Hero** con patrón A–F (`cultoworld-kb/01-animacion-gsap-ligera/heros-ligeros.md`) + entrada coreografiada.
5. **Copy** con fórmulas es-MX (`cultoworld-kb/07-conversion-ux/copywriting-mx.md`): nivel secundaria, tuteo, "sin plazos forzosos".
6. **Movimiento**: reveals estándar + micro-interacciones + 1 elemento vivo. Presupuesto cerrado — nada de pin/scrub.
7. **CTA**: WhatsApp prellenado + sticky móvil (patrón en `ctas-formularios-prueba-social.md`).
8. **SEO local** si aplica (`seo-local-mx.md`): JSON-LD + GBP.
9. **Entregar** con `_system/delivery-checklist.md`.

## Los números que rigen este tier (benchmarks 2024-2026)

- Mediana de conversión 6.6%; top 10% ≥11.45% (3-5× la mediana de su industria). La brecha es estructura.
- Copy nivel 5º-7º grado convierte **11.1%** vs 5.3% del copy "profesional" — el doble. 250-725 palabras total.
- Sticky CTA móvil: +12-27% consistente (casos hasta +252% en completado de pedido).
- Multi-step form: 13.85% vs 4.53% single (~3×) — empezar por la pregunta fácil, contacto AL FINAL. Excepción: si solo pides WhatsApp, single gana.
- Click-to-WhatsApp: 93% de internautas MX lo usan; 7 de 10 prefieren mensaje sobre llamada; ~3× más leads que funnel a formulario.
- +0.1s de velocidad ≈ +8-10% conversión; 53% abandona en móvil si tarda >3s.
- 18% abandona por desconfianza: dirección física, fotos reales, reseñas visibles SIEMPRE.

## Anti-patrones del tier A

Nav en landing de campaña · form >5 campos · CTA "Enviar" · urgencia falsa (contadores que se reinician) · texto denso corporativo · hero de stock genérico · animación que retrasa el contenido · cualquier cosa de la lista negra de `06-performance/imagenes-y-assets.md`.

## Definición de éxito

LCP <2s móvil · lead llega a WhatsApp/inbox comprobado · Lighthouse ≥90 · el cliente lo ve en SU teléfono y dice "se ve caro".
