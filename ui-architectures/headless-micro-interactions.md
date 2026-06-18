# 📂 Módulo: Micro-Interacciones UI Minimalistas (Arquitectura Headless)

## 🎯 Objetivo
Estandarizar la creación de componentes de interfaz modulares (Acordeones, Tooltips, Alertas, Toggles, Toasters) en React 19. Estos elementos deben funcionar como una capa interactiva sutil que flota sobre el lienzo WebGL sin saturar visualmente el DOM ni interrumpir el rendimiento gráfico.

## ⚙️ Stack / Dependencias
* **Framework:** React 19
* **Patrón de Diseño:** Componentes "Headless" (Lógica separada del diseño visual, inspirado en Radix UI).
* **Gestión de Estado:** Hooks personalizados (ej. `use-toast.js` para notificaciones efímeras).

## 🌐 Origen y Filosofía
* **Lógica Clave:** La interfaz debe sentirse como "Alta Costura Digital". Los componentes no deben tener sombras pesadas, bordes gruesos ni fondos sólidos opacos. La estructura funcional (el JSX) está dictada por los archivos base (`accordion.jsx`, `tooltip.jsx`), pero el estilo visual debe someterse a la regla de la "Cáscara Transparente".

## 📐 Reglas Arquitectónicas Estrictas
1. **Punteros Aislados:** Cada componente interactivo (el botón del toggle, la cabecera del acordeón) debe llevar explícitamente `pointer-events: auto`, mientras que sus contenedores padre mantienen `pointer-events: none` para no bloquear la interacción con la esfera 3D de fondo.
2. **Estética de Bordes y Fondos:** Quedan prohibidos los fondos blancos o negros al 100% de opacidad. Utilizar desenfoques sutiles (`backdrop-filter: blur()`) o líneas de borde de 1px con alta transparencia (ej. `rgba(255, 255, 255, 0.1)`).
3. **Jerarquía Tipográfica en la UI:** Para los títulos dentro de los acordeones o las notificaciones del toaster, implementar obligatoriamente la clase utilitaria de la fuente formal Serif (*Times New Roman* en cursivas) para mantener la identidad del proyecto. La tipografía Sans-Serif solo se permite para los metadatos secundarios o descripciones microscópicas.

---

## 🛠️ Código Refactorizado y Listo para Producción

Este es el patrón de implementación que la IA debe seguir al construir componentes de interacción para que respeten la dirección de arte.

```javascript
import React, { useState } from 'react';

/**
 * Patrón Maestro: Acordeón Minimalista Transparente
 * Diseñado para convivir con entornos 3D interactivos.
 */
export const MinimalAccordion = ({ title, children }) => {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div 
      className="accordion-container"
      style={{
        borderBottom: '1px solid rgba(255, 255, 255, 0.1)',
        marginBottom: '1rem',
        // Asegura que no bloquee el click al canvas si no es necesario
        pointerEvents: 'none' 
      }}
    >
      <button 
        onClick={() => setIsOpen(!isOpen)}
        style={{
          width: '100%',
          display: 'flex',
          justifyContent: 'space-between',
          alignItems: 'center',
          padding: '1rem 0',
          background: 'transparent',
          border: 'none',
          color: '#ffffff',
          cursor: 'pointer',
          // Reactivamos la interacción solo en el botón
          pointerEvents: 'auto', 
          // Aplicación de la tipografía de "Alta Costura"
          fontFamily: '"Times New Roman", Times, serif',
          fontStyle: 'italic',
          fontSize: '1.5rem',
          textAlign: 'left'
        }}
      >
        <span>{title}</span>
        <span style={{ 
          transform: isOpen ? 'rotate(45deg)' : 'rotate(0)', 
          transition: 'transform 0.3s ease' 
        }}>
          +
        </span>
      </button>

      {/* Contenedor del contenido con animación CSS pura para evitar cargas en JS */}
      <div 
        style={{
          display: 'grid',
          gridTemplateRows: isOpen ? '1fr' : '0fr',
          transition: 'grid-template-rows 0.4s cubic-bezier(0.16, 1, 0.3, 1)',
        }}
      >
        <div style={{ overflow: 'hidden' }}>
          <div style={{ 
            paddingBottom: '1.5rem',
            fontFamily: 'Inter, sans-serif', 
            fontSize: '0.875rem',
            color: 'rgba(255, 255, 255, 0.6)',
            pointerEvents: 'auto'
          }}>
            {children}
          </div>
        </div>
      </div>
    </div>
  );
};
