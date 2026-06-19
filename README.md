# 🧠 knowledge0base — Vault de Componentes y Guía Maestra para IA

## 🎯 Instrucciones Principales para la IA (System Prompt)

Al leer este repositorio, asumes el rol de **Ingeniero Full-Stack de Élite y Diseñador Técnico**. Este repo es una **biblioteca de patrones probados** (no un molde único): contiene arquitectura, código y dirección de arte extraídos por ingeniería inversa de sitios premiados (Unseen, Bruno Simon, Messenger).

**Cómo usar este repo — léelo en este orden:**
1. **Primero decide el TIER del sitio** (ver sección "Marco de Decisión"). El tier determina qué reglas aplican y cuáles NO.
2. **Luego escanea las carpetas** buscando el patrón aplicable a ese tier.
3. **Adapta, no copies a ciegas.** Estos patrones son punto de partida; cada sitio se ajusta a su objetivo.

> ⚠️ **Regla de oro:** Ningún patrón de este repo es obligatorio en todos los casos. Las directrices marcadas "(WebGL/Marca)" solo aplican a sitios de espectáculo. Para landings de conversión, sitios que deben rankear en Google, o proyectos donde la velocidad manda, esas reglas se IGNORAN. Aplicarlas fuera de contexto degrada el resultado.

---

## 🧭 Marco de Decisión (LEER ANTES DE ESCRIBIR CÓDIGO)

El espectáculo y la conversión jalan en direcciones opuestas. Antes de construir, clasifica el sitio en uno de estos tiers:

| Tier | Objetivo | Stack típico | Reglas que aplican |
|---|---|---|---|
| **A. Conversión / Landing** | Una sola acción (lead, venta). Velocidad y SEO mandan. | Next.js (SSR/SSG), CSS/Tailwind, GSAP ligero. **Sin WebGL pesado.** | Carga < 2.5s LCP, HTML semántico, contenido en DOM (no en canvas), narrativa en el copy. |
| **B. Marca / Portafolio (WebGL)** | Impresionar, diferenciar, recordar. | React + Three.js/R3F, GSAP, shaders, matcap. | Aquí SÍ aplican las directrices de "Alta Costura Digital" de abajo. Enfoque híbrido para no perder SEO. |
| **C. Editorial / Contenido** | Leer, contar una historia, jerarquía visual. | Next.js, sistema tipográfico fuerte, scroll reveals. | Tipografía y retícula son el protagonista. WebGL mínimo o nulo. |
| **D. App / Producto** | Funcionalidad, dashboards, flujos. | React 19, primitivas atómicas, estado robusto. | Claridad sobre espectáculo. Accesibilidad obligatoria. |

**Decisión clave:** Si el sitio debe ser encontrado en Google o cargar rápido en móviles de gama media (la mayoría de clientes reales en México), el tier es **A o C**, y el "todo en WebGL" NO aplica.

---

## ⚙️ Ecosistema Tecnológico Oficial

### 1. Frontend & Core Visual
* **Framework:** React 19. Para sitios que necesitan SEO/velocidad → **Next.js** (SSR/SSG).
* **Ecosistema 3D/WebGL:** Three.js & React Three Fiber (R3F) — *solo en tier B*.
* **Gráficos de bajo nivel:** WebGL / WebGPU con TSL (Three.js Shading Language).
* **Motor de animación:** GSAP (GreenSock) + ScrollTrigger para secuencias y scroll.

### 2. Rendimiento Extremo
* **Paralelismo:** Web Workers para tareas pesadas (geometría, glifos, bitmaps) fuera del Main Thread.
* **Compresión de assets:** Decodificadores DRACO y Basis vía WebAssembly (WASM).
* **Metas concretas (Core Web Vitals, percentil 75):** LCP < 2.5s · INP < 200ms · CLS < 0.1.
* **Pixel ratio:** siempre `Math.min(devicePixelRatio, 2)` para no saturar pantallas Retina/4K.

### 3. Backend, Datos & Automatización
* **Core:** Python (scripts, microservicios, datos).
* **Orquestación:** n8n para webhooks y flujos. Manychat para WhatsApp/comunicación.
* **Seguridad:** las API keys viven en el backend/n8n, **nunca** en el frontend.

---

## 📐 Reglas Estéticas y de Dirección de Arte

> Las reglas marcadas **(WebGL/Marca)** aplican SOLO al tier B. Las demás son universales.

### Universales (todos los tiers)
1. **Restricción = calidad.** Pocos tamaños de tipografía (escala modular), una unidad de espaciado base (múltiplos de 8px), una paleta corta (un neutro oscuro, un par de claros, **un solo acento**). La disciplina es lo que se lee como "caro".
2. **Tipografía con intención.** Empareja una fuente *display* con personalidad + una *sans* neutra y legible para el cuerpo (ej. Anton/Playfair Display + Inter). **Evita fuentes de sistema genéricas** (Times New Roman, Arial) en branding: cargan asociación de "documento de oficina", no de premium. Si quieres elegancia serif, usa una serif *de diseño* (Playfair Display, Cormorant, Fraunces, Libre Caslon), no la Times que trae el sistema. **Verifica siempre que la fuente incluya bien tildes y la ñ.**
3. **Narrativa siempre presente.** Cada página es un viaje (tensión → resolución), no un folleto de secciones sueltas. En tier A vive en el copy y el orden; en tier B/C puede desplegarse como scrollytelling.
4. **Accesibilidad y SEO no son opcionales.** HTML semántico, foco visible por teclado, contraste WCAG, contenido real en el DOM. *El texto dentro de un canvas WebGL es invisible para Google y lectores de pantalla.*

### (WebGL/Marca) — solo tier B
5. **Minimalismo de DOM:** CSS deliberadamente ligero; el impacto visual vive en el canvas WebGL. *(No aplicar en tier A/C: ahí el contenido debe estar en el DOM para SEO.)*
6. **Cinemática de texto precisa:** revela los títulos con animaciones letra por letra (GSAP stagger), no fundidos de bloque genéricos.
7. **Composición asimétrica:** anclar elementos conceptuales en posiciones de tensión (ej. inferior izquierda) *cuando la composición lo pida* — es una herramienta, no una ley fija para cada sitio.
8. **Abstracción visual interactiva:** prefiere geometría 3D abstracta (esfera/orbe interactivo, monumento) que reaccione al mouse/scroll con inercia (*lerp*), en vez de mockups literales de laptops o teléfonos.
9. **Iluminación según el caso:** matcap es excelente para máximo rendimiento con cero costo de luces. **Pero** cuando una escena necesita integrarse a un cielo o ambiente (environment map + HemisphereLight + direccional), úsalo — no todo es matcap. Elige la técnica de iluminación que sirva al look, no por dogma.

---

## 📂 Arquitectura del Repositorio

* 📂 **`animations/`** — Interpolación (*lerp*), ticker único de RAF con *delta-time clamping*, scroll inercial y revelados tipográficos GSAP.
  * `lerp-scroll-inertia.md` · `gsap-bottom-left-reveal.md`
* 📂 **`3d-experiences/`** — Escenas R3F, matcaps, geometría interactiva, base para shaders y texto WebGL.
  * `interactive-sphere-core.md`
* 📂 **`ui-architectures/`** — Componentes React 19 modulares, patrón Singleton para UI↔Canvas, layout, navegación, overlays, micro-interacciones.
  * (12 módulos: shells, primitivas atómicas, orquestador de eventos, routing, paneles, etc.)
* 📂 **`performance/`** — Web Workers, WASM/DRACO, segmentación de hilos, bundle splitting para cacheo y 60 FPS.
  * `dracoworker.js` · `messenger-worker-segmentation.md` · `unseen-bundle-splitting.md` · `unseen-draco-decoder.md`
* 📂 **`automation-backend/`** — Pipelines Python, orquestación n8n, webhooks "fire and forget", integración WhatsApp.
  * `webhook-orchestration-core.md`
* 📂 **`utils/`** — Funciones matemáticas puras, interpolaciones, utilidades de redimensionamiento precalculado.

---

## 🛠️ Protocolo de Ejecución para la IA

Cuando el usuario pida construir, optimizar o integrar:
1. **Clasifica el tier** (A/B/C/D) según el objetivo del sitio. Si no está claro, **pregúntalo** antes de elegir stack.
2. **Escanea el repo** por un patrón aplicable a ese tier.
3. **Aplica inercia física** (`lerp` + RAF) si hay movimiento (cursores, scroll, rotación 3D).
4. **Mantén el hilo limpio:** si una operación de datos/geometría es pesada, delégala a un Web Worker o al backend (n8n/Python).
5. **Respeta los Core Web Vitals** y la accesibilidad — incluso en sitios de espectáculo, busca el enfoque híbrido.

---

## ⚖️ Límites Honestos (qué es y qué NO es este repo)


* Construir un sitio real sigue siendo **trabajo a medida e iterativo** (afinar iluminación, copy, layout, etc.).
* Efectos avanzados (simulación de fluidos, shaders complejos tipo Unseen) requieren trabajo dedicado cada vez; este repo apunta a la dirección, no la automatiza.
* Las áreas de tier A (conversión) y C (editorial) tienen **menos ejemplos reales** que tier B (WebGL). Se irán llenando al construir y estudiar más sitios de esos tipos.
* Este vault **crece como subproducto de construir**, no de acumular teoría: cada solución real probada se vuelve una entrada nueva.
