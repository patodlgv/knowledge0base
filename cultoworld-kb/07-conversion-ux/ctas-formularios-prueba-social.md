# CTAs, formularios y prueba social (los componentes que convierten)

> **Tier: LIGERO** · Componentes listos para la fábrica, con la micro-animación que los hace sentirse premium sin dañar conversión.

## CTAs

### Jerarquía
- **Primario**: 1 por pantalla, color acento, la acción del negocio.
- **Secundario**: outline/ghost ("Ver menú", "Conocer más") — nunca compite en color.
- Texto = verbo + beneficio + fricción cero: "Agenda gratis", "Pide por WhatsApp", "Cotiza en 1 minuto".

### WhatsApp CTA (el rey en MX)

```html
<a class="btn btn-wa"
   href="https://wa.me/5218112345678?text=Hola%2C%20quiero%20agendar%20una%20cita"
   target="_blank" rel="noopener">
  <svg aria-hidden="true" …><!-- icono WA --></svg>
  Escríbenos por WhatsApp
</a>
```
Prellenar el mensaje baja la fricción (el cliente solo da "enviar"). Trackear con `gtag('event', 'whatsapp_click')`.

### Micro-detalle premium del CTA
Hover lift + wipe (ver `01/micro-interacciones.md`). Para el CTA final, entrada con `back.out(1.4)` + un pulso sutil de sombra cada ~6s:

```js
gsap.to(".cta-final", { boxShadow: "0 0 0 12px color-mix(in srgb, var(--accent) 0%, transparent)",
  duration: 1.2, repeat: -1, repeatDelay: 5, ease: "power2.out" });
```
(un solo elemento, animación espaciada = INP intacto)

## Formularios

### Reglas
- ≤ 5 campos; ideal 2–3 (nombre, WhatsApp, [motivo]).
- Labels visibles SIEMPRE (placeholder no es label — se olvida al escribir y falla a11y).
- Un campo por fila en móvil; `inputmode`/`type` correctos (`type="tel"` abre teclado numérico).
- Validación inline al `blur`, no al submit; errores con texto + color (no solo color).
- El botón dice el beneficio: "Recibir cotización" > "Enviar".

### Form base accesible

```html
<form class="lead-form" novalidate>
  <div class="field">
    <label for="nombre">Nombre</label>
    <input id="nombre" name="nombre" type="text" autocomplete="name" required>
    <p class="error" role="alert" hidden>Escribe tu nombre</p>
  </div>
  <div class="field">
    <label for="tel">WhatsApp</label>
    <input id="tel" name="tel" type="tel" inputmode="tel" autocomplete="tel" required>
    <p class="error" role="alert" hidden>Necesitamos tu número para responderte</p>
  </div>
  <button class="btn" type="submit">Recibir cotización</button>
</form>
```

```css
.field input {
  width: 100%; padding: .9rem 1rem; font-size: 1rem;   /* ≥16px: iOS no hace zoom */
  border: 1px solid var(--line); border-radius: 4px; background: var(--surface);
  transition: border-color .2s, box-shadow .2s;
}
.field input:focus-visible {
  outline: none; border-color: var(--accent);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--accent) 20%, transparent);
}
.field input[aria-invalid="true"] { border-color: #c0392b; }
```

### Micro-animaciones del form (sutiles, útiles)
- Shake en error (300ms): `gsap.fromTo(field, { x: 0 }, { x: 8, duration: 0.06, repeat: 5, yoyo: true, ease: "none" })`
- Éxito: swap del form a mensaje de confirmación con `clipPath` + check SVG dibujado (`DrawSVGPlugin`, ya gratis).
- Multi-step (si el negocio pide calificar leads): 1 pregunta por paso con barra de progreso — un primer paso de baja fricción ("¿Qué servicio buscas?") convierte mejor que el muro de campos.

## Prueba social

### Orden de potencia
1. **Números específicos animados** (contadores): "1,247 clientes", "4.9★ de 386 reseñas"
2. **Testimonios con cara + nombre + contexto** (1 bueno > 5 genéricos)
3. **Logos** (B2B) / **badges** (certificaciones, "verificado en Google")
4. **Antes/después** (gym, estética, remodelación)

### Card de testimonio

```html
<figure class="testimonial">
  <blockquote>"Llegué con miedo al dentista y salí sonriendo. Todo el proceso claro y sin dolor."</blockquote>
  <figcaption>
    <img src="ana.avif" alt="" width="48" height="48" loading="lazy">
    <div><strong>Ana Martínez</strong><span>Ortodoncia · Paciente desde 2024</span></div>
  </figcaption>
</figure>
```

Entrada con stagger (ver `01/scroll-reveals.md`). Si son >3, marquee lento pausable (`.hover → timeScale: 0`) mejor que carrusel con flechas (nadie las usa).

### Slider antes/después (gym/estética, sin librería)

```html
<div class="ba" style="--pos: 50%">
  <img src="antes.avif" alt="Antes">
  <img src="despues.avif" alt="Después" class="ba-top">
  <input type="range" min="0" max="100" value="50" aria-label="Comparar antes y después">
</div>
<style>
.ba { position: relative; } .ba img { display: block; width: 100%; }
.ba-top { position: absolute; inset: 0; clip-path: inset(0 calc(100% - var(--pos)) 0 0); }
.ba input { position: absolute; inset: 0; opacity: 0; width: 100%; cursor: ew-resize; }
</style>
<script>
document.querySelectorAll(".ba").forEach((el) => {
  el.querySelector("input").addEventListener("input", (e) =>
    el.style.setProperty("--pos", e.target.value + "%"));
});
</script>
```

### Rating animado

```js
// estrellas que se llenan + contador (dispara una vez, al verse)
const tl = gsap.timeline({ scrollTrigger: { trigger: ".rating", start: "top 85%" } });
tl.from(".star", { scale: 0, stagger: 0.08, ease: "back.out(2)" })
  .from(".rating-count", { textContent: 0, snap: { textContent: 1 }, duration: 1.2 }, "<");
```

## Jerarquía visual (recordatorio operativo)

- Patrón F para páginas con texto; Z para heros. El CTA vive donde termina el barrido del ojo.
- Señales direccionales funcionan: flecha sutil o mirada de la persona de la foto HACIA el form.
- Espacio en blanco alrededor del CTA = más clicks (no lo encajones).
- Un solo punto focal por viewport de scroll.
