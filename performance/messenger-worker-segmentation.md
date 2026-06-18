# 📂 Módulo: Segmentación Extrema y Multi-Threading (Workers Especializados)

## 🎯 Objetivo
Eliminar por completo la carga computacional pesada del Hilo Principal (Main Thread) del navegador. Este módulo establece una arquitectura de delegación basada en Web Workers hiper-específicos, garantizando que el análisis de textos (Glyphs), la decodificación de texturas (Bitmaps) y la matemática tridimensional (Geometry) ocurran en paralelo. Esto mantiene la UI (React 19) y la física de inercia inmutables y fluidas.

## ⚙️ Stack / Dependencias
* **Ecosistema:** Web Workers API, Transferable Objects.
* **Formatos Nativos:** `ImageBitmap` (para texturas), `ArrayBuffer` / `Float32Array` (para vértices).

## 🌐 Origen (Ingeniería Inversa)
* **Inspiración:** Código fuente de *Messenger* (Awwwards) y su colección de scripts (`bitmapworker.js`, `geometryworker.js`, `glyphworker.js`).
* **Lógica Clave:** Si el navegador tiene que cargar una textura 4K o calcular la posición de 10,000 vértices para una animación, la interfaz se congelará. Para evitarlo, envían la URL o los datos crudos a un Worker. El Worker hace la matemática pesada y devuelve "Objetos Transferibles" (`Transferables`), que pasan de la memoria del Worker a la memoria del Hilo Principal instantáneamente, sin costo de clonación.

## 📐 Reglas Arquitectónicas Estrictas
1. **Cero Cálculos de Malla en React:** Los componentes de React o las clases de interfaz NUNCA deben iterar sobre *arrays* masivos para agrupar geometría. Se debe delegar al `geometryworker`.
2. **Decodificación de Texturas en Segundo Plano:** Toda imagen destinada al Canvas WebGL debe pasar por el `bitmapworker` usando `createImageBitmap`, que decodifica asíncronamente en la GPU, en lugar de usar el bloqueante `new Image()`.
3. **Textos 3D (MSDF) Pre-calculados:** El cálculo de los rayos UV y el espaciado tipográfico (Kerning) debe ocurrir en el `glyphworker` antes de inyectarse en el lienzo, permitiendo que la fuente formal Serif fluya sin micro-lag.

---

## 🛠️ Código Refactorizado: El Orquestador de Workers

Este patrón define cómo el ecosistema principal debe comunicarse con sus trabajadores en segundo plano.

### 1. Gestor de Tareas Asíncronas (ThreadManager)
```javascript
/**
 * src/workers/WorkerManager.js
 * Orquestador central para comunicación con Workers sin cuellos de botella.
 */
export class WorkerManager {
  constructor() {
    // Inicialización estática de Workers
    this.workers = {
      bitmap: new Worker(new URL('./bitmapworker.js', import.meta.url), { type: 'module' }),
      geometry: new Worker(new URL('./geometryworker.js', import.meta.url), { type: 'module' }),
      glyph: new Worker(new URL('./glyphworker.js', import.meta.url), { type: 'module' })
    };
    
    this.callbacks = new Map();
    this.msgId = 0;

    // Escuchas globales para atrapar las respuestas
    Object.values(this.workers).forEach(worker => {
      worker.onmessage = (e) => this.handleMessage(e);
    });
  }

  handleMessage(event) {
    const { id, result, error } = event.data;
    if (this.callbacks.has(id)) {
      const { resolve, reject } = this.callbacks.get(id);
      if (error) reject(error);
      else resolve(result);
      this.callbacks.delete(id);
    }
  }

  // Ejemplo de método: Decodificar textura en paralelo
  async loadBitmap(url) {
    return new Promise((resolve, reject) => {
      const id = ++this.msgId;
      this.callbacks.set(id, { resolve, reject });
      
      this.workers.bitmap.postMessage({ id, url });
    });
  }

  // Ejemplo de método: Agrupar geometría compleja
  async batchGeometry(attributes) {
    return new Promise((resolve, reject) => {
      const id = ++this.msgId;
      this.callbacks.set(id, { resolve, reject });
      
      // Enviar usando Transferables (Cero copia de memoria)
      const transferables = Object.values(attributes).map(attr => attr.array.buffer);
      this.workers.geometry.postMessage({ id, action: 'batched', attributes }, transferables);
    });
  }
}
// El Worker Interno Est es la estructura limpia de como operan los archivos que capturaste
/**
 * src/workers/bitmapworker.js
 * Decodifica imágenes fuera del hilo principal.
 */
onmessage = async (event) => {
  const { id, url } = event.data;
  
  try {
    const response = await fetch(url);
    const blob = await response.blob();
    
    // createImageBitmap es la magia: decodifica usando hardware sin trabar el navegador
    const bitmap = await createImageBitmap(blob, {
      colorSpaceConversion: 'none',
      imageOrientation: 'flipY' // Estándar para Three.js/WebGL
    });
    
    // Devolver al hilo principal marcando el bitmap como 'Transferable'
    postMessage({ id, result: bitmap }, [bitmap]);
  } catch (error) {
    postMessage({ id, error: error.message });
  }
};

//
// Instancia Singleton para usar en cualquier parte de la App
export const globalWorkerManager = new WorkerManager();
