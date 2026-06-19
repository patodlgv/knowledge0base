
# 🎨 Sistema de Color — Construir una Paleta desde Cero

> Cómo construir una paleta profesional para cualquier cliente, paso a paso. Esto
> alimenta los PRIMITIVOS y la capa SEMÁNTICA de `design-tokens-system.md`. El
> objetivo: que cualquier sitio (de cualquier plan) se vea intencional y pase
> accesibilidad — calidad en todos los planes, baratos incluidos.

---

## 🧭 El método en 6 pasos (orden importa)

1. **Arranca de la marca, no de tus gustos.** Parte del logo/identidad del cliente
   y de la emoción que la marca quiere transmitir, no del color que a ti te guste.
2. **Elige el TIPO de paleta antes que los colores.** El tipo es la decisión que
   viene primero: monocromática, análoga, complementaria, split-complementaria o
   triádica — según el registro emocional del producto. Acertar el tipo hace que
   todas las decisiones siguientes sean más rápidas y consistentes.
3. **Elige UN hue primario (acento).** Todo lo demás se deriva de esa elección.
4. **Genera escalas de luminosidad.** Para cada hue base, una escala de 9-10 pasos
   (50, 100, 200 … 900). Esos son tus primitivos.
5. **Asigna roles con la regla 60-30-10** (abajo).
6. **Valida contraste (WCAG) y prueba en contexto** antes de comprometerte.

---

## ⚖️ La regla 60-30-10 (el corazón del balance)

Divide el uso del color en tres pesos. Es el estándar de la industria para
interfaces de alta conversión.

- **60% — Dominante (fondo y superficies grandes).** Casi siempre NEUTRO. Es "la
  pared de la galería" para que el contenido respire: blanco puro, gris muy claro
  (#F5F5F7) o carbón/navy oscuro (#121212) en modo oscuro.
- **30% — Secundario (estructura).** Cards, sidebars, navegación, headers/footers.
  Relacionado al fondo pero visiblemente separado. *Error común: que el 60 y el 30
  sean casi iguales → no hay agrupación visual. Empuja el secundario más oscuro o
  saturado.*
- **10% — Acento (acción).** RESERVADO exclusivamente para CTAs, estados activos,
  links, focus. Vibrante, contrasta fuerte contra el 60 y el 30.

> **El principio profundo: contraste por escasez.** Cuando tu acento solo aparece
> en botones primarios y estados activos, el usuario lo lee al instante como "aquí
> actúo". Si aparece en todos lados, deja de significar algo. (Spotify: fondo
> negro 60%, gris 30%, verde SOLO en play/toggles 10%. Por eso tu ojo va directo.)

> **El test de los 2 metros:** una vez puestos los 3 colores, aléjate de la
> pantalla. ¿La zona correcta (los CTAs) jala tu ojo sin pensar? Si sí, el acento
> hace su trabajo.

---

## 🌗 Oscuro vs Claro (NO es decorativo, es mayoría de usuarios)

La mayoría de usuarios tiene una preferencia de sistema (claro/oscuro). Una paleta
que se ve preciosa en claro pero se rompe (turbia, bajo contraste) en oscuro ya
está fallándole a la mayoría. **Diseña para ambos desde la estrategia, no como
caso extremo.** Como la capa semántica de tokens es la única que se toca, el modo
oscuro es re-mapear alias (ver `design-tokens-system.md`).

- **Fondo oscuro** (carbón #1A1A2E, negro #0A0A0A, navy): señala precisión técnica
  y sofisticación (GitHub, Vercel, herramientas dev). Bien para marca/premium/tech.
- **Fondo claro:** confianza, limpieza, contenido largo, e-commerce, editorial.
- **Regla no negociable en oscuro:** el contraste texto/fondo debe cumplir WCAG
  ≥4.5:1. El bajo contraste sobre fondo oscuro se vuelve ilegible en móvil bajo luz
  ambiental fuerte.

---

## ♿ Contraste y accesibilidad (calidad en TODOS los planes)

- **WCAG AA:** texto normal **4.5:1** mínimo contra su fondo. Texto grande (18pt
  regular / 14pt bold+) **3:1**. Elementos decorativos y deshabilitados, exentos.
- **Verifica cada par texto/fondo** con WebAIM Contrast Checker (segundos).
- **Nunca uses solo color para comunicar** significado — acompáñalo de texto,
  icono o patrón (daltonismo). Prueba con simulador de daltonismo.
- Negocios en EE.UU. que fallan WCAG tienen exposición legal ADA — para clientes
  serios, esto es argumento de venta, no solo buena práctica.

---

## 🧱 Cuántos colores en total

Una paleta de UI típica: 1-2 primarios de marca, 1 acento, una escala de grises
neutros (8-10 pasos), y 4 semánticos de estado (error, éxito, advertencia, info),
cada uno con su escala de luminosidad. En total puedes definir **50-80 valores
individuales, pero todos derivan de solo 5-7 hues base.** Eso son tus primitivos.

> **Truco de contraste:** el contraste viene de diferencias de LUMINOSIDAD, no de
> hues que compiten. Linear y Notion corren sistemas casi monocromáticos y se ven
> limpísimos justo por eso.

---

## 🛠️ Flujo recomendado (con las herramientas del archivo de recursos)
1. Tipo de paleta + hue de marca → **Adobe Color** (Explore por mood) o **Khroma**.
2. Refina y genera escalas → **Coolors** / **ColorKit** / **Open Color**.
3. Asigna roles 60-30-10 y escríbelos en la capa semántica de tokens.
4. Valida contraste → **WebAIM** / **Stark**.
5. Prueba en una página real → **Realtime Colors**.
6. Si quedó excelente → guárdalo como skin en `skins-catalog.md`.

---

## ⚠️ Errores comunes a evitar
- 60 y 30 demasiado parecidos → sin separación visual.
- Acento en demasiados elementos → deja de señalar acción.
- Hardcodear hex en componentes → un cambio de marca obliga a buscar por todos los
  archivos. **Siempre tokens** (CSS vars / Tailwind config).
- Asumir que 60-30-10 garantiza accesibilidad → la regla asigna proporción, NO
  contraste. Verifica WCAG aparte.
