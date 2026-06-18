
# 📂 Módulo: Rendimiento Extremo y Decodificación Asíncrona (Draco + WASM)

## 🎯 Objetivo
Evitar el bloqueo del hilo principal (Main Thread) del navegador al cargar o procesar modelos 3D y geometrías pesadas. Este módulo implementa decodificación DRACO mediante WebAssembly (WASM) ejecutado en Web Workers independientes, garantizando que la navegación, el scroll inercial y las animaciones de la interfaz se mantengan a 60 FPS durante la carga de assets.

## ⚙️ Stack / Dependencias
* **Ecosistema:** Web Workers API, WebAssembly (WASM).
* **Librerías:** `three`, `@react-three/drei` (`useGLTF`, `DRACOLoader`).
* **Archivos Estáticos Requeridos:** `draco_decoder.wasm`, `draco_wasm_wrapper.js` (Extraídos de las ingenierías inversas de la carpeta vendor/binarios).

## 🌐 Origen (Ingeniería Inversa)
* **Inspiración:** Arquitectura de carga de `unseen.co` y `Messenger` (Awwwards).
* **Lógica Clave:** Utilizan fragmentos binarios altamente comprimidos. En lugar de procesar la geometría (vértices, caras, normales) con JavaScript puro, envían el archivo a un "trabajador" en segundo plano (Web Worker) que utiliza el decodificador de Google (Draco) en formato WASM (compilado casi a velocidad nativa). Una vez decodificado, el Worker devuelve la geometría lista para inyectarse a la GPU.

## 📐 Reglas Arquitectónicas
1. **Rutas Estáticas Absolutas:** Los archivos del decodificador (`draco_decoder.js`, `draco_decoder.wasm`) deben vivir en la carpeta pública (`/public/draco/`) para que el Web Worker pueda acceder a ellos independientemente del bundle de React.
2. **Instancia Única:** El `DRACOLoader` debe instanciarse una sola vez globalmente para evitar fugas de memoria al crear múltiples Web Workers innecesarios.
3. **Pre-carga:** Las mallas 3D críticas deben usar `<link rel="preload" as="fetch">` en el `<head>` del documento.

---

## 🛠️ Código Refactorizado y Listo para Producción

Este código muestra cómo configurar el entorno de React Three Fiber para devorar modelos pesados sin inmutarse.

```javascript
import { useGLTF } from '@react-three/drei';
import { DRACOLoader } from 'three-stdlib';

/**
 * Configuración Global del Decodificador DRACO.
 * Se establece la ruta a los binarios extraídos (WASM + Wrapper).
 * Drei se encarga de reciclar esta instancia para todas las llamadas posteriores.
 */
const dracoLoader = new DRACOLoader();
// IMPORTANTE: Esta ruta asume que colocaste draco_decoder.js y .wasm en public/draco/
dracoLoader.setDecoderPath('/draco/'); 
dracoLoader.setDecoderConfig({ type: 'wasm' });

/**
 * Hook utilitario para cargar modelos 3D comprimidos.
 * @param {string} path - Ruta del archivo .gltf o .glb comprimido con Draco
 */
export const useOptimizedModel = (path) => {
  // useGLTF internamente usa Suspense, por lo que este componente debe
  // estar envuelto en un <Suspense fallback={<Loader />}> en la UI superior.
  const { nodes, materials } = useGLTF(path, dracoLoader);
  return { nodes, materials };
};

/**
 * Ejemplo de Implementación: Esfera Compleja o Geometría Dinámica
 */
export const ComplexGeometryNode = () => {
  // Carga el archivo binario pesado (ej. modelo hiper-detallado)
  const { nodes } = useOptimizedModel('/assets/models/esfera_fluida_optimizada.glb');

  return (
    <mesh geometry={nodes.EsferaPrincipal.geometry}>
      {/* En lugar de usar el material que viene con el modelo (que puede tener luces caras),
        sobreescribimos con nuestro Matcap hiper-optimizado del módulo interactivo.
      */}
      <meshMatcapMaterial color="#ffffff" />
    </mesh>
  );
};

// Precarga preventiva para que el navegador descargue el modelo antes de renderizarlo
useGLTF.preload('/assets/models/esfera_fluida_optimizada.glb', dracoLoader);
