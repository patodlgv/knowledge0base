# SEO completo 2026: on-page, técnico y la era AI search (GEO/AEO)

> **Tier: LIGERO/A+C** · Complementa `07-conversion-ux/seo-local-mx.md` (que cubre GBP/local). Esto es el resto del juego: clusters, técnico, y cómo ser CITADO por las IAs.

## El panorama en números (2026)

- ~58.5% de búsquedas Google terminan sin clic; en AI Mode ~93%.
- Con AI Overview presente, el CTR del #1 orgánico cae 34.5-58% (Ahrefs). PERO las marcas citadas DENTRO del overview ven +35% CTR.
- Tráfico desde ChatGPT convierte ~15.9% vs ~1.76% del orgánico (~9×): poco volumen, intención altísima.
- **Solo ~7.9% de búsquedas LOCALES disparan AI Overview** → el Local Pack es de los pocos espacios protegidos. El SEO local que ya dominamos vale MÁS, no menos. Ángulo de venta: "blindamos tu negocio contra el zero-click".

## On-page moderno

- **Intención primero**: cada página clasificada (informacional/comercial/navegacional). Error clásico del negocio de servicio: blog que rankea pero CERO páginas comerciales por servicio+zona.
- Title ≤60 chars con keyword al inicio + diferenciador; H1 puede diferir (H1 persuasivo, title query-matching); H2/H3 que respondan sub-preguntas literales (alimenta AI Overviews).
- **Topical authority / clusters**: 1 página pilar ("Guía completa de [servicio] en [ciudad]") + 8-15 clusters interenlazados y apuntando a la página comercial. Datos: 20-40% más visibilidad, ~30% más tráfico, rankings ~2.5× más duraderos que posts sueltos. Hiperespecializado > genérico.
- Internal linking con anchors descriptivos; del blog SIEMPRE hacia las páginas que venden.

## SEO técnico

- **CWV**: factor real pero de desempate (~2.3 posiciones entre páginas parejas); el beneficio doble es ranking + conversión. Umbrales en `06-performance/core-web-vitals.md`.
- **JS e indexación**: Googlebot renderiza JS con retraso; **ChatGPT/Perplexity/Claude NO renderizan JS** → un sitio client-side es invisible para AI search. Regla: SSR/SSG para todo lo indexable; el contenido crítico (h1, precios, FAQ) en el HTML inicial. (Nuestra fábrica Next.js ya cumple — es argumento de venta.)
- Sitemap solo con 200+canónicas · canonical autorreferente · hreflang SOLO si hay versiones reales por idioma/país (es-mx correcto, "es-latam" no existe; enlaces recíprocos obligatorios o Google ignora el set; jamás canonical cruzado con hreflang).
- Auditar: noindex accidental de staging, cadenas de redirects, duplicados por parámetros.

## GEO/AEO: cómo ser citado por las IAs (los 7 pilares)

1. **Answer block arriba**: cada página abre con 2-4 frases que responden la pregunta de forma autocontenida y citable.
2. **Claims específicos con números**: "Instalamos minisplits en 24-48h con garantía de 5 años" > "servicio rápido y de calidad". Páginas con cifras/listas/tablas: 30-40% más visibilidad en respuestas IA.
3. **Estructura extraíble**: H2 como pregunta, listas, tablas, FAQ.
4. **JSON-LD completo**: LocalBusiness, Service, FAQPage, Review, Person (autores) — contexto explícito en vez de dejar que aluciné el bot.
5. **Expertise original**: casos propios, fotos reales, resultados medidos — las IAs favorecen lo que no existe en otro lado.
6. **Infraestructura**: SSR, permitir GPTBot/PerplexityBot/ClaudeBot en robots.txt, **alta en Bing Webmaster Tools** (ChatGPT Search usa el índice de Bing).
7. **Consenso web**: menciones en directorios/prensa local + reseñas trabajadas (Perplexity cita en tiempo real).

**llms.txt — veredicto honesto**: estándar no oficial, ~844K sitios lo tienen, NINGÚN proveedor grande confirmó parsearlo, sin evidencia medida de mejora. Cuesta 30 min: inclúyelo como extra premium, no lo vendas como palanca de resultados.

## Las 4 páginas que más rinden (negocio de servicio)

1. **Precios / "cuánto cuesta X en [ciudad]"** — la más subutilizada en MX (miedo a publicar precios). Estructura: answer block con rango ("Una remodelación de baño en Monterrey cuesta entre $45,000 y $180,000 MXN en 2026…") → tabla por nivel → factores → qué incluye → FAQ → CTA. Imán de citas IA + filtra leads.
2. **FAQ por servicio** — preguntas REALES del WhatsApp, respuesta directa de 40-60 palabras, schema FAQPage.
3. **Comparativas "X vs Y"** (carillas vs coronas, minisplit vs central): veredicto arriba → tabla → cuándo cada uno → CTA. Intención comercial altísima; formato favorito de las IAs.
4. **Guías locales** ("Guía 2026 de [servicio] en [ciudad]"): topical authority + links locales.

## E-E-A-T operativo para negocio local

- **Experience**: fotos/videos de trabajos REALES, antes/después con datos — lo más barato y lo que nadie hace.
- **Expertise**: autores con nombre, cédula/certificaciones, bios enlazadas a LinkedIn.
- **Authoritativeness**: menciones locales (prensa, cámaras, directorios), NAP consistente.
- **Trust (el componente central según Google)**: reseñas abundantes Y respondidas (señal directa para recomendaciones de AI local), precios transparentes, HTTPS, aviso de privacidad (además obligatorio por LFPDPPP en México).

## Paquete "AI-ready" de la fábrica (checklist de venta)

- [ ] SSR/SSG + contenido crítico en HTML inicial
- [ ] JSON-LD completo por página + answer blocks + FAQ marcadas
- [ ] Página de precios con cifras reales
- [ ] robots.txt permitiendo bots de IA + Bing Webmaster Tools
- [ ] Cluster inicial: pilar + precios + FAQ + 1 comparativa + 1 guía local
- [ ] Sistema de reseñas activo (QR + WhatsApp post-servicio)
