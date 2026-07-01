# Micro-interacciones premium

> **Tier: AMBOS** · **Costo de perf: BAJO** — no tocan LCP; cuidar INP (handlers ligeros).
> Regla de oro: si es un cambio de estado A→B (hover, focus), **CSS puro basta y es más barato**. GSAP entra cuando hay inercia, secuencia o física.

## 1. Botón premium — CSS puro (el default)

```css
.btn {
  --lift: -2px;
  display: inline-block; padding: 1rem 2rem; border-radius: 4px;
  background: var(--accent); color: var(--ink-inverse);
  font-weight: 600; cursor: pointer; border: 0;
  transition: transform .25s cubic-bezier(.22,1,.36,1),
              box-shadow .25s cubic-bezier(.22,1,.36,1);
}
.btn:hover  { transform: translateY(var(--lift)); box-shadow: 0 10px 30px rgb(0 0 0 / .18); }
.btn:active { transform: translateY(0); transition-duration: .1s; }
.btn:focus-visible { outline: 2px solid currentColor; outline-offset: 3px; }
```

`cubic-bezier(.22,1,.36,1)` ≈ power3.out. Guárdalo como token: `--ease-out-premium`.

## 2. Botón con relleno que barre (efecto "wipe")

```css
.btn-wipe {
  position: relative; overflow: hidden; isolation: isolate;
  background: transparent; color: var(--accent);
  border: 1px solid var(--accent); transition: color .35s;
}
.btn-wipe::before {
  content: ""; position: absolute; inset: 0; z-index: -1;
  background: var(--accent);
  transform: translateY(101%); transition: transform .35s cubic-bezier(.22,1,.36,1);
}
.btn-wipe:hover { color: var(--ink-inverse); }
.btn-wipe:hover::before { transform: translateY(0); }
```

## 3. Link con subrayado que crece (el clásico editorial)

```css
.link {
  position: relative; text-decoration: none;
  background-image: linear-gradient(currentColor, currentColor);
  background-size: 0% 1px; background-position: 0 100%; background-repeat: no-repeat;
  transition: background-size .3s cubic-bezier(.22,1,.36,1);
}
.link:hover { background-size: 100% 1px; }
```

## 4. Botón magnético (GSAP — aquí sí, porque hay inercia)

El hover que "jala" el botón hacia el cursor. Solo desktop (pointer fino):

```js
if (matchMedia("(pointer: fine)").matches) {
  document.querySelectorAll(".magnetic").forEach((el) => {
    const strength = 0.3;
    el.addEventListener("mousemove", (e) => {
      const r = el.getBoundingClientRect();
      gsap.to(el, {
        x: (e.clientX - r.left - r.width / 2) * strength,
        y: (e.clientY - r.top - r.height / 2) * strength,
        duration: 0.4, ease: "power2.out"
      });
    });
    el.addEventListener("mouseleave", () =>
      gsap.to(el, { x: 0, y: 0, duration: 0.6, ease: "elastic.out(1, 0.4)" }));
  });
}
```

El `elastic.out` en la salida es lo que lo hace sentirse caro.

## 5. Card con tilt "3D falso" (GSAP + perspective)

Validado en sitios de Culto. El padre necesita `perspective: 800px`:

```css
.cards-wrap { perspective: 800px; }
.card-3d { transform-style: preserve-3d; }
```
```js
document.querySelectorAll(".card-3d").forEach((card) => {
  card.addEventListener("mousemove", (e) => {
    const r = card.getBoundingClientRect();
    const rx = ((e.clientY - r.top) / r.height - 0.5) * -10;
    const ry = ((e.clientX - r.left) / r.width - 0.5) * 10;
    gsap.to(card, { rotateX: rx, rotateY: ry, duration: 0.4, ease: "power2.out" });
  });
  card.addEventListener("mouseleave", () =>
    gsap.to(card, { rotateX: 0, rotateY: 0, duration: 0.6, ease: "power3.out" }));
});
```

## 6. Transición de estado (menú móvil, acordeón, tabs)

Timeline pausada + `play()/reverse()` — el patrón GSAP para toggles:

```js
const tl = gsap.timeline({ paused: true, defaults: { ease: "power3.out" } })
  .to(".menu", { clipPath: "inset(0 0 0% 0)", duration: 0.5 })
  .from(".menu a", { y: 24, opacity: 0, stagger: 0.06, duration: 0.4 }, "-=0.2");

let open = false;
btn.addEventListener("click", () => {
  open = !open;
  open ? tl.play() : tl.reverse();
  btn.setAttribute("aria-expanded", open);   // a11y siempre
});
```

CSS inicial: `.menu { clip-path: inset(0 0 100% 0); }`

## 7. Cursor custom con inercia (lerp + un solo rAF)

**Tier: AMBOS, pero opcional en ligero** (solo desktop; en ligero, versión simple sin blend modes). El "feel caro" viene del lerp:

```js
if (matchMedia("(pointer: fine)").matches) {
  const cursor = document.querySelector(".cursor");
  const pos = { x: 0, y: 0 }, target = { x: 0, y: 0 };
  const lerp = (a, b, t) => a + (b - a) * t;

  addEventListener("mousemove", (e) => { target.x = e.clientX; target.y = e.clientY; });

  gsap.ticker.add(() => {          // usar el ticker de GSAP = un solo loop global
    pos.x = lerp(pos.x, target.x, 0.12);
    pos.y = lerp(pos.y, target.y, 0.12);
    gsap.set(cursor, { x: pos.x, y: pos.y });
  });

  // estados por data-attribute (patrón Unseen)
  document.querySelectorAll("[data-cursor]").forEach((el) => {
    el.addEventListener("mouseenter", () => cursor.dataset.state = el.dataset.cursor);
    el.addEventListener("mouseleave", () => cursor.dataset.state = "default");
  });
}
```

## Cuándo SÍ / Cuándo NO

- ✅ Hover en TODO elemento interactivo: un sitio sin hover states se siente muerto.
- ✅ CSS puro para A→B; GSAP para inercia/secuencia/elastic.
- ✅ `matchMedia("(pointer: fine)")` para todo lo que dependa de mouse.
- ❌ NO magnetic/tilt en móvil (no hay cursor; desperdicia listeners).
- ❌ NO cursor custom en landings de conversión pura (distrae del CTA). Sí en demos con carácter y premium.
- ❌ NO animar `box-shadow` en elementos grandes con GSAP cada frame (caro de pintar) — en hover con transition CSS está bien.
