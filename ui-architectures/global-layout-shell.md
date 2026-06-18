# 📂 Módulo: Armadura del Layout Global (Header, Footer y Contenedores)

## 🎯 Objetivo
Estandarizar la estructura envolvente principal (`Layout`, `Header`, `Footer`) en React 19. Este módulo elimina el comportamiento tradicional de bloques divisores o fondos opacos y los convierte en una armadura translúcida flotante. Esto asegura que el lienzo WebGL respire en toda la pantalla y que la interfaz mantenga un tono editorial minimalista y asimétrico de altísimo nivel.

## ⚙️ Stack / Dependencias
* **Framework:** React 19 / React Router
* **Estilizado:** Tailwind CSS + Variables CSS Nativas (`--c-ink`, `--c-accent`).
* **Componentes de Soporte:** Radix UI Sheet (para navegación móvil sin bloqueos).

## 🌐 Filosofía del Layout Inmersivo
* **Lógica Clave:** En un sitio web premium, el fondo nunca es estático. Por lo tanto, el componente `Layout` maestro no debe aplicar colores de fondo sólidos (`bg-[var(--c-bg)]`) que bloqueen la GPU o tapen la escena 3D. El Header y el Footer se tratan como superposiciones puras con `pointer-events: none` global, activando los clics individualmente en los enlaces interactivos.

## 📐 Reglas de Refactorización y Dirección de Arte

### 1. El Contenedor Maestro (`Layout.jsx`)
* **Prohibición de Bloqueo:** Quedan eliminados los divs intermedios que corten o limiten la altura del viewport. El layout debe ser una ventana absoluta de `min-h-screen` con flujo transparente.
* **Margen de Respiro:** El área principal (`<main>`) debe coordinar sus espaciados superiores e inferiores mediante rellenos elásticos (`pt-16 lg:pt-20`) para evitar colisiones tipográficas con el menú flotante.

### 2. El Menú de Navegación (`Header.jsx`)
* **Evolución Editorial:** El logo y los enlaces de navegación principales deben incorporar sutiles contrastes tipográficos. Para acentos formales, implementar variaciones Serif (*Times New Roman*) en cursiva (*Italics*).
* **Fondo Dinámico Inteligente:** La barra de navegación debe arrancar con transparencia absoluta. En caso de scroll pesado, la transición hacia el desenfoque de fondo (`backdrop-blur-md bg-[var(--c-ink)]/10`) debe ocurrir mediante clases fluidas aceleradas por hardware.
* **Menú Móvil Transparente:** La vista móvil integrada mediante el componente `Sheet` debe desplegarse como una lámina esmerilada escura (`bg-[var(--c-ink)]/80 backdrop-blur-xl`), impidiendo el uso de fondos planos sólidos.

### 3. El Cierre Estructural (`Footer.jsx`)
* **Integración y Aire Visual:** El footer ya no es una caja negra pesada al fondo de la página. Se convierte en una sección de tipografía fina y de bajo contraste.
* **Separadores Arquitectónicos:** Reemplazar los bordes gruesos por líneas de 1px con opacidad microscópica (`border-[rgba(255,255,255,0.05)]`). Los enlaces de contacto (WhatsApp, Mail) deben usar micro-interacciones suaves que cambien al tono de acento dorado (`var(--c-accent)`).

---

## 🛠️ Estructura Refactorizada y Uniforme para la IA

### 1. El Componente Layout Limpio
```javascript
import React from 'react';
import Header from './Header';
import Footer from './Footer';

export const PremiumLayout = ({ children, className = '' }) => {
  return (
    // Estructura totalmente transparente que permite ver el canvas 3D global en el fondo
    <div className={`min-h-screen flex flex-col bg-transparent text-white ${className}`.trim()}>
      <Header />
      
      {/* Capa de contenido con pointer-events neutralizado para no trabar el drag de la esfera 3D */}
      <main className="flex-1 pt-20 lg:pt-24 pointer-events-none">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 w-full">
          {children}
        </div>
      </main>



//Header Flotante Premium
import React, { useState, useEffect } from 'react';
import { Menu, MessageCircle } from 'lucide-react';
import { Button } from '@/components/ui/button';
import { Sheet, SheetContent, SheetTrigger, SheetOverlay } from '@/components/ui/sheet';

export const PremiumHeader = () => {
  const [scrolled, setScrolled] = useState(false);

  useEffect(() => {
    const handleScroll = () => {
      setScrolled(window.scrollY > 40);
    };
    window.addEventListener('scroll', handleScroll, { passive: true });
    return () => window.removeEventListener('scroll', handleScroll);
  }, []);

  return (
    <header 
      className={`fixed top-0 left-0 right-0 z-50 transition-all duration-500 pointer-events-none ${
        scrolled 
          ? "bg-[var(--c-ink)]/40 backdrop-blur-md border-b border-[rgba(255,255,255,0.05)] py-4" 
          : "bg-transparent py-6"
      }`}
    >
      <nav className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex items-center justify-between">
        {/* Identidad de Marca Editorial */}
        <a href="/" className="text-2xl font-bold tracking-tight text-white pointer-events-auto hover:text-[var(--c-accent)] transition-colors">
          Culto
        </a>

        {/* Enlaces de Escritorio Flotantes */}
        <div className="hidden md:flex items-center gap-12 pointer-events-auto">
          <a href="/automations" className="text-sm font-sans tracking-wide text-white/70 hover:text-white transition-colors">Automatizaciones</a>
          <a href="/pricing" className="text-sm font-sans tracking-wide text-white/70 hover:text-white transition-colors">Precios</a>
          <a href="/contacto" className="text-sm font-serif italic text-[var(--c-accent)] hover:text-white transition-colors text-lg">Contacto</a>
        </div>

        {/* Acción CTA Lateral */}
        <div className="hidden md:flex pointer-events-auto">
          <Button className="bg-white text-[var(--c-ink)] hover:bg-[var(--c-accent)] hover:text-white rounded-none font-sans px-6 h-10 transition-all">
            Hablar ahora
          </Button>
        </div>

        {/* Menú de Navegación Móvil */}
        <div className="md:hidden flex pointer-events-auto">
          <Sheet>
            <SheetTrigger asChild>
              <Button variant="ghost" size="icon" className="text-white hover:bg-white/5">
                <Menu className="w-6 h-6" />
              </Button>
            </SheetTrigger>
            <SheetContent className="bg-[var(--c-ink)]/80 backdrop-blur-2xl border-l-[rgba(255,255,255,0.05)] text-white">
              <div className="flex flex-col gap-8 mt-12">
                <a href="/automations" className="text-xl font-sans text-white/80">Automatizaciones</a>
                <a href="/pricing" className="text-xl font-sans text-white/80">Precios</a>
                <a href="/contacto" className="text-2xl font-serif italic text-[var(--c-accent)]">Contacto</a>
              </div>
            </SheetContent>
          </Sheet>
        </div>
      </nav>
    </header>
  );
};
      <Footer />
    </div>
  );
};
