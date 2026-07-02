# Data-attribute injection (patrón Unseen)

> **Tier: AMBOS** · **Costo: BAJO** · Declarar la intención en el HTML con `data-*`; UNA sola pieza de JS lee y aplica el comportamiento. Escala infinitamente mejor que cablear elemento por elemento.

## El patrón

```html
<a data-cursor="drag">…</a>
<img data-speed="0.25" …>        <!-- parallax -->
<h2 data-reveal="lines">…</h2>   <!-- tipo de reveal -->
<section data-theme-trigger="dark">…</section>
```

```js
// un solo módulo por comportamiento, descubre sus targets solo:
document.querySelectorAll("[data-reveal]").forEach((el) => {
  const type = el.dataset.reveal;              // "lines" | "fade" | "cards"
  REVEALS[type]?.(el);                          // mapa de funciones
});

const REVEALS = {
  lines: (el) => {/* SplitText + yPercent (cultoworld-kb/01/scroll-reveals.md §4) */},
  fade:  (el) => gsap.from(el, { y: 40, opacity: 0,
           scrollTrigger: { trigger: el, start: "top 80%" } }),
  cards: (el) => {/* batch stagger */},
};
```

## Por qué es oro para la fábrica

- El markup de cada demo DECLARA su animación; el motor JS es el mismo en los cientos de sitios.
- Cambiar un comportamiento = tocar 1 función, no N páginas.
- Claude Code puede generar solo el HTML con data-attributes y el sitio ya queda animado.

## Usos ya documentados en el vault

`data-cursor` (estados del cursor custom, `01/micro-interacciones.md` §7) · `data-speed` (capas parallax, `01/parallax-ligero.md` §2) · `data-depth` (parallax de mouse) · `data-theme-trigger` (cambio de tema por sección) · `data-target` (contadores).

Regla: los `data-*` son la API pública de cada página; documenta los valores válidos en el motor.
