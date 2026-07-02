# 🧠 knowledge0base — El Vault de CultoWorld

> Sistema completo de conocimiento para construir desde landings que convierten en <2s hasta experiencias WebGL de nivel Site of the Day. **67 archivos accionables**: código verificado, datos con fuente, y estrategia. Este README es el mapa; el plan de negocio vive en `_system/PLAN-MAESTRO.md`.

## 🎯 Instrucciones para la IA (System Prompt)

Al leer este repositorio asumes el rol de **Ingeniero Full-Stack de Élite y Diseñador Técnico de CultoWorld**. Protocolo estricto:

1. **Clasifica el tier ANTES de escribir código** (tabla abajo). Si no está claro, pregunta.
2. **Planifica** con `_system/planning-template.md` y sigue la guía del tier en `foundations-by-plan/`.
3. **Usa los patrones del vault**, no código de memoria: cada carpeta indica tier, costo de performance y cuándo NO usar cada técnica.
4. **Proyectos top con `hero: handoff`**: construye todo el sitio MENOS el hero — deja `HeroSlot` + `HERO-BRIEF.md` según `cultoworld-kb/01-animacion-gsap-ligera/hero-handoff.md`.
5. **Entrega solo con** `_system/delivery-checklist.md` en verde.
6. Regla de oro: ningún patrón es obligatorio en todos los casos. Las técnicas WebGL/premium NUNCA van en sitios que deben rankear o cargar rápido en móvil de gama media.

## 🧭 Tiers (el marco de decisión)

**Pregunta única: ¿este sitio vende por Google/velocidad, o vende impresionando?**

| Tier | Objetivo | Stack | Guía |
|---|---|---|---|
| **A — Landing** | Una acción (lead/venta). Velocidad y SEO mandan | Next.js + GSAP ligero. Sin WebGL | `foundations-by-plan/tier-a-landing.md` |
| **A+C — Sitio multipágina** | Negocio local completo que rankea | + páginas servicio+zona, blog opcional | `foundations-by-plan/tier-ac-multipage.md` |
| **B — Premium WebGL** | Impresionar, awards, $14,500+ | Vite + Three.js + GSAP + Lenis | `foundations-by-plan/tier-b-premium-webgl.md` |
| **D — App/Producto** | Funcionalidad, dashboards | React 19 + primitivas de `ui-architectures/` | claridad sobre espectáculo |

Ambos tiers llevan alma: la diferencia es el PESO de las técnicas, no la ausencia de animación. Detalle completo: `cultoworld-kb/00-sistema/tiers.md`.

## 📂 Mapa del repositorio

### `_system/` — El sistema operativo del estudio
`PLAN-MAESTRO.md` (negocio, ruta al award, mapa) · `planning-template.md` (llenar por proyecto) · `delivery-checklist.md` (QA de batalla) · `pro-claude-workflow.md`

### `cultoworld-kb/` — El knowledge base principal (por número)
- **00-sistema** — tiers, prohibiciones, presupuestos, checklist detallado
- **01-animacion-gsap-ligera** ⭐ — setup GSAP/useGSAP, scroll reveals + easings, micro-interacciones, parallax ligero, contadores/skeletons/transiciones, 6 patrones de hero por nicho, **hero-handoff** (el protocolo del hero hecho a mano)
- **02-animacion-premium** — Lenis, pin/scrub, scroll horizontal, scrollytelling, preloaders narrativos
- **03-threejs-webgl** — producción (color management, ticker, matcaps, DRACO), lazy-load/fallbacks, efectos award con shaders
- **04-color** — 9 paletas por nicho con psicología, WCAG, escalas OKLCH, extracción desde logo
- **05-tipografia** — pares por negocio, escalas fluid/clamp, next/font, subsetting
- **06-performance** — Core Web Vitals con fixes, imágenes/assets, GSAP sin sacrificar CWV
- **07-conversion-ux** — estructura por negocio, CTAs/forms/prueba social, **psicología de conversión** (Cialdini + sesgos + precio), copywriting es-MX, SEO local MX (GBP + schema), CRO avanzado con datos
- **08-referencias** — 18 técnicas de sitios award con matriz ligero↔premium, proceso end-to-end de brief a SOTD, mapa competitivo MX/LATAM
- **09-marketing** — funnel completo MX (CTWA/WhatsApp, nurture, retargeting, métricas), SEO 2026 + GEO/AEO (ser citado por las IAs)

### Capa de ingeniería inversa (Unseen / Bruno Simon / Messenger)
- **animations/** — lerp + ticker delta-time, reveals GSAP
- **3d-experiences/** — esfera interactiva R3F
- **ui-architectures/** — 12 módulos React (shells, singleton UI↔canvas, routing, overlays, primitivas)
- **performance/** — Web Workers, DRACO/WASM, bundle splitting
- **utils/** — data-attribute injection (la API declarativa de cada página)

### Fundaciones de diseño
- **design-foundations/** — sistema de tokens, color, tipografía, recursos, y el **catálogo de skins** (10 registradas, meta 100+, crece con cada proyecto)
- **foundations-by-plan/** — las 3 guías paso a paso por tier
- **references/** — React Bits (índice de componentes animados)
- **automation-backend/** — n8n, webhooks, WhatsApp

## ⚙️ Stack oficial

**Fábrica (A/A+C):** Next.js App Router + TS + Tailwind + GSAP (100% gratis, todos los plugins) + next/font + Vercel.
**Premium (B):** Vite + Three.js crudo + GSAP + Lenis; shaders con Tweakpane; DRACO/KTX2; TSL para código nuevo.
**Backend/automatización:** Python + n8n + WhatsApp API. Keys SIEMPRE en backend.
**Metas (percentil 75 real):** LCP <2.5s (fábrica <2s) · INP <200ms · CLS <0.1 · DPR ≤2.

## 📈 Los números que rigen todo (con fuente en los archivos)

- +1s de carga ≈ −7% conversiones; copy simple convierte 2× vs corporativo
- Sticky CTA móvil +12-27% · multi-step ~3× · click-to-WhatsApp ~3× leads (93% de MX usa WhatsApp)
- 46% juzga credibilidad por diseño visual en ~50ms → el hero decide (por eso hero-handoff)
- Jurado Awwwards: Design 40% · Usability 30% · Creativity 20% · Content 10%
- México tiene 1 solo SOTD en la historia → la cima está vacía y es nuestra

## 🛠️ Flujos rápidos

**Demo de fábrica** → tiers.md + skin del catálogo + estructura del nicho (07) + hero A-F (01) + checklist. 4 archivos y a producir.
**Sitio top** → lo anterior + `hero: handoff` → Pato ejecuta el hero con el HERO-BRIEF listo.
**Sitio insignia/award** → `tier-b-premium-webgl.md` de principio a fin + 08-referencias.
**Cliente con ads** → 09-marketing (message match, pixels/CAPI, UTMs, nurture).

## ⚖️ Límites honestos

Construir sigue siendo trabajo iterativo; los shaders finos toman días, no prompts. La persuasión de este vault es para alinear diseño con VERDAD — escasez real, prueba real. Dark patterns = multas + marca muerta. El vault crece como subproducto de construir: cada proyecto real deja una skin, un patrón o un error documentado.
