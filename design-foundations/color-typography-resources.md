
# 🎨 Librería de Recursos: Color & Tipografía (Fuentes Curadas Reales)

> Referencias a las MEJORES fuentes gratuitas de paletas y pairings que usan los
> profesionales (investigadas, no inventadas). El método: no fabricar combinaciones
> de memoria, sino jalar de estos catálogos curados por expertos, validar contraste,
> y guardar en el "Catálogo de Skins" (ver design-tokens-system.md) las que se
> prueban en proyectos reales. Así el almacén crece con calidad, no con relleno.

---

## 🌈 COLOR — Las mejores fuentes (por uso)

### Generación rápida y exploración
- **Coolors.co** — "la navaja suiza del color". Genera con barra espaciadora,
  bloquea las que te gustan, modo IA que sugiere extensiones armónicas. Exporta a
  CSS/tokens. *El caballo de batalla.*
- **Khroma** — aprende tus gustos (eliges 50 colores) y genera combinaciones
  personalizadas con machine learning. Mejor para *explorar* identidad nueva.
- **Huemint** — IA que genera paletas para un USO específico (web, logo, etc.).
  Le das contexto y optimiza para eso. Produce combinaciones inesperadas pero
  efectivas que los generadores por reglas se pierden.

### Curadas por humanos / comunidad (cuando quieres algo ya validado)
- **ColorHunt.co** — colección curada y votada por la comunidad. Filtrable por mood.
  Curación humana, no algoritmo. *Para arrancar rápido con algo probado.*
- **Adobe Color** — sección "Explore": navega paletas hechas por usuarios por
  popularidad/recencia. Color Wheel con teoría del color (armonías). Pro-grade.

### Estructura de sistema (primitivos para tokens)
- **Open Color** — esquema open-source optimizado para UI. 13 tonos × 10 sombras,
  pensado para fondos, bordes y texto (no ilustración). *Ideal como Nivel 1 de tokens.*
- **Tailwind palette** — escalas 50-950 listas. Base sólida de primitivos.
- **ColorKit** — rampas de sombras custom (mezcla dos colores, genera intermedios).

### Validación en contexto (el paso que casi nadie da)
- **Realtime Colors (realtimecolors.com)** — aplica tu paleta a un sitio real al
  instante: fondos, texto, botones, bordes. Evita el error de "se ve bien en
  swatch, horrible en UI". *Úsalo SIEMPRE antes de comprometerte.*
- **Brandcolors** — códigos oficiales de marcas grandes (Google, Spotify, Slack).
  Para análisis competitivo y alinear con marcas del cliente.

### Accesibilidad (NO negociable — calidad en todos los planes, baratos incluidos)
- **Stark** / **WebAIM Contrast Checker** — que TODO texto pase WCAG AA.

> **Flujo de color recomendado (de Muzli):** marca nueva → Khroma para explorar →
> Coolors para refinar → Stark para accesibilidad. Sistema de diseño → Open Color
> para estructura → ColorKit para rampas → Stark para cumplimiento.

---

## ✍️ TIPOGRAFÍA — Las mejores fuentes de pairings

### Catálogos curados (lo mejor de lo mejor)
- **Typewolf** — referencia #1 de la industria. Las 40 mejores Google Fonts
  curadas + qué fuentes usan los sitios premiados "in the wild". Marca con * las
  aptas para cuerpo de texto. *Para inspiración de alto nivel y ver uso real.*
- **Fontpairings.net** — pairings con IA, preview en tiempo real, exporta CSS y a
  Figma. Filtra por mood, industria o estilo. *El más práctico para producción.*
- **Fontpair.co** / **Fontjoy** — pairings display+body ya combinados por expertos.

### Cheat sheets con CONTEXTO (por qué funcionan, no solo cuáles)
- **Mantlr — 40 Google Font Pairings** — cada pairing con explicación de POR QUÉ
  funciona + CSS de implementación. La lección clave: el pairing no es "dos fuentes
  que se ven bien juntas", es crear jerarquía clara — la fuente de título carga
  PERSONALIDAD, la de cuerpo carga LEGIBILIDAD.
- **LandingPageFlow — 20+ pairings** — probados para armonía, jerarquía y
  legibilidad. Organizados por tipo: e-commerce, blogs, landing pages.
- **The Brief — 30 pairings** — incluye combos como Playfair Display (título) que
  funciona en tamaños grandes "como contando una historia".

### La regla de oro de los que entregan rápido
> "Si veo una landing de SaaS más usando Inter o Roboto por defecto sin
> customizar, voy a gritar." La tipografía es el activo de MAYOR apalancamiento
> para que un producto bootstrapeado parezca de Serie A. El secreto: emparejar una
> fuente de cuerpo gratis y ubicua (Inter, DM Sans, Manrope) con una fuente de
> título DISTINTIVA y con carácter. — Precode (entregan UX en 5 días)

### Tendencia 2025-2026
- **Bricolage Grotesque** (títulos) + **Inter** (cuerpo) — el tipo de letra
  trending del momento, con personalidad distintiva.

---

## 🇲🇽 Nota para el mercado mexicano (NO negociable)
**Verifica SIEMPRE soporte completo de tildes (á é í ó ú) y la ñ** antes de usar
cualquier fuente. Google Fonts permite previsualizar con texto custom — escribe
"Diseño Español Niño Acción" y confirma que renderiza bien. Una fuente preciosa
que rompe la ñ es inservible para un cliente mexicano.

---

## 🔁 Cómo esto alimenta el "almacén" (producción en serie)
1. Por cada proyecto/industria, eliges de estas fuentes (no inventas).
2. Validas contraste (WCAG) y soporte de ñ/tildes.
3. Pruebas en contexto (Realtime Colors).
4. La combinación que queda excelente → se registra como SKIN en
   `design-tokens-system.md`, etiquetada por industria.
5. En semanas tienes decenas de skins REALES y probadas, clasificadas — el almacén
   que querías, pero construido con calidad en vez de relleno de un tiro.

> Meta: catálogo amplio (rumbo a 100+) clasificado por industria, lleno con combos
> validados en proyectos. Estas fuentes son el surtidor; el catálogo es tu curaduría.
