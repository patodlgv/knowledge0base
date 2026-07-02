# ✅ Delivery Checklist — antes de entregar CUALQUIER sitio

> Versión ejecutiva. La versión detallada con valores exactos vive en `cultoworld-kb/00-sistema/checklist-pre-entrega.md` — esta es la de batalla.

## Performance (PageSpeed Insights, móvil)
- [ ] LCP < 2.5s (fábrica: < 2.0s) · CLS ≤ 0.1 · INP ≤ 200ms
- [ ] Hero image AVIF/WebP < 150 KB, `fetchpriority="high"`, SIN lazy
- [ ] Todo lo bajo el fold: `loading="lazy"` + dimensiones explícitas
- [ ] Probado con CPU 4x + Slow 4G en DevTools (no en tu máquina a pelo)
- [ ] Third-parties (chat, pixels) diferidos

## Animación
- [ ] Solo transform/opacity · `prefers-reduced-motion` respetado
- [ ] Sitio legible con JS apagado (SSR + gsap.from)
- [ ] React: todo GSAP en `useGSAP` · Sin jank al scrollear rápido
- [ ] Tier B: DPR ≤2, fallback estático bello, dispose al desmontar, 60fps en laptop promedio

## Accesibilidad
- [ ] Tab completo con focus visible · Un h1, jerarquía sin saltos
- [ ] Contraste 4.5:1 cuerpo / 3:1 grande (verificado) · alt reales · labels en inputs · lang="es-MX"

## SEO (tier A/C)
- [ ] Title (~55) + description (~155) únicos · OG completo (previews WhatsApp)
- [ ] JSON-LD LocalBusiness válido (validator.schema.org) · sitemap + robots + canonical
- [ ] Contenido vendedor en HTML del server (no client-only)

## Conversión
- [ ] Headline + CTA visibles sin scroll EN MÓVIL · CTA sticky móvil activo
- [ ] WhatsApp con mensaje prellenado + evento de tracking
- [ ] Form ≤5 campos, validación inline, mensaje de éxito con siguiente paso claro
- [ ] Prueba social específica cerca del primer CTA · Landing de campaña SIN nav

## Cierre
- [ ] Probado en un Android real de gama media
- [ ] Lighthouse ≥90 Performance y Accessibility (tier A/C)
- [ ] 404 con carácter · favicon+og:image · formulario/WhatsApp PROBADOS end-to-end (llega el lead)
- [ ] GBP del cliente enlazado al sitio · Analytics/eventos activos
- [ ] Skin y aprendizajes registrados en el vault
