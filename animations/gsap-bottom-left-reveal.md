
# 📂 Módulo: Animación Tipográfica Cinemática (Letra por Letra)

## 🎯 Objetivo
Garantizar que las entradas de texto principales no se sientan como plantillas web estándar. Este módulo dicta la lógica de GSAP para revelar elementos tipográficos separando el contenido **letra por letra** mediante un efecto de cascada (stagger), anclando conceptos clave estrictamente en la esquina **inferior izquierda** del viewport.

## ⚙️ Stack / Dependencias
* **Framework:** React 19
* **Motor de Animación:** GSAP (Core)
* **Estilo:** Tipografía Serif (*Times New Roman*, *Italics*) para un contraste elegante.

## 🌐 Origen (Ingeniería Inversa)
* **Inspiración:** Las transiciones de *Unseen* y la manipulación del DOM mediante `theme.js`.
* **Lógica Clave:** En lugar de animar el contenedor completo (lo que genera un bloque pesado), el texto se divide dinámicamente en etiquetas `<span>` individuales utilizando JavaScript nativo. GSAP luego itera sobre cada letra aplicando una máscara de desbordamiento (`overflow: hidden`) y transformaciones en el eje Y.

## 📐 Reglas Arquitectónicas
1. **Separación Estricta:** Quedan prohibidas las animaciones de opacidad agrupadas (fundidos de bloques de texto completos).
2. **Posicionamiento Estratégico:** El texto conceptual debe fijarse en `bottom: 0`, `left: 0` con un padding respetuoso, creando tensión visual asimétrica con el resto de la página.
3. **Optimización de Renderizado:** Usar `yPercent` en lugar de `top` o `margin` dentro de GSAP para evitar el *reflow* del navegador y mantener los cálculos en la GPU.

---

## 🛠️ Código Refactorizado y Listo para Producción

Este hook y componente dividen el texto y ejecutan la coreografía exacta al montarse en el DOM.

```javascript
import { useEffect, useRef } from 'react';
import gsap from 'gsap';

/**
 * Componente: Texto Cinemático Anclado (Bottom-Left)
 * Divide el texto en letras individuales y las anima en secuencia.
 */
export const BottomLeftCinematicText = ({ text = "Creando lo imposible" }) => {
  const containerRef = useRef(null);
  const lettersRef = useRef([]);

  useEffect(() => {
    // Configuración inicial: Asegurar que las letras estén invisibles antes de animar
    gsap.set(lettersRef.current, { yPercent: 110 });

    // La animación: Revelado letra por letra desde abajo hacia su posición original
    const timeline = gsap.timeline({ defaults: { ease: "power4.out" } });

    timeline.to(lettersRef.current, {
      yPercent: 0,
      duration: 1.2,
      stagger: 0.04, // El retraso exacto entre cada letra para el efecto fluido
      delay: 0.2 // Pequeña pausa para que el canvas 3D termine de cargar
    });

    return () => timeline.kill(); // Limpieza para React 19 Strict Mode
  }, []);

  // Función auxiliar para dividir el string en spans individuales
  const renderLetters = (word) => {
    return word.split('').map((char, index) => (
      <span 
        key={index}
        style={{ display: 'inline-block', overflow: 'hidden' }}
      >
        <span 
          ref={(el) => (lettersRef.current[index] = el)}
          style={{ display: 'inline-block' }}
        >
          {char === ' ' ? '\u00A0' : char}
        </span>
      </span>
    ));
  };

  return (
    <div 
      ref={containerRef}
      style={{
        position: 'absolute',
        bottom: '40px',
        left: '40px',
        fontFamily: '"Times New Roman", Times, serif',
        fontStyle: 'italic',
        fontSize: 'clamp(2rem, 4vw, 4rem)',
        color: '#ffffff',
        zIndex: 10,
        pointerEvents: 'none', // Permite que el mouse interactúe con la esfera 3D debajo
        lineHeight: 1.1,
        overflow: 'hidden'
      }}
    >
      {renderLetters(text)}
    </div>
  );
};
