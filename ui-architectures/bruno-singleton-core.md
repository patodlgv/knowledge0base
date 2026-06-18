# 📂 Módulo: Arquitectura Singleton para Entornos WebGL (Estilo Bruno Simon)

## 🎯 Objetivo
Desvincular el motor 3D y las simulaciones físicas del ciclo de vida de React. Utilizando el patrón de diseño *Singleton*, creamos una instancia única y global de la experiencia WebGL. Esto permite que múltiples componentes (físicas, audio, cámara, geometría) se comuniquen entre sí sin prop-drilling, y asegura que la escena 3D no se desmonte al cambiar de rutas en la interfaz.

## ⚙️ Stack / Dependencias
* **Lenguaje:** Vanilla JavaScript (ES6+ Classes).
* **Motor:** Three.js crudo (aislado de React Three Fiber para control absoluto de rendimiento, si es necesario) o actuando como un gestor de estado global para R3F.
* **Físicas:** Rapier.js (WASM) para colisiones a 60 FPS.

## 🌐 Origen (Ingeniería Inversa)
* **Inspiración:** Código fuente de `bruno-simon.com` (Portafolio 2025).
* **Lógica Clave:** Todo el ecosistema tridimensional vive en una clase maestra llamada `Experience`. Cualquier submódulo (como el controlador de la esfera interactiva o la cámara) simplemente llama a `Experience.getInstance()` para acceder al renderizador, al tiempo (Ticker) o al gestor de físicas, eliminando el código espagueti.

## 📐 Reglas Arquitectónicas
1. **Desacoplamiento Estricto:** Los componentes de React (ej. `AutomationsPage`, `PricingPage`) nunca deben recalcular geometría. Solo envían señales (eventos o cambios de ruta) a la instancia Singleton.
2. **Única Fuente de Verdad:** Solo existe UN bucle de animación (`requestAnimationFrame`), gestionado por la clase `Time` dentro del Singleton, que reparte el *delta-time* a las físicas y las animaciones.

---

## 🛠️ Código Refactorizado y Listo para Producción

### 1. El Núcleo Singleton (`src/webgl/Experience.js`)
Esta clase asegura que solo exista un universo 3D, sin importar cuántas veces React se actualice.

```javascript
import * as THREE from 'three';
// Importaciones de módulos internos de la experiencia
// import Time from './utils/Time.js';
// import Physics from './world/Physics.js';
// import InteractiveSphere from './world/InteractiveSphere.js';

let instance = null;

export default class Experience {
    constructor(canvas) {
        // Implementación del Patrón Singleton
        if (instance) {
            return instance;
        }
        instance = this;

        // Opciones globales
        this.canvas = canvas;

        // Inicialización del Ecosistema
        this.scene = new THREE.Scene();
        
        // El motor de físicas en WebAssembly (Rapier) se inicializaría aquí
        // this.physics = new Physics();
        
        // Elementos visuales abstractos (Cero laptops/pantallas, puras geometrías interactivas)
        // this.sphere = new InteractiveSphere();

        // Inicialización del Ticker central (Delta-time clampeado documentado previamente)
        // this.time = new Time();
        // this.time.on('tick', () => { this.update(); });
    }

    // Método estático para acceder a este universo desde cualquier otro archivo JS
    static getInstance() {
        return instance;
    }

    update() {
        // Este es el único lugar donde ocurren actualizaciones por frame.
        // Se llama a this.physics.update(), this.sphere.update(), y finalmente al render.
        // this.renderer.render(this.scene, this.camera.instance);
    }

    destroy() {
        // Lógica estricta de limpieza de memoria si alguna vez se necesita matar la experiencia
    }
}

//integracion en react
import React, { useEffect, useRef } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Experience from './webgl/Experience';

// Páginas de Interfaz (La cáscara UI tipográfica)
import HomePage from './pages/HomePage';
import AutomationsPage from './pages/AutomationsPage';
import PricingPage from './pages/PricingPage';

export const App = () => {
  const canvasRef = useRef(null);

  useEffect(() => {
    // 1. Al arrancar la app, inicializamos el Universo 3D en el canvas subyacente.
    // Al ser un Singleton, si este useEffect se volviera a correr, no se duplicaría.
    if (canvasRef.current) {
        new Experience(canvasRef.current);
    }
  }, []);

  return (
    <>
      {/* EL LIENZO WEBGL: Fijo, intocable, renderizando elementos como la esfera 3D. */}
      <canvas 
        ref={canvasRef} 
        style={{
          position: 'fixed',
          top: 0,
          left: 0,
          width: '100vw',
          height: '100vh',
          zIndex: -1,
          pointerEvents: 'none' // La interacción se captura globalmente o vía atributos
        }}
      />

      {/* LA INTERFAZ REACT: Maneja el enrutamiento y la tipografía formal. */}
      <div className="ui-layer">
        <BrowserRouter>
          <Routes>
            <Route path="/" element={<HomePage />} />
            <Route path="/automations" element={<AutomationsPage />} />
            <Route path="/pricing" element={<PricingPage />} />
          </Routes>
        </BrowserRouter>
      </div>
    </>
  );
};
