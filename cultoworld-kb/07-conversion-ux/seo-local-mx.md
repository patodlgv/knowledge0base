# SEO local México (Google Business Profile + schema + zonas)

> **Tier: LIGERO/A+C** · El Local Pack se lleva 40-60% de los clics de una búsqueda local. ~46% de búsquedas tienen intención local; 28% de las locales terminan en compra. Esto es tan importante como el sitio mismo.

## Google Business Profile — checklist 2026

1. Verificación + **NAP idéntico** al del sitio (nombre, dirección, teléfono — carácter por carácter).
2. **Categoría principal exacta** ("Clínica dental" ≠ "Dentista") + 2-3 secundarias. Factor #1 controlable.
3. Nombre REAL del negocio (keyword stuffing en el nombre = riesgo de suspensión).
4. Descripción (750 chars): propuesta + servicios + zonas, keyword natural en las primeras 2 líneas.
5. **Fotos reales**: exterior (para encontrarte), interior, equipo, producto; 3-5 nuevas/mes.
6. **Reseñas = factor #1**: cantidad + calificación + RECENCIA + keywords dentro de la reseña. Sistema: QR en mostrador + WhatsApp post-servicio con link directo. **Responder el 100%** (positivas breve con keyword; negativas con solución).
7. **Q&A sembrado**: publica tú las 5-10 preguntas frecuentes (precios aprox, estacionamiento, formas de pago) y respóndelas.
8. Posts semanales (ofertas/novedades) + servicios/menú CON precios (capturan "precio de X").
9. Atributos + horarios festivos MX actualizados (12 dic, Semana Santa…).
10. Mensajería/WhatsApp activado y respuesta rápida (el tiempo se muestra público).

## JSON-LD LocalBusiness — plantilla válida MX

Usar el subtipo MÁS específico: `Restaurant` (+`servesCuisine`, `acceptsReservations`), `Dentist`, `HealthAndBeautyBusiness` (spa), `RealEstateAgent`, `ExerciseGym`, `LegalService`.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Dentist",
  "@id": "https://clinicasonrisamty.mx/#negocio",
  "name": "Clínica Dental Sonrisa",
  "description": "Clínica dental en Monterrey especializada en ortodoncia invisible, implantes y limpieza dental. Valoración sin costo.",
  "url": "https://clinicasonrisamty.mx",
  "telephone": "+52-81-1234-5678",
  "image": "https://clinicasonrisamty.mx/img/fachada.jpg",
  "priceRange": "$$",
  "currenciesAccepted": "MXN",
  "paymentAccepted": "Efectivo, Tarjeta, Transferencia",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Av. San Pedro 250, Local 4, Col. del Valle",
    "addressLocality": "San Pedro Garza García",
    "addressRegion": "NL",
    "postalCode": "66220",
    "addressCountry": "MX"
  },
  "geo": { "@type": "GeoCoordinates", "latitude": 25.6573, "longitude": -100.3625 },
  "openingHoursSpecification": [
    { "@type": "OpeningHoursSpecification",
      "dayOfWeek": ["Monday","Tuesday","Wednesday","Thursday","Friday"],
      "opens": "09:00", "closes": "19:00" },
    { "@type": "OpeningHoursSpecification", "dayOfWeek": "Saturday",
      "opens": "09:00", "closes": "14:00" }
  ],
  "sameAs": ["https://www.facebook.com/…", "https://www.instagram.com/…"],
  "areaServed": ["San Pedro Garza García", "Monterrey", "Santa Catarina"]
}
</script>
```

Validar en validator.schema.org + Rich Results Test. Nota: `aggregateRating` solo con reseñas propias del sitio (no copiadas de Google).

## Keywords locales — patrones MX

`[servicio] cerca de mí` (+500% en 5 años) · `[servicio] en [colonia]` · `[servicio] [ciudad] precio` / `cuánto cuesta [servicio]` · `[servicio] abierto ahora` · `mejor [servicio] en [zona]` · coloquiales ("dentista que acepte pagos", "gym con alberca").

**En zonas metro (CDMX/MTY/GDL), la COLONIA pesa tanto como la ciudad.**

### Estructura de páginas servicio+zona

```
/limpieza-dental/                → servicio
/limpieza-dental-monterrey/      → servicio+ciudad
/dentista-col-del-valle/         → servicio+colonia
/precios/                        → captura "cuánto cuesta X"
```

Cada página zona con contenido ÚNICO real (landmarks, cómo llegar, testimonios de esa zona, mapa, FAQ local, su JSON-LD con `areaServed`). Clonar plantillas = doorway pages = penalización.
Title pattern: `Limpieza Dental en Col. del Valle, Monterrey | Desde $599 | Clínica Sonrisa`.

## Citations que importan en México (NAP idéntico en todas)

1. Google Business Profile (obligatorio)
2. Facebook/Instagram Business (en MX funcionan como segundo buscador)
3. Sección Amarilla (el directorio MX con más autoridad)
4. Waze / Apple Maps / Bing Places · Yelp MX
5. Por giro: **Doctoralia** (salud — crítico), TripAdvisor/OpenTable (restaurantes), Inmuebles24/Lamudi (inmobiliarias), Fresha/Booksy (barbería/spa)
6. CANACO local, DENUE de INEGI, directorios del giro

## Oferta de la fábrica

Cada sitio tier A/C se entrega con: JSON-LD válido + GBP optimizado (checklist arriba) + 1-3 páginas servicio+zona + sistema de reseñas (QR + mensaje). Es diferenciador frente a "te hago tu página" y justifica retainer mensual (posts GBP + reseñas + reporte).
