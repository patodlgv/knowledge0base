
# 📂 Módulo: Esqueleto Tipográfico Minimalista (CSS Flaco)

## 🎯 Objetivo
Proveer una base de estilos global (CSS) que actúe únicamente como una "cáscara" transparente para la maquetación. Este módulo elimina el comportamiento predeterminado del navegador, evita el uso de grids complejos que saturen el DOM y garantiza que la interfaz (UI) nunca bloquee la interacción con el lienzo WebGL (Canvas) que vive debajo.

## ⚙️ Stack / Dependencias
* **Lenguaje:** CSS3 Puro / Módulos CSS (Compatible con LESS/SASS si se requiere alcance local).
* **Framework:** React 19 (Importado en el punto de entrada global, ej. `index.css` o `App.css`).

## 🌐 Origen (Ingeniería Inversa)
* **Inspiración:** `unseen.co` (Filosofía de "CSS de 72K", maquetación mínima, protagonismo del canvas).
* **Lógica Clave:** Todo elemento del DOM que no sea estrictamente interactivo (como el texto decorativo o los títulos) debe tener `pointer-events: none`. Esto permite que el cursor del usuario "atraviese" el HTML y pueda interactuar con la geometría 3D subyacente.

## 📐 Reglas Arquitectónicas
1. **Reseteo Agresivo:** Eliminar márgenes, paddings y comportamientos de scroll nativos que interfieran con la inercia calculada por JavaScript.
2. **Tipografía Base:** Establecer un sistema de fuentes súper limpio, preparando el terreno para los acentos formales en *Times New Roman* cursiva que se inyectarán vía componentes.
3. **Cero Fondos Sólidos:** El `body` y el `#root` de React deben ser transparentes para revelar la escena WebGL de fondo.

---

## 🛠️ Código Refactorizado y Listo para Producción

Copia este código en tu archivo de estilos globales (`global.css` o `main.css`).

```css
/* ==========================================================================
   RESET GLOBAL Y PREPARACIÓN DEL LIENZO
   ========================================================================== */
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html,
body,
#root {
  width: 100vw;
  height: 100vh;
  /* Evita el scroll nativo del navegador, todo se controla por JS/GSAP */
  overflow: hidden; 
  /* Transparencia total para que el WebGL Canvas sea visible */
  background-color: transparent; 
  /* Tipografía secundaria limpia para UI de soporte (botones, menús pequeños) */
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ==========================================================================
   GESTIÓN DE CAPAS (Z-INDEX) Y EVENTOS DEL PUNTERO
   ========================================================================== */

/* Contenedor principal de la Interfaz de Usuario.
  Flota sobre el Canvas 3D.
*/
.ui-layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 10;
  /* IMPORTANTE: Permite que el mouse "atraviese" la UI transparente */
  pointer-events: none; 
}

/* Clase utilitaria para elementos que SÍ necesitan recibir clics
  (Enlaces, botones del menú, controles).
*/
.interactive-element {
  pointer-events: auto;
  cursor: pointer;
}

/* ==========================================================================
   UTILIDADES ESTÉTICAS (Alta Costura)
   ========================================================================== */

/* Clase para aplicar la dirección de arte formal rápidamente */
.typography-serif-accent {
  font-family: 'Times New Roman', Times, serif;
  font-style: italic;
  font-weight: 400;
  line-height: 1.1;
  letter-spacing: -0.02em;
}

/* Ocultar elementos visualmente pero mantenerlos para accesibilidad/SEO */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
