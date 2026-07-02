# Tier A+C — Sitio multipágina (negocio local completo / editorial)

> Home + servicios + zonas + contacto (+ blog si hay contenido real). Debe rankear en Google Y sentirse de estudio. Es el tier A con arquitectura de sitio.

## Stack

El mismo del tier A (Next.js + GSAP). Se AGREGA: páginas de servicio+zona, blog opcional (MDX o CMS ligero), transición de página sutil (template.tsx, 300-450ms). Plan 2/intermedio puede sumar Lenis + UNA sección pinneada máximo + 1 momento memorable (física Matter.js o un shader OGL contenido) — solo si el budget de <2.5s aguanta.

## Arquitectura de páginas (la que posiciona, verificada 2026)

```
/                         → home (conversión + marca)
/[servicio]/              → 1 página por servicio core
/[servicio]-[ciudad]/     → servicio+zona (la mina del SEO local MX)
/precios/ o /faq/         → captura "cuánto cuesta X en [ciudad]"
/nosotros/                → confianza (fotos reales, credenciales)
/contacto/                → NAP + mapa + form + WhatsApp
/blog/[slug]/             → solo si habrá contenido real sostenido
```

Reglas de las páginas servicio+zona: contenido ÚNICO real por zona (landmarks, cómo llegar, testimonios de esa zona, FAQ local) — las plantillas clonadas son penalizables (doorway pages). Title pattern: `[Servicio] en [Colonia], [Ciudad] | Desde $X | [Marca]`. Cada una con su JSON-LD (`areaServed`).

En México la COLONIA pesa tanto como la ciudad (CDMX/MTY/GDL): "dentista en la Narvarte" > "dentista cdmx" en intención.

## Navegación y layout

- Nav SÍ existe (es sitio, no landing): logo + 4-5 links + CTA persistente destacado.
- Menú móvil con timeline GSAP pausada (`01/micro-interacciones.md` §6), aria-expanded correcto.
- Footer completo: NAP idéntico al GBP, horarios, mapa, redes — es señal local y de confianza.
- Breadcrumbs en páginas internas (+ schema BreadcrumbList).

## El "alma" repartida (que se sienta estudio, no plantilla)

- Home: hero con carácter (patrón del nicho) + reveals + 1 elemento vivo.
- Páginas internas: entrada más sobria (fade+y del h1, reveals de secciones) — 60% del movimiento del home.
- Cambio de tema por sección en el home (`01/contadores-skeletons-transiciones.md` §3) = firma visual gratis.
- Transición de página sutil: suficiente para sentirse app, no tan larga que castigue la navegación.

## SEO técnico del tier

- Todo contenido vendedor en server components · metadata única por página (generateMetadata).
- sitemap.xml automático + canonical + OG por página.
- JSON-LD: LocalBusiness (home/contacto) + Service + BreadcrumbList + FAQPage donde aplique.
- GBP enlazado y optimizado (checklist completo en `cultoworld-kb/07-conversion-ux/seo-local-mx.md`).
- Blog solo con compromiso real: 1 post/mes bien hecho > 10 posts IA de relleno.

## Definición de éxito

Todas las páginas pasan CWV · rankea para "[servicio] + [zona]" en 2-3 meses · leads atribuibles (eventos por página) · el sitio entero se siente UNA marca (misma skin, mismos easings).
