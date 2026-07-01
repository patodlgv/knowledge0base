# Accesibilidad de color, escalas y extracción de paleta

> **Tier: AMBOS** · El contraste no es negociable en ningún tier.

## WCAG — los números exactos

| Elemento | AA | AAA |
|---|---|---|
| Texto cuerpo (<24px / <18.66px bold) | ≥ 4.5:1 | ≥ 7:1 |
| Texto grande (≥24px o ≥18.66px bold) | ≥ 3:1 | ≥ 4.5:1 |
| Componentes UI, bordes de input, focus | ≥ 3:1 | — |

Reglas prácticas:
- El error más común: `--muted` (gris secundario) sobre fondo claro. Verificar, no ojear.
- Placeholder de inputs también cuenta como texto.
- Nunca comunicar solo con color (error rojo → también icono/texto).
- Herramientas: webaim.org/resources/contrastchecker, DevTools (el hover del color picker muestra el ratio), o programático:

```js
// ratio de contraste WCAG (fórmula oficial W3C)
function luminance(hex) {
  const rgb = hex.match(/\w\w/g).map((c) => {
    const v = parseInt(c, 16) / 255;
    return v <= 0.03928 ? v / 12.92 : ((v + 0.055) / 1.055) ** 2.4;
  });
  return 0.2126 * rgb[0] + 0.7152 * rgb[1] + 0.0722 * rgb[2];
}
function contrast(a, b) {
  const [l1, l2] = [luminance(a), luminance(b)].sort((x, y) => y - x);
  return (l1 + 0.05) / (l2 + 0.05);
}
// contrast("#0A1128", "#F6F4F1") → 16.9 ✓
```

## Generar una escala desde un color base (OKLCH — el método moderno)

OKLCH es perceptualmente uniforme: cambiar L se ve como el mismo "paso" en cualquier hue (HSL no). Soportado en todos los navegadores modernos.

```css
:root {
  --brand-h: 250;         /* hue del cliente */
  --brand-c: 0.12;        /* chroma */

  --brand-50:  oklch(97% calc(var(--brand-c) * 0.15) var(--brand-h));
  --brand-100: oklch(93% calc(var(--brand-c) * 0.3)  var(--brand-h));
  --brand-200: oklch(86% calc(var(--brand-c) * 0.5)  var(--brand-h));
  --brand-400: oklch(66% var(--brand-c) var(--brand-h));
  --brand-600: oklch(50% var(--brand-c) var(--brand-h));   /* base usable en CTA */
  --brand-800: oklch(32% calc(var(--brand-c) * 0.8) var(--brand-h));
  --brand-950: oklch(18% calc(var(--brand-c) * 0.5) var(--brand-h));
}
```

Cambias `--brand-h` y toda la escala se regenera → perfecto para la fábrica de demos (un theming por cliente en 2 líneas). Aproximaciones de contraste sobre blanco: L 50% ≈ 4.5:1, L 40% ≈ 7:1 (verificar siempre).

Con JS (para tooling): librería `culori` (`npm i culori`) — `formatHex(oklch({ l: 0.5, c: 0.12, h: 250 }))`.

## Variaciones al vuelo con color-mix (hovers, tintes)

```css
.btn:hover { background: color-mix(in oklch, var(--accent), black 12%); }
.badge     { background: color-mix(in oklch, var(--accent), transparent 88%); color: var(--accent-deep); }
.border    { border-color: color-mix(in oklch, var(--ink), transparent 85%); }
```

## Extraer paleta desde foto/logo del cliente

### Workflow manual (el bueno para la fábrica)
1. Sacar los dominantes del logo/fotos (DevTools eyedropper o node-vibrant).
2. El dominante NO suele servir de acento tal cual → llevarlo a OKLCH y ajustar L hasta pasar 4.5:1 sobre el fondo elegido.
3. Neutros teñidos: gris con 3–5% del chroma del hue de marca.
4. Verificar los 4–5 pares reales (ink/paper, accent/paper, ink-inverse/accent, muted/paper).

### Programático (node-vibrant)

```js
// npm i node-vibrant
import { Vibrant } from "node-vibrant/node";

const palette = await Vibrant.from("logo-cliente.png").getPalette();
console.log({
  vibrant: palette.Vibrant?.hex,       // candidato a acento
  dark:    palette.DarkMuted?.hex,     // candidato a tinta
  light:   palette.LightMuted?.hex,    // candidato a fondo
});
```

### Desde una foto hero
La paleta puede salir de la foto principal del negocio (el interior del restaurante, la fachada) → cohesión automática foto-UI. Tomar un tono medio-oscuro para tinta y uno saturado para acento, ajustados en OKLCH.

## Dark mode con los mismos tokens

```css
:root { --ink: #141414; --paper: #f7f6f2; --accent: #0a5cff; }
[data-theme="dark"] {
  --ink: #eaeaea; --paper: #101014;
  --accent: oklch(from var(--accent) calc(l + 0.12) c h);  /* el acento sube L en oscuro */
}
```
Los componentes no cambian: solo los tokens. Acentos saturados suelen necesitar +10-15% de L sobre fondo oscuro para mantener contraste.

## Cuándo SÍ / Cuándo NO

- ✅ OKLCH para toda escala nueva; `color-mix` para estados.
- ✅ AAA (7:1) en texto largo de sitios de salud/legal (audiencia mayor).
- ❌ NO generar 10 escalas tipo Tailwind completas por proyecto — 1 escala de marca + neutros basta; más tokens = menos sistema.
- ❌ NO confiar en el color "del logo" sin ajustar L — los logos casi nunca pasan contraste como color de texto/CTA.
