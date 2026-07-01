# Estructura de secciones que convierte, por tipo de negocio

> **Tier: LIGERO** (es el corazón de la fábrica) · Benchmarks 2026: mediana de conversión de landing ~6.6%; top 10% ≥ 11.45%. La brecha es estructura, no talento.

## Los no-negociables (con números)

1. **Una página = un objetivo = un CTA primario.** Páginas de objetivo único convierten ~22% mejor.
2. **Landing de campaña sin nav** (~10–15% de lift al quitarla; el sitio completo sí lleva nav).
3. **Above the fold limpio:** headline + propuesta + CTA visibles sin scroll EN MÓVIL.
4. **Headline de beneficio ≤ 8 palabras.** Se escribe ANTES de diseñar. Describe el resultado del visitante, no el producto.
5. **Prueba social arriba**, específica: "4.9★ — 1,247 reseñas" > "nos aman". Testimonios suben la acción ~2–3×.
6. **Formularios ≤ 5 campos** (cada campo extra cuesta leads). En MX: el botón de WhatsApp ES el formulario corto.
7. **Copy magro** con jerarquía visual (páginas minimalistas superan a densas ~19%).
8. **CTA repetido** en cada punto de decisión (hero, después de prueba, final).
9. **Velocidad:** +1s ≈ −7% conversiones. 68%+ del tráfico de landing es móvil.

## Secuencia base (el "chasis" de la fábrica)

```
HERO → beneficio + prueba social mínima + CTA
PROBLEMA/AGITACIÓN → el dolor con sus palabras
SOLUCIÓN → cómo funciona (3 pasos máx)
PRUEBA → testimonios, números, logos, antes/después
OFERTA/PRECIOS → claridad, anclaje
OBJECIONES/FAQ → las 4-6 reales
CTA FINAL → cierre con urgencia honesta
```

Cada negocio reordena y especializa:

## Restaurante / café
```
HERO: foto del platillo estrella + "Reserva/Pide" (patrón C de heros)
MENÚ DESTACADO: 6 platillos con foto (no el PDF completo)
EXPERIENCIA: interior/ambiente (galería con parallax ligero)
PRUEBA: reseñas Google + rating grande animado (contador)
UBICACIÓN + HORARIOS: mapa lazy + botones directos (Maps, WhatsApp)
CTA: reservar / pedir por WhatsApp
```
Objeción clave: "¿valdrá la pena?" → fotos reales, no stock.

## Clínica / dental
```
HERO: beneficio del paciente ("Sonríe sin dolor ni esperas") + CTA agendar
CONFIANZA INMEDIATA: credenciales, años, certificaciones (fila de badges)
SERVICIOS: cards de 4-6 tratamientos con precio "desde"
DOCTORES: fotos reales + credenciales (humaniza = convierte)
TESTIMONIOS: con nombre y tratamiento
FAQ: dolor, precios, urgencias
CTA: agendar (form 3 campos: nombre, teléfono, motivo) o WhatsApp
```
Objeción clave: miedo + precio → tono calmado, precios "desde", fotos del espacio.

## Gym / crossfit
```
HERO: energía (patrón B) + "Primera clase gratis"
TRANSFORMACIÓN: antes/después o testimonios con foto (la prueba REINA aquí)
PROGRAMAS: 3-4 cards (contadores: miembros, clases/semana, coaches)
COACHES: fotos + especialidad
PLANES: 3 precios con el medio destacado (anclaje)
CTA: clase gratis (nombre + WhatsApp, 2 campos)
```

## Barbería
```
HERO: actitud tipográfica (patrón B/F) + "Agenda tu corte"
SERVICIOS + PRECIOS: lista clara con precios exactos (aquí SÍ precios exactos)
GALERÍA: cortes reales (grid con hover)
BARBEROS: perfil + IG de cada uno
BOOKING: link a agenda (Booksy/Fresha) o WhatsApp
```

## Bienes raíces
```
HERO: propiedad estrella o buscador simple (patrón D)
INVENTARIO DESTACADO: 3-6 propiedades (card: foto, precio, m², recámaras)
POR QUÉ NOSOTROS: números (vendidas, años, zonas) con contadores
PROCESO: 3 pasos ("Cuéntanos qué buscas → te mostramos → cierras seguro")
TESTIMONIOS: familias reales con la casa de fondo
CTA: form corto (qué busca + presupuesto + WhatsApp)
```
Objeción clave: desconfianza → transparencia de proceso y números.

## Spa / estética
```
HERO: serenidad (patrón A) + "Reserva tu momento"
TRATAMIENTOS: cards con duración y precio
LA EXPERIENCIA: galería del espacio (parallax suave)
PAQUETES: 2-3 con anclaje
GIFT CARDS: sección propia (revenue extra que casi nadie pone)
CTA: reservar por WhatsApp
```

## Legal / despacho
```
HERO: autoridad sobria (patrón A/E) + "Consulta inicial"
ÁREAS DE PRÁCTICA: 4-6 cards
POR QUÉ: años, casos, resultados (contadores discretos)
EQUIPO: socios con credenciales
PROCESO: qué esperar de la primera consulta (baja la barrera)
CTA: form (3 campos) — aquí WhatsApp puede restar seriedad según el target
```

## Reglas de CTA

- Verbo + beneficio: "Agenda tu clase gratis" > "Enviar".
- Color acento reservado para el CTA (ver `04-color`).
- Móvil: CTA sticky inferior (barra con WhatsApp) tras scrollear el hero:
```css
.cta-sticky { position: fixed; inset: auto 0 0 0; padding: .75rem var(--gutter);
  background: var(--paper); border-top: 1px solid var(--line);
  transform: translateY(100%); transition: transform .3s; z-index: 50; }
.cta-sticky.visible { transform: translateY(0); }
```
```js
ScrollTrigger.create({ trigger: ".hero", start: "bottom top",
  onEnter: () => bar.classList.add("visible"),
  onLeaveBack: () => bar.classList.remove("visible") });
```
