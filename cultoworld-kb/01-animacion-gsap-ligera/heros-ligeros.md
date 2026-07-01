# Catálogo de heros ligeros con carácter (por tipo de negocio)

> **Tier: LIGERO** · **Costo de perf: BAJO** si se respetan las reglas del final.
> El hero decide si el demo se siente "plantilla" o "estudio". Carácter = arquitectura distinta + 1 movimiento memorable, NO más peso.

## Anatomía común (todos los heros)

```
1. Entrada coreografiada (timeline única, 1–1.5s total, empieza de inmediato)
2. Headline de beneficio ≤ 8 palabras (ver 07-conversion-ux)
3. CTA visible sin scroll en móvil
4. 1 elemento "vivo" persistente (parallax de mouse, marquee, badge que respira)
```

La timeline de entrada base (ajustar por patrón):

```js
const tl = gsap.timeline({ defaults: { ease: "power3.out" } });
tl.from(".hero-line", { yPercent: 110, duration: 0.9, stagger: 0.12 })
  .from(".hero-sub",  { opacity: 0, y: 20, duration: 0.6 }, "-=0.4")
  .from(".hero-cta",  { opacity: 0, y: 16, duration: 0.5 }, "-=0.3")
  .from(".hero-media",{ clipPath: "inset(100% 0 0 0)", duration: 1, ease: "power3.inOut" }, 0.2);
```

---

## Patrón A — "Editorial split" (clínica, legal, spa, consultoría)

Grid asimétrico 60/40: texto grande a la izquierda, imagen con máscara-cortina a la derecha. Serenidad + autoridad.

```css
.hero { display: grid; grid-template-columns: 1.2fr 1fr; gap: var(--s6); align-items: center; min-height: 90svh; }
@media (max-width: 767px) { .hero { grid-template-columns: 1fr; min-height: auto; padding-block: var(--s8); } }
```
Movimiento: reveal de líneas + imagen con `clipPath` + parallax sutil de la imagen al scroll (`yPercent: -10`, scrub).
Elemento vivo: badge de confianza ("+15 años") con `gsap.to(badge, { y: -6, repeat: -1, yoyo: true, duration: 2, ease: "sine.inOut" })`.

## Patrón B — "Full-bleed con tipografía gigante" (gym, barbería, agencia)

Imagen full-screen oscurecida + titular que ocupa el 80% del ancho, en 2–3 líneas escalonadas. Energía.

```css
.hero { position: relative; min-height: 100svh; display: grid; align-content: end; padding: var(--gutter); }
.hero h1 { font-size: clamp(3rem, 11vw, 9rem); line-height: 0.95; letter-spacing: -0.02em; text-transform: uppercase; }
.hero::before { content: ""; position: absolute; inset: 0; background: linear-gradient(180deg, transparent 30%, rgb(0 0 0 / .65)); z-index: -1; }
```
Movimiento: líneas del titular desde abajo con `stagger` grande (0.15) + la imagen de fondo con `scale: 1.15 → 1` durante 2s (`ease: "power2.out"`) — el "zoom de respiración" de las agencias.
Elemento vivo: marquee inferior con servicios (`xPercent: -50, repeat: -1, ease: "none", duration: 20`).

## Patrón C — "Producto flotante con 3D falso" (restaurante, producto, e-commerce)

Foto del platillo/producto recortada (PNG/WebP con alpha) flotando sobre un círculo de color, con tilt al mouse. El "wow 3D" sin canvas.

```css
.hero-media { position: relative; }
.hero-media::before { content: ""; position: absolute; inset: 10%; border-radius: 50%; background: var(--accent); filter: blur(0); z-index: -1; }
.stage { perspective: 900px; }
```
```js
// flotación permanente + tilt de mouse (ver micro-interacciones.md §5)
gsap.to(".hero-media", { y: -14, duration: 2.4, repeat: -1, yoyo: true, ease: "sine.inOut" });
```
Movimiento: producto entra con `scale: 0.8, opacity: 0, ease: "back.out(1.4)"`.
Peso: la foto recortada en WebP < 80 KB. Es el patrón más vendedor para comida.

## Patrón D — "Stack de cards en abanico" (bienes raíces, portafolios, hoteles)

3–4 fotos de propiedades en abanico rotado que se ordenan al entrar y responden al hover.

```js
const cards = gsap.utils.toArray(".fan-card");
gsap.from(cards, {
  y: 80, opacity: 0, rotation: 0, duration: 1, ease: "power3.out",
  stagger: 0.1,
});
gsap.set(cards, { rotation: (i) => (i - 1.5) * 6 });   // abanico: -9°, -3°, 3°, 9°
```
Elemento vivo: hover levanta la card (`y: -12, rotation: 0, zIndex: 10`).

## Patrón E — "Cifras + mapa de fondo" (despacho, fintech, logística)

Fondo con patrón SVG sutil (grid de puntos, mapa) + titular + fila de contadores animados (ver `contadores-skeletons-transiciones.md`). Autoridad con datos.

Elemento vivo: los puntos del SVG con opacidad pulsante vía CSS `@keyframes` (no JS).

## Patrón F — "Menú-tipografía" (barbería premium, marca personal, estudio creativo)

Sin imagen: solo tipografía descomunal con roll de caracteres al hover en el nav y un marquee. Carácter puro, peso mínimo (~0 imágenes = LCP instantáneo).

Roll de caracteres (patrón Obys/Locomotive, sin WebGL):
```html
<a class="roll" href="#"><span class="roll-inner"><span>Servicios</span><span aria-hidden="true">Servicios</span></span></a>
<style>
.roll { display: inline-block; overflow: hidden; }
.roll-inner { display: block; }
.roll-inner span { display: block; }
</style>
```
```js
document.querySelectorAll(".roll").forEach((el) => {
  const inner = el.querySelector(".roll-inner");
  el.addEventListener("mouseenter", () => gsap.to(inner, { yPercent: -50, duration: 0.4, ease: "power3.out" }));
  el.addEventListener("mouseleave", () => gsap.to(inner, { yPercent: 0, duration: 0.4, ease: "power3.out" }));
});
```

---

## Asignación rápida por nicho

| Nicho | Patrón | Por qué |
|---|---|---|
| Clínica / dental / spa | A | Calma, confianza, aire |
| Legal / contadores | A o E | Autoridad, datos |
| Gym / crossfit | B | Energía, impacto |
| Barbería | B o F | Actitud, tipografía |
| Restaurante / café | C | El producto ES el hero |
| Bienes raíces / hotel | D | Inventario visual |
| Fintech / logística | E | Números, seriedad |
| Estudio creativo / personal | F | Carácter tipográfico |

## Reglas para mantener <2s en móvil

1. La imagen del hero ES el LCP: AVIF/WebP, `fetchpriority="high"`, sin `loading="lazy"`, tamaño exacto con `srcset`. Meta: < 150 KB.
2. El `<h1>` en HTML real desde el server (SSR). La animación lo mueve, no lo crea.
3. Timeline de entrada arranca sin esperar imágenes (no `window.onload`).
4. Nada de video en el hero del tier ligero. Si el cliente insiste: poster + video < 1 MB, `preload="none"`, solo desktop.
5. `min-height: 100svh` (no `vh`) para evitar saltos con la barra de iOS → CLS.
