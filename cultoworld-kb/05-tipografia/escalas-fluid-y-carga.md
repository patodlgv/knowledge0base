# Escalas modulares, fluid typography y carga eficiente de fuentes

> **Tier: AMBOS** · **Costo de perf:** las fuentes suelen ser el mayor costo "de diseño" de la página — cargarlas bien es puro LCP/CLS gratis.

## Escala modular (pocos tamaños, un ratio)

Cada tamaño = anterior × ratio. Ratios: 1.2 (sutil), 1.25 (el de la casa), 1.333, 1.5 (dramático). 4–6 tamaños TOTAL:

```css
:root {
  /* ratio 1.25 desde 16px */
  --fs-sm:  0.8rem;   /* 12.8 — labels, captions */
  --fs-base: 1rem;    /* 16 — cuerpo */
  --fs-md:  1.25rem;  /* 20 — lead, subtítulos */
  --fs-lg:  1.95rem;  /* 31 — h3 */
  --fs-xl:  3.05rem;  /* 49 — h2 */
  --fs-2xl: 4.77rem;  /* 76 — h1/hero */
}
```

El tamaño más grande se reserva para UN momento (el hero), no se reparte.

## Fluid typography con clamp()

`clamp(mín, preferido, máx)` — escala con el viewport sin media queries:

```css
:root {
  --fs-hero: clamp(2.5rem, 1.2rem + 6.5vw, 4.77rem);
  --fs-xl:   clamp(1.95rem, 1.2rem + 3.7vw, 3.05rem);
  --fs-lg:   clamp(1.5rem, 1.2rem + 1.5vw, 1.95rem);
  --fs-md:   clamp(1.125rem, 1rem + 0.6vw, 1.25rem);
}
```

Regla del término medio: `Xrem + Yvw` (no `vw` solo — eso rompe el zoom del usuario = falla WCAG 1.4.4). Calculadora: utopia.fyi/type/calculator (genera la escala fluida completa entre dos viewports). El cuerpo base casi no necesita fluid: 1rem–1.125rem basta.

```css
h1 { font-size: var(--fs-hero); line-height: 1.05; letter-spacing: -0.02em; text-wrap: balance; }
p  { max-width: 65ch; }   /* medida: 60–75 caracteres */
```

## Carga de fuentes — Next.js (tier ligero)

`next/font` self-hostea, subsetea y elimina el layout shift automáticamente (calcula el fallback ajustado). El patrón de la fábrica:

```tsx
// app/layout.tsx
import { Playfair_Display, Inter } from "next/font/google";

const display = Playfair_Display({
  subsets: ["latin"],            // "latin" incluye á é í ó ú ñ ¿ ¡
  weight: ["600"],               // SOLO los pesos usados
  variable: "--font-display",
  display: "swap",
});
const body = Inter({ subsets: ["latin"], weight: ["400", "600"], variable: "--font-body", display: "swap" });

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="es-MX" className={`${display.variable} ${body.variable}`}>
      <body>{children}</body>
    </html>
  );
}
```
```css
:root { --font-display: /* la variable la inyecta next/font */; }
h1, h2, h3 { font-family: var(--font-display), Georgia, serif; }
body { font-family: var(--font-body), system-ui, sans-serif; }
```

Fuente local (Fontshare, comprada): `next/font/local` con el .woff2.

## Carga de fuentes — vanilla/Vite (tier premium)

```html
<link rel="preload" as="font" type="font/woff2" crossorigin href="/fonts/display-600.woff2">
<link rel="preload" as="font" type="font/woff2" crossorigin href="/fonts/body-400.woff2">
```
```css
@font-face {
  font-family: "Display";
  src: url("/fonts/display-600.woff2") format("woff2");
  font-weight: 600;
  font-display: swap;        /* texto visible YA con fallback, swap al llegar */
}
```

Reglas:
1. **Solo .woff2** (el formato más chico; soporte universal).
2. **Preload máximo 2** fuentes (las above-the-fold). Preload de más = compite con el LCP.
3. **Subsetting** si self-hosteas manual: `pyftsubset font.ttf --output-file=font.woff2 --flavor=woff2 --unicodes="U+0000-00FF,U+2010-2027,U+00BF,U+00A1"` (latin + ¿ ¡). Corta 100–300 KB a 20–40 KB.
4. **Fallback métrico anti-CLS** (lo que next/font hace solo, versión manual):

```css
@font-face {
  font-family: "Display-fallback";
  src: local("Georgia");
  size-adjust: 105%;          /* ajustar hasta que el swap no salte */
  ascent-override: 92%;
}
h1 { font-family: "Display", "Display-fallback", serif; }
```
Herramienta para calcular overrides: fallback generator de Malte Ubl (screenspan.net/fallback).

5. En premium con preloader: `document.fonts.ready` como parte del `Promise.all` de carga — así el reveal tipográfico nunca corre con la fuente equivocada.

## Jerarquía (el sistema completo en 6 reglas)

1. Un h1 por página, el más grande; nada más compite con él.
2. Saltos de jerarquía = saltos de escala (h2 usa --fs-xl, h3 usa --fs-lg…), no valores inventados.
3. Peso y tamaño no se mueven juntos sin razón: display grande puede bajar a 500-600; cuerpo enfatiza con 600, no con tamaño.
4. `--muted` para metadatos/captions, no para texto que importa.
5. Espaciado ENTRE bloques de texto sale de la escala de spacing (--s2, --s3), no de line-height inflado.
6. `text-wrap: balance` en headings; `text-wrap: pretty` en párrafos (evita viudas; soporte moderno, degrada sin daño).
