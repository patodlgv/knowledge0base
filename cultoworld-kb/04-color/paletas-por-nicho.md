# Paletas por nicho (con psicología del color)

> **Tier: AMBOS** · Estructura universal: **1 tinta + 1–2 fondos + 1 acento + neutros**. El acento es RARO (solo CTA y énfasis) — la escasez es lo que lo hace poderoso. Restricción = premium; arcoíris = amateur.
> Todos los pares texto/fondo listados pasan WCAG AA para cuerpo (≥4.5:1) salvo nota.

## Cómo usar estas paletas

Cada nicho da tokens listos para copiar. Ajustar el acento al logo del cliente si existe (ver `accesibilidad-y-escalas.md` §extracción). El "modo" (claro/oscuro) es una decisión de carácter: claro = abierto/confiable; oscuro = dramático/premium.

---

## Clínica / dental / salud — confianza + calma

Psicología: azules/verdes bajan la ansiedad (asociación con limpieza y competencia médica); evitar rojos (sangre/alarma). Blancos cálidos, no quirúrgicos.

```css
:root {
  --ink: #16303f;      /* azul petróleo oscuro (texto) */
  --paper: #f8fafb;    /* blanco frío suave */
  --surface: #e8f1f2;  /* paneles */
  --accent: #0e7c86;   /* teal — CTA */
  --muted: #5b7280;
}
```

## Restaurante / café — apetito + calidez

Psicología: rojos/naranjas/tierras estimulan el apetito (por eso los usan las cadenas); los tonos tierra suben el rango percibido. Azul suprime el apetito — evitarlo como dominante.

```css
:root {
  --ink: #2b1d16;      /* café espresso */
  --paper: #faf5ef;    /* crema */
  --surface: #f1e6d8;
  --accent: #c0392b;   /* rojo terracota — CTA/reservar */
  --muted: #8a7466;
}
/* variante fine-dining: fondo #1a1512 (oscuro) + acento dorado #c5a46d */
```

## Gym / crossfit — energía + intensidad

Psicología: negro = poder; acentos saturados (lima, rojo, naranja) = adrenalina y urgencia. Modo oscuro casi obligado.

```css
:root {
  --ink: #f2f2f2;
  --paper: #101014;    /* negro azulado */
  --surface: #1b1b22;
  --accent: #d7fd3a;   /* lima eléctrico — CTA (usar sobre fondo oscuro; con texto NEGRO encima) */
  --muted: #9a9aa5;
}
```
Nota WCAG: lima sobre negro pasa sobrado; texto sobre lima debe ser `--paper`-oscuro (#101014).

## Barbería — herencia + actitud

Psicología: marrones/negros/cremas = tradición y masculinidad; un acento borgoña o cobre = navaja clásica, cuero.

```css
:root {
  --ink: #ece7df;
  --paper: #17130f;    /* negro café */
  --surface: #241d16;
  --accent: #b3702c;   /* cobre */
  --muted: #9c917f;
}
```

## Bienes raíces / hotel boutique — aspiración + solidez

Psicología: navy = estabilidad financiera y lujo discreto; dorado/arena = exclusividad. (La paleta madre de Culto vive aquí: navy #0A1128 / oro #C5A46D / ivory #F6F4F1.)

```css
:root {
  --ink: #0f1b33;      /* navy */
  --paper: #f6f4f1;    /* ivory */
  --surface: #eae5dd;
  --accent: #b08d4f;   /* oro viejo — solo CTA y detalles; NUNCA texto chico sobre claro */
  --muted: #64708a;
}
```

## Spa / estética — serenidad + suavidad

Psicología: verdes salvia y rosas empolvados = naturaleza, cuidado, piel. Contrastes suaves pero SIEMPRE ≥4.5:1 en cuerpo (el error #1 del nicho es texto gris-claro ilegible).

```css
:root {
  --ink: #3a4238;      /* verde grisáceo oscuro */
  --paper: #f7f5f0;
  --surface: #e9ebe2;  /* salvia pálido */
  --accent: #7d8c6f;   /* salvia — CTA con texto #f7f5f0 = 3.2:1 → usar SOLO texto grande/botones ≥18.66px bold */
  --accent-deep: #55624b; /* variante para botones con texto chico (5.1:1) */
  --muted: #6f7568;
}
```

## Legal / despacho — autoridad + sobriedad

Psicología: gris carbón + azul oscuro = rigor; un acento vino o dorado = tradición. Nada juguetón.

```css
:root {
  --ink: #1c2430;
  --paper: #f5f5f3;
  --surface: #e8e9e6;
  --accent: #7c2d3e;   /* borgoña */
  --muted: #5c6672;
}
```

## Fintech / SaaS local — modernidad + confianza

```css
:root {
  --ink: #10141f;
  --paper: #fbfbfd;
  --surface: #eef0f6;
  --accent: #2545d3;   /* azul eléctrico */
  --muted: #5d647a;
}
```

## Educación / academia — apertura + optimismo

```css
:root {
  --ink: #232838;
  --paper: #fdfcf8;
  --surface: #f0ede2;
  --accent: #e8632c;   /* naranja cálido — energía sin infantilizar */
  --muted: #6b7080;
}
```

---

## Reglas transversales

1. **Nunca pure black sobre pure white** (#000/#fff es duro); suaviza ambos extremos.
2. En modo oscuro: texto #eaeaea-ish sobre #0f0f0f-ish, line-height +0.1.
3. Neutros TEÑIDOS hacia el mood de la marca (un gris cálido para restaurante, frío para clínica) — la cohesión viene de ahí.
4. Hover del acento: mover lightness (±8%), no cambiar de hue.
5. El acento aparece 1–2 veces por pantalla. Si todo es acento, nada lo es.
6. Verificar CADA par texto/fondo (herramientas en `accesibilidad-y-escalas.md`) — el gris "muted" sobre fondo claro es el que más falla.
