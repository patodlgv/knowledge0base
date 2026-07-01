# Pares tipográficos premium por tipo de negocio

> **Tier: AMBOS** · Regla: **1 display (personalidad) + 1 texto (neutral)**, o **1 familia / 2 pesos**. Máximo 2 familias. El contraste entre ambas debe ser obvio o nulo — lo intermedio parece error.
> Todas las opciones listadas están en Google Fonts (gratis, compatibles con next/font) salvo las marcadas 💰 (premium de fundidora, para clientes $14,500+).

## Personalidades (mapa rápido)

- **Serif display** → lujo, herencia, editorial (spa premium, bufete, hotel)
- **Grotesca** → moderno, técnico, confiable (SaaS, agencia, fintech)
- **Geométrica** → contemporáneo, amigable (startup, educación) — cuidado: Poppins ya huele a plantilla
- **Humanista** → cálido, cercano (salud, comunidad)
- **Slab/Display condensada** → fuerza, deporte, impacto (gym, barbería)
- **Mono** → acento "engineered" (dev, fintech) — solo como tercer toque justificado

## Pares por nicho

### Clínica / salud — competencia cálida
- **Fraunces (display, opsz alto) + Inter** — serif moderna con calidez, cuerpo neutral
- Alternativa: **Source Serif 4 + Source Sans 3** (armonía de superfamilia)
- 💰 Tiempos Headline + Untitled Sans

### Restaurante / café — apetito editorial
- **Playfair Display + Lato** — clásico bistró
- **Fraunces (soft, wonk) + Work Sans** — café de especialidad
- 💰 Canela + Founders Grotesk (fine dining)

### Gym / crossfit — impacto
- **Archivo Expanded/Black + Inter** — la expandida en mayúsculas es el look deportivo actual
- **Anton (display condensada) + Roboto Flex**
- Titulares en caps con `letter-spacing: 0.02em` (las condensadas en caps respiran mejor abiertas)

### Barbería — herencia con actitud
- **Bricolage Grotesque + Inter** — carácter sin disfraz vintage
- **Abril Fatface + Work Sans** — didona con peso, vibra clásica
- 💰 Saol Display + Neue Montreal

### Bienes raíces / hotel — lujo discreto (la zona Culto)
- **Cormorant Garamond (600) + Inter** — elegancia alta, usar grande (delgada en chico se pierde)
- **Playfair Display + Inter** — el par de la casa Culto
- 💰 Canela + Suisse Int'l

### Spa / estética — suavidad
- **Marcellus + Karla** — serif inscripcional serena
- **Lora + Nunito Sans** — más accesible/cálido

### Legal / contadores — autoridad
- **Libre Caslon Text + IBM Plex Sans** — Caslon = tradición jurídica
- **Una familia, dos pesos: IBM Plex Sans 600/400** — despacho moderno
- 💰 Tiempos Text + Graphik

### Fintech / SaaS — precisión
- **Inter (700 tight) + Inter (400)** — una familia, tracking negativo en display: `letter-spacing: -0.03em`
- **Space Grotesk + Inter** — display con carácter técnico
- Acento mono: **JetBrains Mono** para cifras/labels

### Educación — optimismo
- **Bricolage Grotesque + Source Sans 3**
- **Epilogue + Mulish**

### Estudio creativo / marca personal — carácter máximo
- **Clash Display (Fontshare, gratis) + Satoshi (Fontshare)** — el par "agencia 2025"
- **Unbounded + Inter** — rareza geométrica
- Fontshare (ITF) es gratis y de calidad fundidora: Clash, Satoshi, General Sans, Cabinet Grotesk

## Reglas de uso display

```css
h1, h2 {
  font-family: var(--font-display);
  line-height: 1.05;                 /* display grande = line-height apretado */
  letter-spacing: -0.02em;           /* serif/grotesca grande casi siempre lo pide */
  text-wrap: balance;                /* líneas equilibradas gratis */
}
body { line-height: 1.6; font-size: 1rem; }  /* nunca <16px en cuerpo */
```

- Pesos: cargar SOLO los usados (típico: display 600 o 700 + body 400 y 600). Cada peso = bytes.
- Español MX: verificar á é í ó ú ñ ¿ ¡ en el display elegido ANTES de comprometerse (algunas display gratuitas traen diacríticos pobres).
- Medida de cuerpo: 60–75 caracteres por línea (`max-width: 65ch` en bloques de texto).

## Variable fonts (bonus premium)

Fraunces, Roboto Flex, Bricolage y Archivo son variables: un solo archivo, todos los pesos + ejes extra (`opsz`, `SOFT`, `wonk`). Para kinetic typography (animar `font-variation-settings` con GSAP scrub) son LA herramienta — ver `08-referencias`.

```css
h1 { font-variation-settings: "opsz" 144, "wght" 640; }
```
