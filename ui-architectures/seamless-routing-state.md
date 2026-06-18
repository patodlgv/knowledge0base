# 📂 Módulo: Enrutamiento Sin Recarga e Inyección Dinámica de Datos

## 🎯 Objetivo
Mantener el contexto de WebGL vivo y a 60 FPS durante la navegación del usuario. En lugar de ejecutar peticiones tradicionales que recargan el navegador y destruyen el canvas, este módulo utiliza un estado global y *fetching* dinámico para inyectar datos (JSON) directamente a la capa de UI transparente, orquestando transiciones suaves con GSAP.

## ⚙️ Stack / Dependencias
* **Framework:** React 19 (Uso avanzado de `Suspense`, `use` hook o Context API para caché).
* **Enrutamiento:** React Router (configurado para no desmontar el layout base) o Wouter.
* **Animación:** GSAP (Para salidas y entradas del texto).

## 🌐 Origen (Ingeniería Inversa)
* **Inspiración:** Lógica de `contentscript.js` y el endpoint `projects` de `unseen.co`.
* **Lógica Clave:** Separar estrictamente el "Escenario" (Canvas WebGL) de la "Información" (DOM). Cuando se solicita un nuevo proyecto, GSAP oculta la tipografía actual, el motor hace un *fetch* silencioso del nuevo bloque de datos, y GSAP revela la nueva información con la misma secuencia estricta letra por letra.

## 📐 Reglas Arquitectónicas
1. **El Canvas es Inmortal:** El componente `<WebGLScene />` jamás debe desmontarse al cambiar de ruta. Debe residir fuera del `<Routes>` o bloque de cambio de vistas.
2. **Precarga (Prefetching):** Al pasar el cursor sobre un enlace (hover), se debe disparar silenciosamente la carga del archivo de datos (`projects.json`) antes de que el usuario haga clic.
3. **Sincronización de Salida/Entrada:** La nueva interfaz no debe aparecer hasta que la animación de salida de la anterior haya concluido.

---

## 🛠️ Código Refactorizado y Listo para Producción

Este es el orquestador de layout que protege el 3D y maneja los datos.

```javascript
import { useState, useEffect } from 'react';
import gsap from 'gsap';
import { BottomLeftCinematicText } from '../animations/gsap-bottom-left-reveal';
import { WebGLScene } from '../3d-experiences/interactive-sphere-core';

/**
 * Simulador de Inyección de Datos (Extraído de la lógica de 'projects')
 * En producción, esto hace un fetch al endpoint o archivo JSON.
 */
const fetchProjectData = async (projectId) => {
  // Simulando el archivo 'projects' de Unseen
  const mockDatabase = {
    'home': { title: "Creando lo imposible" },
    'culto': { title: "Automatización de élite" },
    'design': { title: "Arquitectura asimétrica" }
  };
  return new Promise(resolve => setTimeout(() => resolve(mockDatabase[projectId]), 300));
};

/**
 * Layout Maestro (El Enrutador Transparente)
 */
export const SeamlessAppLayout = () => {
  const [currentProject, setCurrentProject] = useState('home');
  const [projectData, setProjectData] = useState({ title: "Creando lo imposible" });
  const [isTransitioning, setIsTransitioning] = useState(false);

  // Lógica de transición al cambiar de "página"
  const handleNavigate = async (newProjectId) => {
    if (newProjectId === currentProject || isTransitioning) return;
    setIsTransitioning(true);

    // 1. Animación de Salida (Ocultar UI actual deslizando hacia abajo)
    await gsap.to('.ui-layer-content', {
      yPercent: 20,
      opacity: 0,
      duration: 0.6,
      ease: 'power3.in'
    });

    // 2. Inyección Silenciosa de Datos
    const newData = await fetchProjectData(newProjectId);
    setProjectData(newData);
    setCurrentProject(newProjectId);

    // 3. Reset del contenedor para la nueva entrada
    gsap.set('.ui-layer-content', { yPercent: 0, opacity: 1 });
    
    // (La animación de entrada letra por letra se auto-ejecuta al montar 
    // el componente BottomLeftCinematicText con el nuevo texto)

    setIsTransitioning(false);
  };

  return (
    <>
      {/* EL CANVAS INMORTAL: Siempre corriendo en el fondo */}
      <WebGLScene />

      {/* LA CÁSCARA UI: Recibe la inyección de datos */}
      <main className="ui-layer">
        
        {/* Menú de Navegación Interactivo */}
        <nav style={{ position: 'absolute', top: 40, right: 40, zIndex: 20 }}>
          <button 
            className="interactive-element"
            onClick={() => handleNavigate('home')}
            style={{ background: 'none', border: 'none', color: '#fff', marginRight: 20 }}
          >
            Inicio
          </button>
          <button 
            className="interactive-element"
            onClick={() => handleNavigate('culto')}
            style={{ background: 'none', border: 'none', color: '#fff' }}
          >
            Siguiente Proyecto
          </button>
        </nav>

        {/* Contenedor dinámico que se actualiza sin recargar */}
        <div className="ui-layer-content">
          {/* Al cambiar la prop 'key', forzamos a React a desmontar y remontar 
              el componente, disparando de nuevo la animación de GSAP de entrada */}
          <BottomLeftCinematicText key={currentProject} text={projectData.title} />
        </div>

      </main>
    </>
  );
};
