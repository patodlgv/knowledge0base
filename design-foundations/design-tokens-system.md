
# 🎨 Sistema de Design Tokens — Arquitectura de 3 Capas (Cimiento Universal)

> La pieza fundacional del sistema de producción de Culto. UNA sola arquitectura
> sirve para TODOS los tipos de sitio (landing, blog, e-commerce, marca/WebGL).
> No se cambia la estructura por cliente — se cambian los VALORES. Esto es lo que
> permite "re-vestir" un sitio en minutos y entregar en < 3 días.
>
> Estándar usado por IBM Carbon, Adobe Spectrum, Airbnb, Uber. Adopción 84% entre
> profesionales (2026, encuesta ~300, spec DTCG estable). No es moda, es el método.

---

## 🏛️ La Arquitectura: 3 Niveles

La misma para landing, blog, tienda o marca. Cambian los valores, no la estructura.

### Nivel 1 — PRIMITIVOS (valores crudos, sin contexto)
Describen QUÉ es el valor, no dónde se usa. Es la "paleta de pinturas" completa.
```css
:root {
  /* Color — escalas completas */
  --navy-900:#0A1128; --navy-700:#1B2A4A; --navy-500:#2E4374;
  --gold-600:#A8864B; --gold-500:#C5A46D; --gold-300:#E0CBA0;
  --ivory-100:#F6F4F1; --ivory-50:#FBFAF8;
  --gray-900:#111; --gray-500:#6B7280; --gray-100:#F3F4F6;

  /* Espaciado — base 8 */
  --space-1:4px; --space-2:8px; --space-3:12px; --space-4:16px;
  --space-6:24px; --space-8:32px; --space-12:48px; --space-16:64px;

  /* Tipografía — familias y escala modular (ratio 1.25) */
  --font-display:'Playfair Display', Georgia, serif;
  --font-body:'Inter', system-ui, sans-serif;
  --text-sm:0.8rem; --text-base:1rem; --text-lg:1.25rem;
  --text-xl:1.563rem; --text-2xl:1.953rem; --text-3xl:2.441rem; --text-4xl:3.052rem;

  /* Forma y profundidad */
  --radius-sm:6px; --radius-md:12px; --radius-full:9999px;
  --shadow-sm:0 1px 2px rgba(0,0,0,.05);
  --shadow-md:0 4px 6px -1px rgba(0,0,0,.1);
  --shadow-lg:0 10px 25px -3px rgba(0,0,0,.15);
}
```

### Nivel 2 — SEMÁNTICOS / ALIAS (intención, no apariencia)
Referencian a los primitivos. **ESTA es la capa que se intercambia por cliente.**
Describen el rol, no el color. Aquí vive la "piel" de la marca.
```css
:root {
  --color-bg:           var(--navy-900);
  --color-surface:      var(--navy-700);
  --color-text:         var(--ivory-100);
  --color-text-muted:   var(--gray-500);
  --color-brand:        var(--gold-500);   /* el acento / CTA */
  --color-brand-hover:  var(--gold-600);
  --color-focus-ring:   var(--gold-300);

  --font-heading:       var(--font-display);
  --font-paragraph:     var(--font-body);
}
```
> 🔑 **Para un cliente nuevo:** solo reescribes ESTA capa (y agregas sus primitivos).
> El sitio entero cambia de piel sin tocar componentes. Eso es la producción en serie.

### Nivel 3 — COMPONENTE (específico de cada pieza)
```css
.button--primary {
  background: var(--color-brand);
  color: var(--navy-900);
  padding: var(--space-3) var(--space-6);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-md);
}
.button--primary:hover { background: var(--color-brand-hover); }
.button--primary:focus-visible { outline: 2px solid var(--color-focus-ring); }
```

---

## ✅ Por qué esto cubre TODOS los casos (la duda del "¿y si es un blog?")

No hay 5 estructuras. Hay 1 estructura y N "skins" (capa semántica):
- **Landing/Conversión:** mismo sistema, acento fuerte en `--color-brand` para CTAs.
- **Blog/Editorial:** mismo sistema, prioriza escala tipográfica (`--text-*`) y
  `--color-text` legible; acento discreto.
- **E-commerce:** mismo sistema + tokens de componente extra (precio, badge, stock).
- **Marca/WebGL (Culto):** mismo sistema; el canvas vive aparte, el DOM usa tokens.

La diferencia entre un blog y una tienda NO es la arquitectura de tokens — es qué
componentes montas encima y qué valores semánticos priorizas.

---

## 🌗 Bonus gratis: modos (claro/oscuro) sin reescribir nada
Como la capa semántica es la única que se toca, un modo oscuro es re-mapear alias:
```css
[data-theme="light"] {
  --color-bg: var(--ivory-50);
  --color-text: var(--navy-900);
  --color-surface: #fff;
}
```
> **Swap test:** cambia el modo y revisa que nada se rompa (contraste, bordes
> invisibles, texto que desaparece). Si algo falla, hay un hueco en el mapeo.

---

## 🔗 FUENTES GRATUITAS CURADAS (no inventar paletas — referenciar a los expertos)

> En vez de fabricar 100 paletas de memoria (relleno), el método pro es jalar de
> fuentes ya curadas por expertos y guardar SOLO las que se prueban en proyectos.

### Paletas de color
- **Coolors.co** — generador + miles de paletas curadas, exporta a CSS/tokens.
- **Adobe Color** — armonías por teoría del color + verificador de contraste WCAG.
- **ColorHunt.co** — paletas curadas por la comunidad, filtrables por mood.
- **Tailwind default palette** — escalas 50-950 listas, base sólida de primitivos.
- **Realtime Colors (realtimecolors.com)** — prueba una paleta sobre un sitio real
  al instante. Ideal para validar antes de comprometerse.

### Tipografía (pairings curados)
- **Google Fonts** — gratis, todas con soporte de tildes/ñ verificable. Base.
- **Fontpair.co** — pairings display+body ya combinados por expertos.
- **Typewolf** — qué fuentes usan los sitios premiados (inspiración de alto nivel).
- **Fontjoy** — generador de pairings por IA.

### Verificación (no negociable para "calidad sin importar el precio")
- **WebAIM Contrast Checker** — que TODO texto pase WCAG AA, hasta en el plan barato.

---

## 📋 Protocolo de uso (cómo se vuelve "producción en serie")

1. Cliente nuevo → defines su paleta (de Coolors/Adobe) y su pairing (de Fontpair).
2. Verificas contraste (WebAIM) — calidad va en todos los planes, baratos incluidos.
3. Escribes SOLO sus primitivos nuevos + reescribes la capa semántica (Nivel 2).
4. El cimiento del plan (landing/multi/premium) ya usa los tokens → se reviste solo.
5. Si una combinación queda excelente y probada → se guarda como "skin" reutilizable
   aquí, etiquetada por tipo de negocio. Así el catálogo crece con calidad real,
   no con relleno.

---

## 🗃️ Catálogo de Skins probadas (crece con cada proyecto)
> Se llena SOLO con combinaciones usadas y validadas en sitios reales.

| Skin | Negocio | bg | brand | display | body | Estado |
|---|---|---|---|---|---|---|
| **Culto** | Estudio/Agencia premium | navy-900 | gold-500 | Playfair Display | Inter | ✅ en uso |
| *(siguiente)* | | | | | | pendiente |

> Meta realista: llegar a 100 skins clasificadas por industria CONSTRUYENDO, no de
> un tiro. Cada cliente suma 1-2 skins validadas. En 2-3 meses tienes decenas reales.
