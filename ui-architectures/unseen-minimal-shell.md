# 📂 Módulo: El Cascarón Mínimo (HTML/DOM Root Shell)

## 🎯 Objetivo
Configurar el punto de entrada principal del documento (`index.html`) y el montaje raíz de React 19. Este módulo asegura que el navegador no desperdicie recursos en renderizados nativos y prepare una jerarquía estricta: un contenedor absoluto para el lienzo 3D en el fondo y una capa transparente superior para la interfaz.

## ⚙️ Stack / Dependencias
* **Lenguaje:** HTML5, JavaScript (React 19 Root).
* **Meta-etiquetas:** Optimizadas para evitar zoom táctil indeseado y escalar correctamente en móviles.

## 🌐 Origen (Ingeniería Inversa)
* **Inspiración:** `index.html` de `unseen.co`.
* **Lógica Clave:** Mantienen el `<head>` limpio, precargan las fuentes críticas y dejan el `<body>` vacío con un solo nodo raíz. No hay maquetación estática; todo se inyecta dinámicamente para que el motor WebGL tenga control total sobre los píxeles de la pantalla.

## 📐 Reglas Arquitectónicas
1. **Bloqueo de Escala:** El viewport debe estar estrictamente bloqueado (`user-scalable=no`) para que los gestos táctiles en móviles se envíen al lienzo 3D (para rotar objetos o hacer scroll inercial) en lugar de hacer zoom en el navegador.
2. **Precarga (Preload):** Las tipografías principales (*Times New Roman* o variaciones formales) deben estar marcadas con `rel="preload"` en el HTML para evitar destellos de texto sin estilo (FOUT) cuando la animación de GSAP arranque.

---

## 🛠️ Código Refactorizado y Listo para Producción

### 1. El `index.html` Maestro
Reemplaza el contenido de tu `index.html` público con esta estructura ultra-optimizada.

```html
<!DOCTYPE html>
<html lang="es">
  <head>
    <meta charset="UTF-8" />
    <title>Culto | Arquitectura de Élite</title>
    
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    
    <link rel="preload" href="/fonts/TimesNewRoman-Italic.woff2" as="font" type="font/woff2" crossorigin="anonymous">
    
    <link rel="stylesheet" href="/main.css" />
  </head>
  <body>
    <div id="root"></div>

    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>

// el montaje de react 19
import React, { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';

// Importación de módulos arquitectónicos
import { SeamlessAppLayout } from './ui-architectures/seamless-routing-state';

// El root de React 19
const rootElement = document.getElementById('root');
const root = createRoot(rootElement);

root.render(
  <StrictMode>
    {/* SeamlessAppLayout (documentado en módulos anteriores) ya contiene 
      internamente el <WebGLScene /> en el fondo y la <div className="ui-layer"> arriba. 
      No inyectar nada más aquí para mantener el DOM flaco.
    */}
    <SeamlessAppLayout />
  </StrictMode>
);
