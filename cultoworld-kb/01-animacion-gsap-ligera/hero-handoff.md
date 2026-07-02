# Hero Handoff — el apartado para el hero hecho a mano

> **Tier: AMBOS** · Protocolo para los proyectos de mejor calidad: **la IA/fábrica construye TODO el sitio menos el hero**, y deja el slot + una propuesta de storytelling lista. Pato entra, ejecuta solo el hero, y el sitio queda completo. Cero fricción de integración.

## Cómo funciona (instrucción para Claude Code)

Cuando el proyecto esté marcado como `hero: handoff` en el planning, construye el sitio ENTERO (secciones, conversión, SEO, animaciones de todo lo demás) y en lugar del hero:

1. Deja el componente `HeroSlot` (contrato abajo) con un placeholder digno (no un hueco gris — el sitio debe ser demostrable sin el hero final).
2. Genera el archivo `HERO-BRIEF.md` en la raíz del proyecto con la propuesta de storytelling (plantilla abajo) YA LLENA con el contexto del cliente.
3. No inventes el hero final. El handoff ES el entregable.

## El contrato del HeroSlot (lo que el sitio le garantiza al hero)

```tsx
// components/HeroSlot.tsx — el hero vive aquí y SOLO aquí
// GARANTÍAS del resto del sitio hacia este componente:
// 1. Tokens disponibles: var(--color-bg/surface/brand/text), var(--font-display/body),
//    escala --fs-*, espaciado --s-* (de la skin del proyecto)
// 2. Contenedor: full-bleed, min-height 100svh, sin padding impuesto
// 3. El sitio NO anima nada dentro de este slot; expone un callback
//    `onHeroReady()` para encadenar la entrada del resto (nav, primera sección)
// 4. El <h1> SEO vive AQUÍ (el sitio no tiene otro h1) — si el hero es canvas,
//    el h1 real va en DOM sobre/junto al canvas
// 5. Presupuesto de peso del hero según tier (ligero: sin WebGL, <150KB assets;
//    premium: lazy-load + fallback obligatorios, ver 03-threejs-webgl/)

"use client";
export default function HeroSlot({ onHeroReady }: { onHeroReady?: () => void }) {
  // ⬇️ PLACEHOLDER — reemplazar con el hero hecho a mano (ver HERO-BRIEF.md)
  return (
    <section className="hero-slot" aria-labelledby="hero-title">
      <h1 id="hero-title">{/* headline del brief */}</h1>
      {/* placeholder digno: skin aplicada + reveal básico de líneas */}
    </section>
  );
}
```

Reglas de integración: el hero puede importar GSAP/Three libremente; si registra ScrollTriggers debe hacerlo dentro de `useGSAP` (cleanup); llamar `onHeroReady()` cuando su entrada termine para que la nav/badge entren después (coherencia de coreografía).

## Plantilla HERO-BRIEF.md (la propuesta de storytelling)

```markdown
# HERO BRIEF — [Cliente]

## El trabajo del hero (1 línea)
En 5 segundos, el visitante debe sentir: ______ y entender: ______.

## Through-line del sitio (ya implementada abajo del hero)
La página cuenta: [tensión] → [resolución]. El hero abre la tensión / hace la promesa.

## Propuesta de storytelling (elegir o remezclar)
### Opción A — [nombre evocador]
- Concepto: ______ (referencia extra-web: película/disco/arquitectura/textura)
- Momento clave: qué pasa en los primeros 1.5s (la entrada)
- Elemento vivo persistente: ______
- Composición: [patrón A-F de heros-ligeros.md como base o ruptura deliberada]
- Técnica sugerida: [GSAP puro / 3D falso / partículas / shader] + costo de perf
### Opción B — [contraste con A: si A es calma, B es impacto]
(mismos campos)

## Materia prima lista
- Headline aprobado: "______" (≤8 palabras, ya validado con copywriting-mx.md)
- Sub y CTA: "______" / "______" (el CTA ya está cableado a WhatsApp/form)
- Assets disponibles: [fotos reales / logo vector / paleta skin "______"]
- Referencias visuales: [3 links máx — sitios/frames que capturan el mood]

## Restricciones no negociables de este proyecto
- Tier: ______ → presupuesto: [sin WebGL, LCP<2s / WebGL con fallback]
- El h1 queda en DOM · reduced-motion con estado final digno
- Mood de la skin: no introducir colores fuera de tokens

## Definición de terminado del hero
[ ] Entrada coreografiada <1.5s  [ ] Elemento vivo  [ ] onHeroReady() llamado
[ ] LCP intacto (probar con throttling)  [ ] Se siente [emoción del brief]
```

## Por qué este sistema es ventaja

- El 20% del sitio que decide la primera impresión (Stanford: 46% juzga credibilidad por diseño visual; juicio estético en ~50ms) recibe el 100% del toque humano.
- La fábrica no se detiene: el sitio se entrega demostrable con placeholder y sube de nivel cuando el hero artesanal entra.
- El brief de storytelling elimina la página en blanco: llegas a ejecutar, no a decidir desde cero.

## Cuándo SÍ / Cuándo NO

- ✅ Proyectos premium, sitio insignia, demos de nichos nuevos que quieres que vendan solos.
- ✅ Cuando el cliente pagó por "distinto a todo".
- ❌ NO en la fábrica de volumen (ahí los patrones A-F de `heros-ligeros.md` ya resuelven).
