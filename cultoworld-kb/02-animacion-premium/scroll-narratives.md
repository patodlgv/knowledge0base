# Scroll-driven narratives (scrollytelling)

> **Tier: PREMIUM** · **Costo de perf: MEDIO-ALTO** · La narrativa como PRINCIPIO aplica a ambos tiers; la maquinaria de scroll-cinema solo al premium.

## El principio (siempre encendido, en ambos tiers)

Toda página es un viaje: tensión → resolución. La pregunta que estructura: **"¿qué cambia para el visitante?"**

- **Tier ligero:** la narrativa vive en el COPY y el orden de secciones (problema → agitación → solución → prueba → transformación). Cero scroll-cinema.
- **Tier premium:** la narrativa se despliega físicamente con el scroll — capítulos pinneados, reveals coreografiados, un pico emocional y un payoff (el CTA).

## Estructura de un scrollytelling premium

```
PRELOADER (0-100 con carácter, ver abajo)
  ↓
HERO — el hook en 50ms. Una promesa/pregunta, no un menú de features
  ↓
CAPÍTULO 1..N — cada uno una sección pinneada con SU beat de la historia
  (usar los patrones de scrolltrigger-avanzado.md: pin+scrub, horizontal, stacking)
  ↓
PICO EMOCIONAL — el momento de mayor espectáculo (aquí va el shader/partículas si hay)
  ↓
RESOLUCIÓN + CTA — qué gana el visitante; salida clara
```

Reglas de pacing:
- Alternar denso ↔ aireado. Después de un capítulo scrubbed intenso, una sección estática que respira.
- `end: "+=N"` controla el tempo: momentos clave largos (+=2500), transiciones cortas (+=800).
- **Nunca atrapar al usuario:** nav siempre accesible con anclas para saltar a contacto/precios.

## Preloader narrativo (el estándar award-winning)

Contador 0–100 + palabras + wipe de salida. El progreso REAL viene de los assets:

```js
// con Three.js: THREE.LoadingManager da el progreso real
const manager = new THREE.LoadingManager();
const progress = { value: 0 };

manager.onProgress = (url, loaded, total) => {
  gsap.to(progress, { value: loaded / total, duration: 0.5, ease: "power2.out",
    onUpdate: () => counter.textContent = Math.round(progress.value * 100) });
};

manager.onLoad = () => {
  gsap.timeline()
    .to(".preloader-counter", { yPercent: -110, duration: 0.6, ease: "power3.in" })
    .to(".preloader", { clipPath: "inset(0 0 100% 0)", duration: 0.9, ease: "power4.inOut" })
    .from(".hero-line", { yPercent: 110, stagger: 0.1, duration: 0.9, ease: "power3.out" }, "-=0.3")
    .set(".preloader", { display: "none" });
};
```

Sin WebGL: `Promise.all([document.fonts.ready, ...imgs.map(decode)])` como señal de carga.

## Sincronizar DOM + canvas en una sola timeline (patrón Lusion/Active Theory)

La coherencia que premian los jurados: mismo easing y timing para shader y DOM:

```js
gsap.timeline({ scrollTrigger: { trigger: ".chapter", start: "top top", end: "+=2000", pin: true, scrub: 1 } })
  .to(material.uniforms.uProgress, { value: 1, ease: "none" }, 0)   // el shader
  .from(".chapter h2", { yPercent: 80, opacity: 0 }, 0.1)            // el DOM
  .to(camera.position, { z: 3, ease: "none" }, 0);                   // la cámara
```

## Cámara que recorre una curva con el scroll (mundo 3D scroll-driven)

```js
const curve = new THREE.CatmullRomCurve3([
  new THREE.Vector3(0, 0, 5), new THREE.Vector3(2, 1, 2),
  new THREE.Vector3(0, 2, -2), new THREE.Vector3(-3, 1, -6)
]);

const state = { progress: 0 };
gsap.to(state, {
  progress: 1, ease: "none",
  scrollTrigger: { trigger: ".world", start: "top top", end: "+=6000", scrub: 1, pin: true }
});

// en el tick():
const pos = curve.getPointAt(state.progress);
camera.position.lerp(pos, 0.1);                       // lerp = suavidad extra
camera.lookAt(curve.getPointAt(Math.min(state.progress + 0.02, 1)));
```

## Reduced motion en narrativas

La historia debe existir sin movimiento: mismo contenido, secciones estáticas apiladas.

```js
gsap.matchMedia().add("(prefers-reduced-motion: reduce)", () => {
  // no crear pins ni scrubs; el CSS por defecto muestra todo apilado y legible
});
```

## Cuándo SÍ / Cuándo NO

- ✅ Sitio insignia, páginas de marca/manifesto, case studies del portafolio premium.
- ✅ Cuando el cliente compra "experiencia" y el contenido tiene historia real que contar.
- ❌ NO donde el objetivo es lead/venta directa: la conversión odia los túneles.
- ❌ NO si no hay contenido narrativo real — scrollytelling de relleno se nota y aburre.
- ⚠️ SEO: el texto de la narrativa debe estar en el DOM real (no en canvas). Si el sitio también necesita búsqueda, usar híbrido: páginas indexables + la experiencia en la landing de marca.
