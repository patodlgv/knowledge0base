
# ✍️ Sistema de Tipografía — Escala, Jerarquía y Pairing

> Cómo construir el sistema tipográfico de cualquier sitio. Alimenta los tokens de
> `--font-*` y `--text-*` en `design-tokens-system.md`. La meta: jerarquía clara,
> legibilidad y ese acabado "caro" que separa a un senior del resto.

---

## 📐 La escala modular (adiós a tamaños arbitrarios)

En vez de inventar tamaños al azar, multiplicas un tamaño base por un ratio fijo,
repetidamente. Eso crea proporción armónica y jerarquía clara automáticamente.

- **Base:** 16px para cuerpo web (a veces 18px). En móvil, ligeramente más grande.
- **Ratio (elige según contraste deseado):**
  - **1.2 (Minor Third):** contraste sutil → interfaces con mucho texto, dashboards.
  - **1.25 (Major Third):** contraste moderado → **la opción versátil por defecto.**
  - **1.333 (Perfect Fourth):** contraste marcado → marketing, landings con punch.
  - **1.618 (Golden Ratio):** contraste alto → marcas de lujo, sitios premium,
    proyectos artísticos. *Cuidado: demasiado dramático para apps con mucho texto
    (headings gigantes que desperdician espacio).*

Ejemplo con base 16 y ratio 1.25: 16, 20, 25, 31, 39, 49px.

> **Herramientas:** type-scale.com, modularscale.com, **Utopia.fyi** (este genera
> escala FLUIDA responsiva — el más avanzado). Previsualizan con tu fuente real y
> exportan CSS.

---

## 🪜 Jerarquía (el orden de lectura)

La jerarquía guía el ojo por importancia: **H1** captura atención, **H2/H3**
organizan secciones, **cuerpo** entrega la sustancia, **caption/label** da detalle
de apoyo. Se logra con tamaño + peso + estilo + espaciado, no solo tamaño.

- **Los headings DEBEN reflejar estructura semántica** (un solo H1, luego H2, H3
  en orden) — para SEO y lectores de pantalla. No saltes niveles por estética.
- **Peso (weight)** establece jerarquía y énfasis: Regular para cuerpo, Medium/
  Semibold/Bold para títulos y énfasis. No abuses del bold o pierde fuerza.

---

## 📏 Legibilidad — los números que importan

- **Line-height (interlineado):** cuerpo **1.5-1.7**; títulos más apretado **1.3**.
- **Longitud de línea:** **60-80 caracteres** por línea (óptimo de lectura). Usa
  `max-width: 65ch` en contenedores de texto.
- **Alineación:** cuerpo SIEMPRE alineado a la izquierda (mejor legibilidad y
  accesibilidad). Evita justificado (riesgoso).
- **Unidades relativas (rem/em)** para tamaños de fuente — respeta la preferencia
  de tamaño del navegador del usuario. Crucial para accesibilidad.
- **Contraste de texto:** mínimo 4.5:1 (igual que color).
- **Dato:** buena tipografía mejora precisión de lectura hasta 20% y reduce fatiga
  visual ~30%. No es estética, es función.

---

## 📱 Tipografía fluida (responsive sin saltos)

`clamp()` es perfecto para tamaños que escalan suave entre móvil y desktop:
```css
h1 { font-size: clamp(2rem, 5vw, 4rem); }  /* min, preferido(viewport), max */
```
Toma mínimo, preferido (basado en viewport) y máximo. El título crece suave de
2rem a 4rem según el ancho de pantalla, sin breakpoints bruscos.

---

## 🤝 Pairing (emparejar fuentes)

> **La regla de oro:** el pairing NO es "dos fuentes que se ven bien juntas". Es
> crear JERARQUÍA CLARA — la fuente de título carga PERSONALIDAD, la de cuerpo
> carga LEGIBILIDAD.

- **2-3 familias máximo.** Típico: una display para títulos + una sans neutra para
  cuerpo. (Ej. sans geométrica para títulos + sans humanista para cuerpo; o serif
  con carácter + sans limpia.)
- **El arma secreta:** emparejar una fuente de cuerpo gratis y ubicua (Inter, DM
  Sans, Manrope) con una de título DISTINTIVA y con carácter. Eso hace que un
  producto se vea de "Serie A" sin presupuesto de Serie A.
- **`font-display: swap`** para que el texto se muestre de inmediato con fuente de
  respaldo (performance + evita FOIT).
- **Combos probados 2026:** Bricolage Grotesque + Inter (trending); Playfair
  Display (títulos grandes) + Inter; ver `color-typography-resources.md` para más.

---

## 🇲🇽 No negociable para México
**Verifica soporte de tildes (á é í ó ú) y ñ** antes de usar cualquier fuente. En
Google Fonts, previsualiza con "Diseño Español Niño Acción". Una fuente que rompe
la ñ es inservible para clientes mexicanos.

---

## ⚠️ Errores comunes a evitar
- Demasiados tamaños distintos → ruido visual. Usa la escala, no valores sueltos.
- Poco contraste de tamaño entre niveles → jerarquía confusa.
- Ratio demasiado dramático (1.618) en apps con mucho texto → títulos enormes que
  desperdician espacio.
- No probar la escala con contenido real en varios dispositivos.
- Fuente de sistema genérica (Times New Roman, Arial) en branding → se lee a
  "documento de oficina". Usa display de diseño (ver recursos).

---

## 🔁 Flujo recomendado
1. Define personalidad de marca → elige display con carácter (Typewolf/Fontpair).
2. Empareja con sans legible para cuerpo (Inter/DM Sans/Manrope).
3. Verifica ñ/tildes y `font-display: swap`.
4. Genera escala modular (Utopia/type-scale) → escribe tokens `--text-*`.
5. Define line-height, max-width 65ch, alineación izquierda.
6. Prueba con contenido real, móvil y desktop. Combo bueno → guárdalo en skin.
