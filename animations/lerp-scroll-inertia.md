
# 📂 Módulo: Interpolación Lineal (Lerp) e Inercia Física con Ticker Unificado

## 🎯 Objetivo
Eliminar los movimientos bruscos y rígidos del navegador. Este módulo implementa un motor de cálculo físico global que suaviza la persecución de coordenadas (cursores, rotaciones 3D, scroll) utilizando interpolación lineal (`lerp`) combinada con un bucle de animación unificado (`requestAnimationFrame`) que corrige la velocidad según los hercios de la pantalla (Delta-Time Clamping).

## ⚙️ Stack / Dependencias
* **Lenguaje:** JavaScript Moderno (ES6+) / TypeScript
* **Framework:** React 19 (Optimizado para Hooks de ciclo de vida limpio)
* **Compatibilidad:** Integración nativa con Canvas WebGL (Three.js) y Timelines de GSAP.

## 🌐 Origen (Ingeniería Inversa)
* **Inspiración:** `unseen.co` (Estructura de estado global compartido y normalización de coordenadas de mouse).
* **Arquitectura:** `bruno-simon.com` (Patrón Ticker de bucle único con limitación estricta de retraso de fotogramas para robustez en producción).

## 📐 Lógica Clave
1.  **Interpolación Lineal Math (`lerp`):** Calcula una posición intermedia entre el punto actual y el destino final aplicando un factor de fricción (`lerp(start, end, amount)`). Esto genera la deceleración orgánica ("peso").
2.  **Delta-Time Clamping:** Si el navegador experimenta una caída severa de frames o se congela momentáneamente, multiplicar ciegamente por el tiempo transcurrido haría que los objetos "teletransporten" o salten distancias enormes de golpe. Limitamos el delta a un tope máximo (equivalente a 30 FPS) para mantener la estabilidad física del entorno interactivo.
3.  **Single Ticker:** Centraliza el bucle visual para evitar múltiples hilos concurrentes que degraden el rendimiento de la CPU.

---

## 🛠️ Código Refactorizado y Listo para Producción

Guarda este código como un componente de infraestructura o utilidad avanzada dentro de tu ecosistema.

```javascript
import { useEffect, useRef } from 'react';

/**
 * Utilidad matemática pura para cálculo de interpolación lineal.
 * @param {number} start - Posición actual del elemento en el espacio.
 * @param {number} end - Coordenada objetivo (ej. posición real del mouse/scroll).
 * @param {number} amount - Factor de suavizado (Fricción entre 0 y 1).
 */
export const lerp = (start, end, amount) => start + (end - start) * amount;

/**
 * Hook Avanzado de Inercia Global (Mouse, Scroll y Dimensiones Precalculadas)
 * Diseñado bajo la arquitectura de estado único compartido.
 */
export const useInertiaTicker = (callback) => {
  const stateRef = useRef({
    // Coordenadas reales (Input del dispositivo)
    target: { x: 0, y: 0, scroll: 0 },
    // Coordenadas suavizadas (Las que persiguen con inercia física)
    current: { x: 0, y: 0, scroll: 0 },
    // Métricas del dispositivo precalculadas una sola vez
    window: { width: 0, height: 0, pixelRatio: 1 },
    // Parámetros de tiempo del Ticker
    ticker: {
      lastTime: performance.now(),
      maxDelta: 1 / 30, // Límite estricto para evitar saltos (Tope a 30fps en lags)
    }
  });

  useEffect(() => {
    const state = stateRef.current;

    // 1. Inicializar y precalcular dimensiones (Evita lecturas continuas del DOM en el loop)
    const handleResize = () => {
      state.window.width = window.innerWidth;
      state.window.height = window.innerHeight;
      state.window.pixelRatio = Math.min(window.devicePixelRatio, 2); // Limitar a 2 por rendimiento gráfico
    };

    // 2. Captura de eventos nativos de manera ultra-ligera
    const handleMouseMove = (e) => {
      // Normalización de coordenadas (-1 a 1 para compatibilidad inmediata con Shaders/WebGL)
      state.target.x = (e.clientX / state.window.width) * 2 - 1;
      state.target.y = -(e.clientY / state.window.height) * 2 + 1;
    };

    const handleScroll = () => {
      state.target.scroll = window.scrollY;
    };

    // Escuchas globales iniciales
    handleResize();
    window.addEventListener('resize', handleResize, { passive: true });
    window.addEventListener('mousemove', handleMouseMove, { passive: true });
    window.addEventListener('scroll', handleScroll, { passive: true });

    // 3. El Ticker Principal (Single RAF Loop)
    let animationFrameId;

    const tick = (currentTime) => {
      // Cálculo del delta-time real convertido a segundos
      const elapsedSeconds = (currentTime - state.ticker.lastTime) / 1000;
      state.ticker.lastTime = currentTime;

      // Aplicación de Clamping para blindar el rendimiento ante congelamientos
      const delta = Math.min(elapsedSeconds, state.ticker.maxDelta);

      // Factor de velocidad adaptable basado en los hercios reales del monitor
      const interpolationFactor = 1 - Math.pow(1 - 0.1, delta * 60);

      // Ejecutar la interpolación lineal para cada eje interactivo
      state.current.x = lerp(state.current.x, state.target.x, interpolationFactor);
      state.current.y = lerp(state.current.y, state.target.y, interpolationFactor);
      state.current.scroll = lerp(state.current.scroll, state.target.scroll, interpolationFactor);

      // Enviar el estado físico actualizado al callback de renderizado (UI o WebGL Canvas)
      if (callback) {
        callback(state.current, state.target, delta);
      }

      animationFrameId = requestAnimationFrame(tick);
    };

    animationFrameId = requestAnimationFrame(tick);

    // Limpieza estricta de memoria al desmontar componentes en React 19
    return () => {
      window.removeEventListener('resize', handleResize);
      window.removeEventListener('mousemove', handleMouseMove);
      window.removeEventListener('scroll', handleScroll);
      cancelAnimationFrame(animationFrameId);
    };
  }, [callback]);

  return stateRef;
};
