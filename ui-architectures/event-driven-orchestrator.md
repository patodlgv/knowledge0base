# 📂 Módulo: Orquestador de Eventos y Comunicación Desacoplada (Hooks)

## 🎯 Objetivo
Evitar que la capa de Interfaz (React 19) y el motor gráfico (WebGL / Singleton) estén rígidamente acoplados. Este módulo establece un sistema de comunicación basado en eventos (Patrón *Pub/Sub* o *Hooks*), permitiendo que la interfaz dispare acciones (ej. "usuario hizo clic en Cotizar") y el lienzo 3D reaccione a ellas sin necesidad de pasarse propiedades (*props*) complejas o forzar re-renderizados pesados.

## ⚙️ Stack / Dependencias
* **Lenguaje:** Vanilla JavaScript (Sistema de Eventos / `@wordpress/hooks` o Custom PubSub).
* **Motor de Animación:** GSAP (Registro global de curvas de interpolación).

## 🌐 Origen (Ingeniería Inversa)
* **Inspiración:** Orquestador principal (`js-e3`) y sistema de eventos (`hook`) de `unseen.co`.
* **Lógica Clave:** En la inicialización del sitio, bloquean la restauración de scroll nativa del navegador. Luego, instancian un bus de eventos global. Si la UI necesita que la cámara 3D se mueva, hace un `doAction('move_camera')`. El ecosistema 3D, que está escuchando con `addAction('move_camera')`, ejecuta el movimiento. Cero acoplamiento, máximo rendimiento.

## 📐 Reglas Arquitectónicas Estrictas
1. **Secuestro de Scroll:** El navegador no debe intentar recordar dónde estaba el scroll al recargar la página. Se debe forzar al inicio para que el motor de físicas (`lerp`) tome el control limpio.
2. **Registro Centralizado de GSAP:** Las curvas de animación (`CustomEase`) deben registrarse una sola vez en el punto de entrada de la aplicación, no dentro de componentes individuales.
3. **Comunicación por Eventos:** React nunca debe manipular el DOM del Canvas directamente. Todo comando visual complejo debe pasar por el bus de eventos.

---

## 🛠️ Código Refactorizado: El Sistema Nervioso Central

### 1. El Bus de Eventos Global (`src/utils/EventBus.js`)
La IA debe utilizar esta estructura ligera para emitir y escuchar eventos en toda la app.

```javascript
// Implementación minimalista del patrón extraído de unseen (addAction/doAction)
class EventBusCore {
  constructor() {
    this.events = {};
  }

  // Equivalente a addAction
  on(event, callback) {
    if (!this.events[event]) this.events[event] = [];
    this.events[event].push(callback);
  }

  // Equivalente a removeAction
  off(event, callback) {
    if (!this.events[event]) return;
    this.events[event] = this.events[event].filter(cb => cb !== callback);
  }

  // Equivalente a doAction
  emit(event, data) {
    if (!this.events[event]) return;
    this.events[event].forEach(callback => callback(data));
  }
}

export const EventBus = new EventBusCore();

//Inicializacion del Orquestador

import gsap from 'gsap';
import CustomEase from 'gsap/CustomEase';
import { EventBus } from './utils/EventBus';

// 1. Registro de Curvas Globales (Extraído de js-e3)
gsap.registerPlugin(CustomEase);
CustomEase.create("premiumOut", "0.32, 0.02, 0, 1");

// 2. Control de hardware y restauración nativa
if ('scrollRestoration' in window.history) {
  window.history.scrollRestoration = 'manual';
}

// 3. Ejemplo de puente de comunicación UI -> WebGL
export const triggerPremiumTransition = (targetSection) => {
  // La interfaz dispara esto. El WebGL lo escucha en su propio archivo.
  EventBus.emit('transition_start', { section: targetSection });
};
