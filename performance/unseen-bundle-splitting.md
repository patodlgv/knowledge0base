# 📂 Módulo: Arquitectura de Carga y Separación de Paquetes (Bundle Splitting)

## 🎯 Objetivo
Maximizar la retención en caché del navegador y minimizar el tiempo de bloqueo del hilo principal dividiendo el código de la aplicación en fragmentos estratégicos. Esto asegura que si actualizas una animación o un texto, el usuario no tenga que volver a descargar los motores pesados de 3D o React.

## ⚙️ Stack / Dependencias
* **Empaquetador:** Vite (Estándar moderno para React 19 y WebGL) o Webpack.
* **Despliegue:** Cloudflare Pages / GitHub Pages.

## 🌐 Origen (Ingeniería Inversa)
* **Inspiración:** Lotes de red de `unseen.co` (`manifest.js`, `vendor.js`, `theme.js`).
* **Lógica Clave:** 1. **Vendor:** Contiene todo el código de terceros (Three.js, GSAP, React). Pesa mucho, pero cambia casi nunca. El navegador del usuario lo descarga una vez y lo guarda en caché por meses.
  2. **Theme (App):** Tu código, tus componentes, tus shaders y tus configuraciones de GSAP. Pesa poco, cambia con cada despliegue.
  3. **Manifest (Runtime):** El mapa ligero que le dice al navegador dónde encontrar los otros fragmentos.

## 📐 Reglas Arquitectónicas
1. **Aislamiento de Librerías Core:** Nunca mezclar el código de infraestructura (`@react-three/fiber`, `gsap`) en el mismo archivo que la lógica de la Interfaz (`BottomLeftCinematicText`).
2. **Nombres con Hashes:** Los archivos deben generarse con hashes en el nombre (ej. `vendor.a7f2.js`) para "romper" la caché solo cuando esa pieza específica cambia.

---

## 🛠️ Código Refactorizado y Listo para Producción

Para replicar esta arquitectura exacta en nuestro ecosistema moderno con React 19, utilizamos la configuración de `manualChunks` en Vite. Copia este código en la raíz de tu proyecto como `vite.config.js`.

```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  build: {
    // Configuración agresiva de separación de código (Bundle Splitting)
    rollupOptions: {
      output: {
        manualChunks(id) {
          // 1. VENDOR (three.js, r3f, gsap) - El motor pesado inmutable
          if (id.includes('node_modules')) {
            if (id.includes('three') || id.includes('@react-three')) {
              return 'vendor-3d'; // Empaqueta el ecosistema 3D aparte
            }
            if (id.includes('gsap')) {
              return 'vendor-animation'; // Empaqueta el motor de tiempo
            }
            if (id.includes('react')) {
              return 'vendor-react'; // Empaqueta el core de React 19
            }
            return 'vendor'; // Cualquier otra librería menor
          }
        },
        // 2. THEME - Nuestro código limpio mantiene el nombre default o se agrupa por vista
        entryFileNames: 'assets/[name].[hash].js',
        chunkFileNames: 'assets/[name].[hash].js',
        assetFileNames: 'assets/[name].[hash].[ext]'
      }
    },
    // Eleva el límite de advertencia de tamaño porque nuestro vendor-3d será naturalmente grande,
    // pero lo decodificaremos asíncronamente o lo guardaremos en caché.
    chunkSizeWarningLimit: 800, 
  }
});
