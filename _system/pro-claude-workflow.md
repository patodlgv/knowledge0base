# 🛠️ Cómo Programan los Profesionales con Claude (Flujo + Stack de Élite)

> El método de los que sacan sitios de máxima calidad con Claude Code en 2026.
> No es prompt mágico — es ESTRUCTURA alrededor del modelo. Esto amplifica todo
> lo demás del vault (tokens, cimientos, skins). Léelo antes de cada build serio.

---

## 🎯 El principio que lo gobierna todo

Dato de pruebas internas de Anthropic: si Claude acierta 80% por decisión y un
feature tiene ~20 decisiones, todo sale bien de un tiro ~1% de las veces (0.8²⁰).
Los intentos sin guía tienen ~33% de éxito. **La diferencia entre los ingenieros
de alto rendimiento y el resto NO es la calidad del prompt — es la estructura que
construyen alrededor de Claude antes de ejecutar.** Planear colapsa esas 20
decisiones ambiguas en un spec revisado donde cada una queda cerca del 100%.

En 2026 el rol del dev pasó de "manejar contexto" a "especificar el resultado":
decidir qué construir, revisar el enfoque propuesto, y validar el resultado.

---

## 📂 El ecosistema de archivos de Claude Code (la estructura real)

### 1. `CLAUDE.md` — el cerebro persistente del proyecto
Va en la raíz. Claude Code lo lee automáticamente en cada arranque. Reemplaza
tener que pegar contexto manualmente. Debe contener:
- Convenciones del proyecto (stack, naming, estructura de carpetas).
- Referencia al knowledge base (los tokens, cimientos, foundations).
- La política de `/compact` (para compresión consistente de contexto).
- Para Culto: que es tier B, paleta, fuentes, y que consulte `design-foundations/`.

### 2. `.claude/skills/<nombre>/SKILL.md` — habilidades auto-activables
NO es un solo .md — es una carpeta con estructura:
```
.claude/skills/api-conventions/
  SKILL.md      # definición principal (requerido)
  examples.md   # ejemplos de salida esperada (opcional)
  reference.md  # guía detallada (opcional)
```
El frontmatter YAML controla cuándo se auto-aplica. Una skill de "revisión de
seguridad" se activa sola cuando mencionas revisar vulnerabilidades — no tienes
que invocarla. Las skills personales en `~/.claude/skills/` sirven en TODOS tus
proyectos.

### 3. `.claude/agents/` — subagentes especialistas
Personas con su propio contexto, herramientas y modelo. (Ver abajo.)

### 4. Hooks — control determinista
Auto-formato al guardar, bloquear comandos peligrosos, correr tests antes de
commit. Control que no depende de que Claude "se acuerde".

---

## 🤖 Subagentes — la técnica que más sube la calidad

**El problema que resuelven:** en un refactor largo, el contexto se llena de
"side quests" (investigar esto, revisar aquello) hasta que el contexto revienta
al 70%+ y se auto-comprime, y pierdes el hilo. **La solución:** delegas la tarea
secundaria a una instancia hija con su PROPIO contexto, herramientas y modelo. La
sesión padre se queda enfocada en el trabajo que paga las cuentas.

- Cada subagente corre en su ventana de contexto aislada, hace su exploración, y
  regresa un RESUMEN comprimido. Tu sesión principal queda limpia.
- Estructuralmente previene la contaminación (acumulación de enfoques fallidos).
- **Usos típicos:** Research (explorar el código sin ensuciar el contexto
  principal), Verification (validar implementación con un agente aparte),
  Code Review (un agente que solo lee y critica).
- **Sweet spot: 3-5 subagentes en paralelo.** Más de 10 rara vez ayuda y dispara
  costos. Mantén el anidamiento somero (2-3 niveles máx).
- **Regla:** los subagentes ayudan cuando importa la separación de contexto y los
  límites de permisos. Para bugs simples, una sola sesión es más rápida.

Ejemplo de definición (`.claude/agents/code-reviewer.md`):
```
---
name: code-reviewer
description: Reviews code for quality. Use proactively after code changes.
tools: Read, Glob, Grep
model: sonnet
---
You are a senior code reviewer. Focus on quality, security, best practices.
```

---

## 📝 El flujo de trabajo profesional (paso a paso)

1. **Plan mode primero.** Para trabajo sustancial, usa `/plan` — crea una fase de
   diseño dedicada ANTES de implementar. Pide el plan SIN código.
2. **Revisa y anota.** Abre el plan, marca dónde se equivocó. Devuélvelo con:
   *"atiende todas las notas, no implementes aún"*. La frase guardián importa: sin
   ella, Claude empieza a escribir código de inmediato.
3. **Repite** hasta que cada decisión esté resuelta. (Un dev invirtió 2 horas en un
   spec de 12 pasos y recuperó 6-10 horas de implementación.)
4. **Solo entonces, ejecuta.**
5. **Valida en vivo:** abre localhost, camina cada página — fidelidad, responsive
   (360px→1920px), motion (¿las animaciones disparan en scroll/hover?).
6. **Itera puntual** sobre lo que falle, con instrucciones específicas (como
   hicimos con la luz/agua de Culto). División de trabajo limpia.
7. **Deja que el contexto se acumule** en sesiones más largas — en 2026 la
   compactación maneja los límites sola. No limpies contexto manualmente.

---

## 🏗️ El Stack de Élite (lo que usan las agencias premiadas en 2026)

### Base (no negociable para "máxima calidad")
- **TypeScript por defecto** — estándar absoluto de la industria. >70% de adopción
  en equipos frontend pro. Atrapa errores en compile-time, no en el navegador del
  usuario. *Red flag de agencia: entregar JavaScript puro sin tipos.*
- **Next.js App Router** (estable en 2026) — SSR/SSG/RSC, layouts anidados,
  streaming. Si un template aún usa Pages Router, está desactualizado.
- **Tailwind CSS** — utilidades + design tokens en config.
- **Vercel** — deploy, analytics, Core Web Vitals integrados.

### Capa visual / motion
- **GSAP + ScrollTrigger** para animación y scroll.
- **Framer Motion** para micro-interacciones React.
- **Three.js / React Three Fiber** solo en tier B (WebGL).
- **React Bits / shadcn/ui** para componentes accesibles rápidos (tier A/C).

### Contenido y datos
- **Headless CMS** (Sanity, Storyblok, Payload, Contentful) — para que el cliente
  edite contenido sin tocar código. Clave en multi-página.

### Calidad (lo que separa pro de amateur)
- **Tests automatizados** — baseline, no extra premium. *Red flag: handover sin
  testing.*
- **Documentación clara** — el código lo mantendrá alguien más después.
- **`@next/bundle-analyzer`** — auditar tamaño de bundle.
- **Lighthouse + Puppeteer en CI** — tests de performance automáticos que atrapan
  regresiones (sobre todo INP) antes de producción.
- **RUM (Real User Monitoring)** + Vercel Analytics — Core Web Vitals de usuarios
  reales, no solo del mockup perfecto.

---

## ✅ Qué revisan los profesionales ANTES de entregar (checklist)

- [ ] TypeScript sin errores, sin `any` regados.
- [ ] Responsive real probado 360px → 1920px.
- [ ] Core Web Vitals: LCP <2.5s, INP <200ms, CLS <0.1 (percentil 75, datos reales).
- [ ] Accesibilidad: semántico, teclado, contraste WCAG AA, lectores de pantalla.
- [ ] SEO: contenido en DOM (no solo en canvas), meta tags, sitemap, headings
      semánticos en orden.
- [ ] Animaciones disparan bien en scroll/hover; respeta `prefers-reduced-motion`.
- [ ] Bundle auditado, scripts de terceros minimizados.
- [ ] Tildes/ñ correctas en todo el contenido.
- [ ] Formularios funcionan y validan; CTAs llevan a donde deben.
- [ ] Deploy a Vercel + analytics conectado.
- [ ] Plan de soporte post-lanzamiento (los primeros 90 días sacan redirects rotos,
      edge cases de contenido, regresiones de performance).

---

## 💡 Lo que más debemos tener en cuenta (síntesis)

1. **Estructura > prompt.** El vault, el CLAUDE.md y el plan-primero son la ventaja.
2. **Contexto es un recurso.** Subagentes para no contaminar la sesión principal.
3. **Calidad invisible vende.** TypeScript, tests, accesibilidad y performance no se
   ven pero son lo que separa "se ve bonito" de "es de los mejores".
4. **El dev decide y valida; Claude ejecuta.** Mientras más entiendas el stack, más
   te amplifica (puedes cachar cuando el código está sutilmente mal).
5. **Soporte post-lanzamiento** = modelo de retainer (encaja con los planes de Culto).
