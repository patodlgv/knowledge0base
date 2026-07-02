# HEROS MAESTROS — el catálogo definitivo (14 patrones nivel estudio)

> **Tier: LIGERO** (todos; cero WebGL) · **El hero decide en ~50ms** (Stanford: 46-75% juzga credibilidad por diseño visual). Investigado de Awwwards/godly/land-book/Codrops 2024-2026 + ganadores de hospitality/salud/barbería. Complementa `heros-ligeros.md` (los 6 base); este es el arsenal completo.
>
> **El hallazgo clave:** el hero premiado 2025-2026 dejó de ser "foto grande + título" y se volvió **sistema de layout tipográfico** — lo cual juega EXACTAMENTE a favor del tier ligero: los mejores heros del mundo hoy NO necesitan WebGL.

## Regla de oro de todos

Titular de beneficio (~31% mejor que features) · UN CTA primario · prueba social verificable (rating Google) en el hero · la animación jamás retrasa la legibilidad >1s ni bloquea el CTA · LCP <2s intocable.

---

## Los 14 patrones

### 1. TYPE-FIRST EDITORIAL ⭐ (el #1 de lo premiado)
- **Anatomía:** titular a `clamp(3rem, 12vw, 11rem)`, 2-4 palabras/línea, 70-90% del ancho, tracking -0.02/-0.04em, line-height 0.9-1.0. Sin imagen o mini-imagen (300px) en esquina. 40% espacio negativo. Alineado IZQUIERDA (no centrado).
- **Motion:** SplitText line-mask (y:110%→0, stagger 0.08, power4.out) + leve `rotate: 3deg→0`.
- **Firma:** UNA palabra en serif itálica dentro de la sans; hover que la cambia o revela mini-imagen inline.
- **Para:** TODOS — el salvavidas cuando la foto del cliente es mala. Perf: el mejor (LCP=texto).

### 2. IMAGEN-ENTRE-LETRAS (inline image pill)
- **Anatomía:** píldora de foto (~1em alto, border-radius 999px, inline-block) DENTRO de la frase: "La mejor [🌮] cocina de Monterrey".
- **Motion:** texto asienta → píldoras scale 0→1 con stagger; hover expande 1.15x o mini-crossfade.
- **Firma:** píldora rotada -3° que flota (y:±4px loop). Solo necesitas UNA foto decente chiquita (<30KB).
- **Para:** restaurante, barbería, spa, gym. Riesgo: reservar min-width (CLS) y cuidar line-break móvil.

### 3. SPLIT ASIMÉTRICO 60/40 (nunca 50/50)
- **Anatomía:** texto 55-65% / foto 35-45% full-bleed VERTICAL (3:4). Un elemento CRUZA la frontera (badge circular, cifra enorme montada sobre ambas columnas).
- **Motion:** clip-path reveal <0.9s + scale 1.3→1 interno; Ken Burns CSS 20s como elemento vivo.
- **Firma:** la violación de la frontera. Templates respetan columnas; estudios las rompen.
- **Para:** dental, inmobiliaria, barbería. La foto ES el LCP: AVIF 100-150KB priority.

### 4. FULL-BLEED ELEVADO (el clásico, bien hecho)
- **Anatomía:** foto 85-100svh, **gradiente TINTADO con el color de marca** (no negro plano), titular serif abajo-IZQUIERDA que toca los bordes, nav transparente.
- **Motion:** scale 1.15→1 en 1.4s + line-mask; Ken Burns lentísimo 25s (sensación video sin video) + scroll-hint animado.
- **Firma:** el tinte de marca en el overlay + titular sangrando del viewport. Opcional `mix-blend-mode: difference`.
- **Para:** restaurante, spa, hotel. JAMÁS carrusel auto-avance; máx crossfade 2-3 (lazy tras la 1ª). AVIF <180KB obligatorio.

### 5. BENTO / COLLAGE DE CELDAS
- **Anatomía:** grid 4-6 celdas radios 16-24px: titular grande + 2-3 fotos (recortes DISTINTOS) + celda de dato ("4.9★ · 800 reseñas") + CTA/mini-mapa. Alturas desiguales.
- **Motion:** cascada diagonal scale 0.92→1 stagger 0.07 back.out; contador animado en la celda de dato; marquee interno en otra.
- **Firma:** UNA celda anómala (rotada 2°, o que se sale 20px, o píldora entre rectángulos). El bento template es regular; el de estudio tiene la anomalía deliberada.
- **Para:** dental, gym, inmobiliaria (mucha prueba). Total fotos <250KB, solo la grande priority.

### 6. FRASE POLÍGLOTA (serif+sans+itálica) ⭐ tendencia #1 tipográfica
- **Anatomía:** una frase donde 1-2 palabras clave cambian a serif ITÁLICA (y quizá al color de acento): "Tu sonrisa, *sin miedo*." Resto en grotesca.
- **Pares gratis:** General Sans+Instrument Serif · Archivo+Fraunces · Space Grotesk+Playfair Italic · Clash Display+EB Garamond Italic.
- **Motion:** la itálica entra ÚLTIMA con blur-in (blur 8px→0); elemento vivo = rotador de palabras ("sin miedo/sin dolor/sin esperas" cada 2.5s, min-width fijado = cero CLS).
- **Firma:** itálica con baseline -0.05em y tamaño 1.05x, como escrita a mano encima.
- **Para:** universal (dental/spa la itálica humaniza; barbería tipo lettering).

### 7. OUTLINE/STROKE + RELLENO AL SCROLL
- **Anatomía:** palabra apilada 3 veces, la del medio rellena, las otras `-webkit-text-stroke: 2px`. Fondo oscuro.
- **Motion:** el relleno avanza con scroll (background-clip: text + gradiente, ScrollTrigger scrub); una línea en marquee.
- **Firma:** relleno parcial scrubbed (no on-load) + stroke que respira en hover.
- **Para:** GYM ("ENTRENA ENTRENA ENTRENA"), barbería flash-tattoo. NO dental/spa.

### 8. MARQUEE-HERO
- **Anatomía:** 1-2 cintas de texto gigante (8-14vw) con separadores (★ • icono), **en diagonal -4°** o cruzadas en direcciones opuestas + bloque compacto de subcopy+CTA estático.
- **Motion:** loop infinito con velocidad REACTIVA al scroll (timeScale por velocity) — ya está en nuestro MotionEngine.
- **Firma:** la inclinación + la reacción al scroll. El marquee template es horizontal y constante.
- **Para:** restaurante casual, barbería, gym. reduced-motion → cinta estática.

### 9. MÁSCARA ORGÁNICA (arco/blob/óvalo) ⭐ la forma de la era
- **Anatomía:** foto dentro de ARCO de capilla (`border-radius: 999px 999px 0 0`), óvalo o blob SVG. Titular izquierda, arco derecha ligeramente montado sobre el texto. Fondos crema/terracota/sage.
- **Motion:** el arco sube con clip-path o la foto zoom-out dentro de máscara fija; **texto circular girando** (SVG textPath, 20s) alrededor del arco con ✳ como marcador.
- **Firma:** el arco recortando una LETRA del titular (foto pasa entre la tipografía con z-index) + el textPath giratorio.
- **Para:** SPA, dental boutique, salón, brunch. **El recorte agresivo esconde fondos feos de fotos de Google Maps.**

### 10. BRUTALISTA SUAVE + STICKERS
- **Anatomía:** bordes 2px duros, sombras offset SÓLIDAS (`box-shadow: 6px 6px 0`) EN COLOR DE ACENTO (no negro), tipografía enorme, capa de stickers: badges rotados ("ABIERTO HOY", "★4.9"), flechas dibujadas, subrayado marcador SVG, foto polaroid rotada 2-4° con cinta adhesiva.
- **Motion:** stickers scale 0→1 con rotación excesiva → asentamiento elastic.out, stagger tras el titular; badge circular girando; polaroid que se endereza en hover.
- **Firma:** imperfección calculada — cada sticker con rotación distinta; elementos encimados tocando bordes.
- **Para:** taquería, barbería, gym boutique. NO clínica/inmobiliaria.

### 11. CIFRA GIGANTE COMO HERO
- **Anatomía:** el elemento más grande es un NÚMERO a 20-30vw ("23" años, "+5,000" cortes) con etiqueta chica al lado; la foto vive DENTRO del número (`background-clip: text`) con pan lento.
- **Motion:** contador GSAP 0→N (1.2s, snap enteros) arrancando YA (no al scroll). `font-variant-numeric: tabular-nums` (sin jitter = sin CLS).
- **Firma:** la foto dentro del número + la cifra sangrando del viewport. NADIE con template hace esto.
- **Para:** inmobiliaria ("+340 vendidas"), gym ("−12kg promedio"), negocios con historia.

### 12. VIDEO-FRAME FALSO (el sustituto del video prohibido)
- **Anatomía:** frame que PARECE video: 3-4 fotos en crossfade lento (4s c/u) con Ken Burns ALTERNADO (una zoom-in, otra pan lateral) + grano fílmico encima + **timecode falso en mono ("01 / 04")** que avanza.
- **Motion:** timeline GSAP en loop con overlap 1s. Intrínsecamente vivo.
- **Firma:** el grano + el timecode = dirección de arte, no slideshow.
- **Para:** restaurante (ambiente+platillo+gente), spa, gym. Solo la 1ª foto priority (LCP); resto lazy precargado. Total <400KB — el más pesado del catálogo, el máximo wow hospitality.

### 13. PORTADA DE PERIÓDICO (print revival)
- **Anatomía:** línea superior en mono ("MONTERREY, N.L. — MARTES — 34°C" con FECHA REAL renderizada en server), regla fina, masthead serif enorme centrado, 2-3 columnas: foto con pie en itálica (+crédito falso de fotógrafo), párrafo con capitular, CTA como CUPÓN punteado con ✂.
- **Motion:** las reglas se dibujan (scaleX 0→1), masthead con stagger mínimo, foto wipe vertical.
- **Firma:** la densidad de micro-detalles (fecha real, pie de foto, cupón). Densidad diminuta = estudio.
- **Para:** restaurante tradicional, cantina, barbería clásica, inmobiliaria heritage.

### 14. STICKY-CORTINA (envuelve a cualquiera de los otros 13)
- **Anatomía:** hero `position: sticky; top: 0` (100svh); la sección 2 scrollea POR ENCIMA. El hero interno puede ser cualquier patrón 1-13.
- **Motion:** al ser tapado: scale 1→0.94 + brightness 1→0.6 + border-radius 0→24px (scrub). Como si el hero fuera una tarjeta física que queda atrás.
- **Firma:** percepción instantánea de sitio caro con costo casi cero (todo composited). Usar svh (no vh, iOS). brightness sí, blur NO (caro en móvil).
- **Para:** universal — es un multiplicador de cualquier otro patrón.

---

## Asignación por nicho (actualiza la tabla de heros-ligeros.md)

| Nicho | Primera opción | Segunda | Tercera |
|---|---|---|---|
| Restaurante casual/taquería | 8 marquee · 10 stickers | 2 imagen-entre-letras | 12 video-falso |
| Restaurante fine-dining | 4 full-bleed tintado | 12 video-falso | 13 periódico |
| Dental | 6 políglota | 3 split asimétrico | 5 bento (prueba) |
| Spa/estética | 9 arco orgánico | 6 políglota | 1 type-first |
| Gym | 7 outline+relleno | 11 cifra gigante | 8 marquee |
| Barbería | 10 stickers | 7 outline | 13 periódico |
| Inmobiliaria | 11 cifra gigante | 3 split | 5 bento |
| Sin fotos buenas (scraping) | 1 type-first | 6 políglota | 11 cifra |

**+ patrón 14 (sticky-cortina) como envoltorio de cualquiera.**

## Tipografía de heros 2026 (reglas)

- Display 9-13vw, 3-5 palabras/línea, line-height 0.9-1.05, tracking negativo, `clamp()` siempre.
- Mono uppercase con tracking amplio como "voz de sistema" (fechas, coordenadas, "SCROLL", "01/04") — el condimento que hace TODO verse diseñado.
- Variable font (1 woff2, peso 300-900): "peso que respira" en hover con font-variation-settings.
- SVG textPath para insignias circulares giratorias — el accesorio #1 de 2025.

## Elevar fotos mediocres de Google Maps (todo CSS)

1. **Duotone:** `grayscale(1) contrast(1.1)` + overlay de marca con `mix-blend-mode: multiply` — cualquier foto se vuelve pieza de identidad.
2. **B/N unificador:** `grayscale(1) contrast(1.15) brightness(1.05)` — unifica fotos de fuentes distintas; el color vive solo en tipografía/stickers.
3. **Grano fílmico:** feTurbulence opacity .06-.1 blend overlay — esconde compresión JPEG.
4. **Máscara agresiva** (arco/óvalo/píldora): elimina estacionamientos, cables, fondos feos.
5. **Crop extremo a un detalle** (manos, vapor, textura) > la escena completa mediocre.
6. **Polaroid:** marco blanco + rotación + sombra offset = "estética intencional".
7. **Gradiente TINTADO de marca** sobre la foto (nunca negro plano).
8. **`background-blend-mode: luminosity`** con el fondo: la foto se funde como textura.

## Conversión (los datos)

Heros optimizados: +25-55% conversión · >60% del heatmap ocurre sobre el fold · beneficio > feature ~+31% · botones > links hasta +45% CTR · rating de Google como micro-badge (verificable en Maps = oro local) · anti-patrón: carrusel auto-avance con mensaje cambiante (nadie lee el slide 2).

## Fuentes

awwwards.com/websites/hotel-restaurant · awwwards collections text-marquee · land-book.com/sections/hero · tympanus.net/codrops (GSAP highlights, SplitText demos, scroll-driven text, CSS masks) · figma.com/resource-library/web-design-trends · line25 2026 · fireart trends · prismic/omniconvert/saashero (CRO de heros) · s8e8 dental winners · thesalonbusiness/colorlib barbershops · toimi.pro restaurantes · jmperezperez.com + cssduotone.com (duotone CSS)
