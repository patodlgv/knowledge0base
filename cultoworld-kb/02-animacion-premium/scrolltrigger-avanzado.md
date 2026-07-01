# ScrollTrigger avanzado: pinning, scrub, timelines cinemáticas

> **Tier: PREMIUM** (y plan intermedio con moderación) · **Costo de perf: MEDIO-ALTO**
> ⚠️ **NO va en la fábrica de demos.** `pin` + `scrub` cambian el layout y consumen main thread. Un solo `pin` mal puesto arruina el INP en móvil de gama media.

## Smooth scroll con Lenis (la base del feel premium)

Integración canónica Lenis + ScrollTrigger (fuente: docs oficiales de darkroom.engineering):

```js
import Lenis from "lenis";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
gsap.registerPlugin(ScrollTrigger);

const lenis = new Lenis({ lerp: 0.1 });        // 0.06–0.08 = feel "pesado" tipo Obys
lenis.on("scroll", ScrollTrigger.update);
gsap.ticker.add((time) => lenis.raf(time * 1000));
gsap.ticker.lagSmoothing(0);
```

Lenis envuelve el scroll nativo: `position: sticky`, anclas y accesibilidad siguen funcionando (a diferencia del viejo Locomotive Scroll). Alternativa: ScrollSmoother de GSAP (ya gratis), que además da `data-speed`/`data-lag` declarativos.

## 1. Pin + scrub: la sección que se queda mientras la historia corre

```js
const tl = gsap.timeline({
  scrollTrigger: {
    trigger: ".chapter",
    start: "top top",
    end: "+=2000",          // 2000px de scroll = duración del capítulo
    pin: true,
    scrub: 1,               // 1s de "catch-up" — más cinemático que scrub:true
    anticipatePin: 1        // evita el salto al pinnear
  }
});
tl.from(".chapter h2",   { yPercent: 60, opacity: 0 })
  .to(".chapter .img-a", { scale: 1.1, xPercent: -8 }, 0)
  .fromTo(".chapter .text-b", { opacity: 0 }, { opacity: 1 }, 0.4)
  .to(".chapter .img-a", { opacity: 0 }, 0.7)
  .from(".chapter .img-b", { clipPath: "inset(100% 0 0 0)" }, 0.7);
```

Claves:
- En timelines scrubbed, las posiciones (0, 0.4, 0.7) son proporciones del scroll, no segundos.
- `scrub: 1–2` para lo cinemático; `scrub: true` para conexión 1:1 (parallax).
- `end: "+=N"`: más N = capítulo más lento/contemplativo.

## 2. Scroll horizontal con pin (galería de proyectos)

```js
const track = document.querySelector(".track");
const panels = gsap.utils.toArray(".panel");

gsap.to(track, {
  xPercent: -100 * (panels.length - 1),
  ease: "none",
  scrollTrigger: {
    trigger: ".horizontal",
    pin: true,
    scrub: 1,
    snap: 1 / (panels.length - 1),        // opcional: imanta a cada panel
    end: () => "+=" + track.offsetWidth   // función = se recalcula en resize
  }
});
```
```css
.horizontal { overflow: hidden; }
.track { display: flex; width: max-content; }
.panel { width: 100vw; height: 100vh; flex-shrink: 0; }
```

Animaciones internas de cada panel usan `containerAnimation`:

```js
gsap.from(".panel-2 h3", {
  y: 60, opacity: 0,
  scrollTrigger: { trigger: ".panel-2", containerAnimation: horizontalTween, start: "left 70%" }
});
```

## 3. Secciones apiladas (stacking cards)

Cada sección se pinnea y la siguiente la cubre:

```js
gsap.utils.toArray(".stack-panel").forEach((panel, i, arr) => {
  ScrollTrigger.create({
    trigger: panel, start: "top top",
    pin: true, pinSpacing: false,           // sin espacio = la siguiente encima
    end: () => (i === arr.length - 1 ? "+=0" : "bottom top")
  });
  if (i > 0) gsap.from(panel, {             // la que entra puede escalar/oscurecer a la anterior
    scrollTrigger: { trigger: panel, start: "top bottom", end: "top top", scrub: true }
  });
});
```

## 4. Skew por velocidad de scroll (aproximación ligera a la distorsión WebGL)

El clásico "imágenes que se inclinan según qué tan rápido scrolleas" — sin shaders:

```js
let proxy = { skew: 0 },
    skewSetter = gsap.quickSetter(".skew-img", "skewY", "deg"),
    clamp = gsap.utils.clamp(-8, 8);

ScrollTrigger.create({
  onUpdate: (self) => {
    const skew = clamp(self.getVelocity() / -300);
    if (Math.abs(skew) > Math.abs(proxy.skew)) {
      proxy.skew = skew;
      gsap.to(proxy, { skew: 0, duration: 0.8, ease: "power3",
        overwrite: true, onUpdate: () => skewSetter(proxy.skew) });
    }
  }
});
```
(Patrón oficial de los demos de GSAP. `quickSetter` = setter optimizado para updates por frame.)

## 5. Marquee reactivo a velocidad de scroll

```js
const marquee = gsap.to(".marquee-inner", { xPercent: -50, repeat: -1, ease: "none", duration: 18 });

ScrollTrigger.create({
  onUpdate: (self) => {
    const v = gsap.utils.clamp(-4, 4, self.getVelocity() / 400);
    gsap.to(marquee, { timeScale: v < 0 ? -1 + v : 1 + v, duration: 0.4, overwrite: true });
    gsap.to(marquee, { timeScale: v < 0 ? -1 : 1, duration: 1, delay: 0.4, overwrite: false });
  }
});
```

## Errores comunes (de la guía oficial "ScrollTrigger tips & mistakes")

1. **Nunca anidar ScrollTriggers dentro de tweens de una timeline** — el scrubbing entra en conflicto. Un ScrollTrigger por timeline, en la config de la timeline.
2. `start`/`end` con funciones si el layout cambia (`end: () => ...`) + `ScrollTrigger.refresh()` tras cargar imágenes/fonts.
3. Crear los triggers en orden DOM, o usar `refreshPriority`.
4. Con Lenis/pin: el contenedor pinneado NO debe tener `transform` de un ancestro (rompe el pin).
5. En React: TODO dentro de `useGSAP` — los pins dejan huérfanos los spacers si no se limpian.

## Cuándo SÍ / Cuándo NO

- ✅ Sitio insignia de Culto, clientes $14,500+, páginas "about/manifesto" de marca.
- ✅ Plan intermedio: MÁXIMO 1 sección pinneada por página, sin scroll horizontal.
- ❌ Landing de conversión: el visitante que quiere el precio no debe atravesar 3 capítulos pinneados (túnel = fuga).
- ❌ Fábrica de demos: nada de pin/scrub. El parallax scrubbed simple (ver `01/parallax-ligero.md`) es la excepción permitida.
- ❌ Móvil: considera desactivar pins con `gsap.matchMedia` y dejar la versión lineal.
