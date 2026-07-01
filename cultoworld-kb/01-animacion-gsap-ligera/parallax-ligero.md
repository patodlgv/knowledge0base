# Parallax ligero (solo transform, sin canvas)

> **Tier: LIGERO** · **Costo de perf: BAJO-MEDIO** — solo `transform` (GPU); el costo es de scroll listeners, que ScrollTrigger optimiza solo.

## 1. Parallax de imagen dentro de su sección (el más usado)

La imagen se mueve más lento que el scroll. `scrub: true` la ata al scroll — como es UN solo transform, es barato (no confundir con scroll-scrub de frames, que sí está prohibido en ligero):

```html
<section class="media">
  <div class="media-mask"><img class="media-img" src="…" alt="…"></div>
</section>
<style>
  .media-mask { overflow: hidden; }
  .media-img  { width: 100%; height: 120%; object-fit: cover; display: block; }
  /* la img es 20% más alta que su máscara = espacio para moverse */
</style>
```
```js
gsap.to(".media-img", {
  yPercent: -20, ease: "none",
  scrollTrigger: { trigger: ".media", start: "top bottom", end: "bottom top", scrub: true }
});
```

## 2. Capas a distintas velocidades (profundidad en el hero)

```js
gsap.utils.toArray("[data-speed]").forEach((el) => {
  gsap.to(el, {
    yPercent: parseFloat(el.dataset.speed) * -100,   // data-speed="0.2" → -20%
    ease: "none",
    scrollTrigger: { trigger: el.parentElement, start: "top bottom", end: "bottom top", scrub: true }
  });
});
```
```html
<div class="hero">
  <img data-speed="0.10" class="bg" …>
  <h1  data-speed="0.25">Titular</h1>
  <img data-speed="0.45" class="fg" …>
</div>
```
Más `data-speed` = más "cerca" del ojo. 2–3 capas bastan; más es ruido.

## 3. Parallax con el mouse (hero desktop)

```js
if (matchMedia("(pointer: fine)").matches) {
  const layers = gsap.utils.toArray("[data-depth]");
  addEventListener("mousemove", (e) => {
    const x = (e.clientX / innerWidth - 0.5), y = (e.clientY / innerHeight - 0.5);
    layers.forEach((el) => {
      const d = parseFloat(el.dataset.depth);        // data-depth="30" → máx 30px
      gsap.to(el, { x: x * d, y: y * d, duration: 0.8, ease: "power2.out", overwrite: "auto" });
    });
  });
}
```

## 4. Texto gigante que se desplaza horizontal al scroll (marquee scrubbed)

Firma visual barata para separar secciones:

```html
<div class="strip"><h2 class="strip-text">CULTOWORLD — CULTOWORLD — CULTOWORLD</h2></div>
<style>.strip { overflow: hidden; } .strip-text { white-space: nowrap; font-size: clamp(3rem, 12vw, 10rem); }</style>
```
```js
gsap.to(".strip-text", {
  xPercent: -30, ease: "none",
  scrollTrigger: { trigger: ".strip", start: "top bottom", end: "bottom top", scrub: true }
});
```

## Reglas de performance

1. Solo `transform` (x/y/xPercent/yPercent). JAMÁS `background-position` ni `top`.
2. `ease: "none"` en todo lo scrubbed.
3. Desactivar en móvil lo que no aporte (el parallax de mouse no existe; el de scroll puede reducirse):

```js
gsap.matchMedia().add("(min-width: 768px)", () => {
  // parallax solo desktop; en móvil la imagen queda estática
});
```

4. Imágenes de capas: comprimidas y del tamaño real (el parallax no perdona un PNG de 2 MB).

## Cuándo SÍ / Cuándo NO

- ✅ Hero y secciones de media del tier ligero: profundidad sin un byte de WebGL.
- ✅ 2–3 capas máximo por sección.
- ❌ NO en secciones con mucho texto corrido (marea al leer).
- ❌ NO parallax de fondo fijo con `background-attachment: fixed` (roto en iOS, mata perf).
- ❌ Scroll-scrub de secuencias de imágenes/frames → PROHIBIDO en ligero (eso es `02-animacion-premium`).
