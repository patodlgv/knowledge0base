
📂 Referencia Externa: React Bits (Componentes Ligeros Animados)


⚠️ Esto es un ÍNDICE de referencia, no código copiado. React Bits es una
librería externa. NO se clona dentro de este repo (es enorme). Se consulta su
fuente cuando se necesita un efecto ligero concreto, se adapta ese componente
al proyecto, y solo si funciona bien se guarda aquí como entrada propia citando
la fuente.



🌐 Fuente


Sitio (catálogo visual con preview + código): https://reactbits.dev
Repositorio: https://github.com/DavidHDev/react-bits
Autor: David Haz (DavidHDev)


⚖️ Licencia (LEER ANTES DE USAR)


MIT + Commons Clause.
✅ Permitido: usar, modificar e integrar los componentes en sitios propios
o de clientes (incluso comerciales). Esto es lo que normalmente queremos.
❌ Prohibido (Commons Clause): vender React Bits como librería/producto
en sí mismo. No hay problema en vender los SITIOS que la usan.
📝 Atribución: al guardar un componente adaptado en este repo, citar que
el origen es React Bits.


🎯 Cuándo usar React Bits (rol en el ecosistema)

React Bits cubre el lado ligero del espectro, opuesto al WebGL pesado de
Unseen/Bruno/Messenger. Encaja en los tiers A (Conversión) y C (Editorial)
del Marco de Decisión del README: efectos que cargan rápido, se montan en
minutos y no matan el rendimiento ni el SEO.

Necesito...React Bits es buena opción si...Un efecto de texto llamativo en un hero (blur, split, shiny, gradient)El sitio es tier A/C y quiero impacto sin WebGLAnimación de entrada / reveal ligeraNo quiero montar GSAP+ScrollTrigger para algo simpleFondos animados ligeros (gradientes, partículas CSS)El fondo debe ser vistoso pero barato en CPUMicro-interacciones y componentes UI animadosEstoy en una landing/portafolio rápido

NO usar React Bits cuando: el proyecto necesita una escena 3D real, shaders,
física o un mundo navegable → eso es tier B (Three.js/R3F, ver 3d-experiences/).

📚 Categorías disponibles en React Bits


Text Animations — blur-text, split-text, shiny-text, gradient-text,
scramble, decrypted, count-up, etc.
Animations — efectos de entrada, hover, click, magnet, etc.
Components — carruseles, stacks, dock, stepper, etc.
Backgrounds — gradientes animados, partículas, aurora, grid, etc.


🛠️ Protocolo de uso


Identificar que el sitio es tier A o C y que un efecto ligero sirve.
Buscar el componente en https://reactbits.dev (tiene preview en vivo).
Copiar la variante correcta (JS/TS + CSS o Tailwind según el stack del proyecto).
Adaptar colores, tiempos y tipografía a la paleta del proyecto (no dejar
los defaults — eso delata "componente de catálogo").
Verificar accesibilidad: respetar prefers-reduced-motion, contraste y que el
texto siga siendo contenido real en el DOM (no romper SEO).
Si quedó bien, guardar la versión adaptada en la carpeta que corresponda
(animations/ o ui-architectures/) citando React Bits como origen.


✍️ Componentes ya adaptados y probados


(vacío por ahora — se irá llenando conforme usemos componentes en proyectos reales)
