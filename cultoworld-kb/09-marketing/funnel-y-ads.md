# El funnel completo: ads → página → WhatsApp → venta (México 2026)

> **Tier: LIGERO/A+C** · La página no vive sola: es una pieza de un sistema medible. Esto es lo que separa un sitio de $8,000 de un sistema de $40,000+.

## El funnel MX

**Tráfico → Landing (o directo a WhatsApp) → Conversación → Nurture → Venta → Reseña/Referido**

La particularidad mexicana: **WhatsApp es el centro, no el email** (~93% de adultos MX lo usan).

### Canal ganador por nicho

| Nicho | Canal | CPL típico Meta MX (USD) |
|---|---|---|
| Restaurante, belleza, estética | Meta/IG + click-to-WhatsApp | ~$7.6 |
| Gym, wellness | Meta + orgánico IG/TikTok | ~$10.4 |
| Hogar (clima, plomería, remodelación) | Google Ads + Local Pack | ~$24.6 (Meta); Google mayor CPC pero intención superior |
| Legal, médico, dental | Google Search + SEO local | ~$36.6 |
| TikTok orgánico | Descubrimiento (food/belleza/inmo) | genera búsqueda de marca, no leads directos |

Ventaja estructural: CPM Meta México ~$3.92 vs ~$14.19 global — anunciar aquí cuesta 4-8× menos por impresión. Con $5,000-15,000 MXN/mes se opera un funnel completo. (Tendencia: CPM +20%/año → la conversión post-clic vale cada vez más = nuestro negocio.)

## Click-to-WhatsApp Ads (CTWA) — el arma y su trampa

- CPL 50-70% menor que la campaña equivalente a landing; funnel de 2 pasos (clic → mensaje).
- **Solo gana si**: respuesta <30s (bot o humano), se captura el `ctwa_clid` del webhook y se dispara la venta vía Conversions API, y se cierra en la ventana de 72h. 9 de 10 anunciantes pierden la atribución (ven "conversaciones", nunca conectan ventas). **Implementar atribución CTWA completa = servicio diferenciador de Culto que casi nadie ofrece.**
- La landing sigue siendo necesaria cuando: ticket alto (necesitan ver portafolio/precios), tráfico de Google Search, o el negocio no responde rápido.

## Message match (Quality Score)

1. **El H1 repite casi literal el headline del anuncio.** Causa #1 de "Below Average" y el fix más rápido.
2. Un ad group = una intención = una landing/variante (5-15 keywords del mismo tema).
3. Desde feb 2025 Google evalúa la landing ANTES de la subasta (IA pondera contenido/claridad/consistencia) — una página puede nacer castigada.
4. Landing page experience = relevancia + velocidad + móvil + transparencia (quién eres, contacto, privacidad).
5. Continuidad VISUAL: la imagen del anuncio en el hero.
6. QS 7-10 puede bajar el CPC 20-50% vs QS 3-4.

## Nurture (el lead que no compró hoy)

**Speed-to-lead es la métrica reina**: auto-respuesta <30s + humano <5min. Cada hora de retraso desploma conversión.

Secuencia de 4-5 toques en 7-14 días (WhatsApp para mid/bottom; email para nurture largo):

- **Día 0**: confirmación + entrega de lo prometido + 1 pregunta de calificación
- **Día 1**: propuesta de valor / resolver la duda pendiente
- **Día 3**: contenido de valor (tips, antes/después, video corto)
- **Día 5**: prueba social (caso, testimonio, fotos reales)
- **Día 7**: oferta directa con deadline real o invitación a cita

Después: cadencia lenta (1-2/mes). Acelerar con quien responde, espaciar con quien no. WhatsApp API: opt-in + plantillas aprobadas fuera de la ventana de 24h. (Open rate WA ~98%, CTR 15-80% vs 2-5% email.)

**Cierre del loop**: pedir reseña de Google por WhatsApp 24-48h post-servicio con link directo — alimenta Local Pack Y las recomendaciones de las IAs.

## Retargeting mínimo viable (operable por negocio local)

1. Meta Pixel + Conversions API (con iOS/ITP el pixel solo pierde ~30% de eventos).
2. 3 audiencias: visitantes 30-90d (excluyendo convertidos), engagement IG/FB 90-365d, lista de clientes (lookalikes + exclusión).
3. 1 campaña always-on ($50-100 MXN/día), creativo de prueba social + oferta, frecuencia <4-6/semana.
4. Números: retargeting convierte ~10× vs display frío; cálidas 15.8% vs 4.3% frías.

## Métricas que importan

CPL por canal · **tasa lead→cita** y cita→venta (donde muere el dinero: el promedio agenda 42% de llamadas; los top 65-90% — el problema suele ser el teléfono, no los ads) · CAC (pyme típica $100-400 USD) · LTV:CAC ≥ 3:1 · landing: mediana real 3-6% (legal ~7.4%, salud 3-4.2%); visitante→lead sano en servicios 5-10%, lead→cliente 20-35%.

## Lo que LA PÁGINA debe traer de serie (checklist de la fábrica)

- [ ] Meta Pixel + CAPI + Google tag; eventos: Lead, Contact/Schedule, clic WhatsApp, clic teléfono
- [ ] CTWA: captura de `ctwa_clid` + evento de venta por Conversions API (proyectos con ads)
- [ ] UTMs: convención fija en minúsculas; persistidas en campos ocultos del form y en el texto prellenado de WhatsApp ("Hola, vi su anuncio de [campaña]")
- [ ] Thank-you page dedicada POR tipo de conversión (`/gracias-cotizacion`) — la conversión se dispara ahí, nunca en el clic; y hace segunda oferta (agendar, WhatsApp)
- [ ] Lead magnet si aplica: el rey es cotización/diagnóstico gratis; luego guía de precios local, calculadora, cupón primera visita
- [ ] CRM aunque sea simple (con UTMs y tags) + la secuencia de nurture cargada
