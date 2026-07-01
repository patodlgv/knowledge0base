# Checklist pre-entrega

> **Tier: AMBOS** · Correr antes de entregar CUALQUIER sitio. Pásalo a Claude Code como criterio de "terminado".

## Performance (medir en PageSpeed Insights, perfil móvil)

- [ ] LCP < 2.5s (ligero: < 2.0s como meta interna)
- [ ] CLS ≤ 0.1 — toda imagen/video/iframe con `width`/`height` o `aspect-ratio`
- [ ] INP ≤ 200ms — sin tareas JS largas en carga
- [ ] Imágenes en AVIF/WebP con `srcset`; hero con `fetchpriority="high"`, resto `loading="lazy"`
- [ ] Fuentes: máx 2 familias, woff2, preload de las críticas, `font-display: swap`
- [ ] Probado en throttling "Slow 4G" + CPU 4x en DevTools (no en tu laptop con fibra)

## Animación

- [ ] Solo se animan `transform` y `opacity` (nunca width/height/top/left/margin)
- [ ] `prefers-reduced-motion` respetado (estado final visible, sin tweens)
- [ ] Sin FOUC: contenido visible si JS falla (animar con `.from()` o `gsap.set` + clase `js`)
- [ ] En React: todo GSAP dentro de `useGSAP()` (cleanup automático)
- [ ] Un solo loop rAF si hay lerp/cursor custom (no múltiples loops)
- [ ] PREMIUM: DPR cap a 2, delta-time con clamp, `dispose()` al desmontar, fallback estático

## Accesibilidad

- [ ] Un `<h1>` por página, jerarquía sin saltos
- [ ] Navegable completo con Tab; focus visible (`:focus-visible`, nunca `outline:none` a secas)
- [ ] Contraste: 4.5:1 cuerpo, 3:1 texto grande/UI (verificado, no a ojo)
- [ ] `alt` real en imágenes; `<label>` en cada input; `lang="es-MX"`
- [ ] Skip link ("Saltar al contenido")

## SEO (solo tier ligero — el premium lo sacrifica a propósito)

- [ ] `<title>` (~55 chars) y `<meta description>` (~155 chars) únicos por página
- [ ] Open Graph: `og:title`, `og:description`, `og:image` (previews de WhatsApp)
- [ ] Contenido vendedor en server components (HTML real, no generado en cliente)
- [ ] JSON-LD `LocalBusiness` para negocio local MX
- [ ] `sitemap.xml`, `robots.txt`, canonical

## Conversión (tier ligero)

- [ ] Headline de beneficio + CTA visibles sin scroll en móvil
- [ ] Un solo objetivo por página; CTA repetido en 2–3 puntos de decisión
- [ ] Prueba social específica cerca del primer CTA
- [ ] Formulario ≤ 5 campos (o CTA de WhatsApp)
- [ ] Landing de campaña: sin nav

## Final

- [ ] Probado en un Android real de gama media si es posible
- [ ] Lighthouse ≥ 90 en Performance y Accessibility (ligero)
- [ ] Git con historia limpia; deploy en Vercel (ligero) / hosting estático (premium)
