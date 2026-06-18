
# 📂 Módulo: Núcleo Tridimensional y Esfera Interactiva (Matcap)

## 🎯 Objetivo
Establecer el lienzo principal (`Canvas`) utilizando React Three Fiber y renderizar el elemento visual central: una **esfera interactiva 3D**. Este módulo reemplaza cualquier necesidad de usar recursos estáticos, imágenes pesadas o *mockups* genéricos (como laptops o teléfonos) que degradan el rendimiento y la dirección de arte.

## ⚙️ Stack / Dependencias
* **Framework:** React 19
* **Librerías 3D:** `three`, `@react-three/fiber`, `@react-three/drei`
* **Técnica Gráfica:** Materiales Matcap (Iluminación falsa horneada).

## 🌐 Origen (Ingeniería Inversa)
* **Inspiración:** Técnicas de optimización de *Unseen* y *Messenger* (Awwwards). 
* **Hallazgo Clave:** Renderizar cálculos de luces y sombras en tiempo real destruye los cuadros por segundo (FPS). Al usar `meshMatcapMaterial`, mapeamos una textura que ya contiene la iluminación y los reflejos calculados sobre la geometría, logrando un aspecto fotorrealista, pulido o metálico con un costo de procesamiento cercano a cero.

## 📐 Reglas Arquitectónicas
1. **El Canvas como Fondo Global:** El `Canvas` debe existir en el nivel más bajo de la jerarquía de componentes (z-index negativo o contenedor absoluto), permitiendo que la interfaz minimalista flote por encima.
2. **Cero Luces Dinámicas:** No usar `<ambientLight />`, `<directionalLight />` ni `<pointLight />`. Toda la iluminación debe provenir del material Matcap.
3. **Inercia Integrada:** Utilizar el hook nativo `useFrame` de React Three Fiber para rotar la esfera basándose en la posición del puntero, aplicando la misma filosofía matemática de interpolación lineal (*lerp*) para darle peso y fluidez al objeto.

---

## 🛠️ Código Refactorizado y Listo para Producción

Este componente define la esfera interactiva y la conecta directamente al ciclo de renderizado optimizado de WebGL.

```javascript
import { useRef } from 'react';
import { Canvas, useFrame } from '@react-three/fiber';
import { useTexture } from '@react-three/drei';
import * as THREE from 'three';

// URL a tu textura Matcap (Una imagen cuadrada, ej. 512x512, de una esfera iluminada)
// Puedes encontrar matcaps gratuitos de alta calidad en repositorios de Three.js
const MATCAP_TEXTURE_URL = '/assets/matcaps/black-metal.png';

/**
 * Componente de la Esfera Interactiva
 * Reacciona al movimiento del mouse con física de inercia pura.
 */
const InteractiveSphere = () => {
  const sphereRef = useRef(null);
  
  // Cargamos la textura Matcap. Drei maneja el caché automáticamente.
  const matcap = useTexture(MATCAP_TEXTURE_URL);

  // useFrame corre a 60FPS sincronizado con el monitor del usuario.
  // Provee el 'delta' (tiempo entre frames) para mantener la velocidad consistente.
  useFrame((state, delta) => {
    if (!sphereRef.current) return;

    // Obtenemos la posición normalizada del mouse (-1 a 1) provista por R3F
    const targetX = state.pointer.x;
    const targetY = state.pointer.y;

    // Aplicamos interpolación lineal (lerp) para la rotación, generando la inercia
    // La esfera persigue al mouse suavemente en lugar de saltar
    const factor = 1 - Math.pow(0.05, delta * 60); // Ajuste al delta-time
    
    sphereRef.current.rotation.x = THREE.MathUtils.lerp(sphereRef.current.rotation.x, targetY * 0.5, factor);
    sphereRef.current.rotation.y = THREE.MathUtils.lerp(sphereRef.current.rotation.y, targetX * 0.5, factor);
  });

  return (
    <mesh ref={sphereRef}>
      {/* Geometría de alta resolución (64x64 segmentos para que sea perfectamente redonda) */}
      <sphereGeometry args={[1.5, 64, 64]} />
      {/* Material hiper-optimizado sin cálculo de luces en tiempo real */}
      <meshMatcapMaterial matcap={matcap} />
    </mesh>
  );
};

/**
 * Escenario Principal (Contenedor WebGL)
 * Este componente debe envolver toda la experiencia visual de fondo.
 */
export const WebGLScene = () => {
  return (
    <div style={{ position: 'fixed', top: 0, left: 0, width: '100vw', height: '100vh', zIndex: -1 }}>
      <Canvas 
        camera={{ position: [0, 0, 5], fov: 45 }}
        dpr={[1, 2]} // Limita el pixel ratio a 2 para evitar saturar pantallas Retina/4K
        gl={{ antialias: true, alpha: true }} // alpha: true permite que el fondo del CSS se vea
      >
        <InteractiveSphere />
      </Canvas>
    </div>
  );
};
