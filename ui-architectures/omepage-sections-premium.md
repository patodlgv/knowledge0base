# 📂 Módulo: Secciones de Página de Inicio (Layout Editorial Suspendido)

## 🎯 Objetivo
Estandarizar los 8 componentes visuales y de conversión de la página de inicio (`Hero`, `DidYouKnow`, `Demos`, `Gallery`, `Marquee`, `Previews`, `Testimonials`, `CTA`) en React 19. Este módulo elimina el uso de contenedores con fondos sólidos masivos y establece una estructura de secciones fluidas con tipografía elegante y rejillas asimétricas que permiten la visualización continua de la esfera 3D interactiva en el fondo.

## ⚙️ Stack / Dependencias
* **Framework:** React 19
* **Estilizado:** Tailwind CSS + Variables de Cristal Avanzadas.
* **Componentes Atómicos:** Integración con primitivos de Radix UI (`Card`, `Button`, `Badge`).

## 🌐 Filosofía Gráfica (Secciones Líquidas)
* **Lógica Clave:** Una landing page premium no fragmenta la pantalla con franjas alternadas de color blanco y negro. Toda la página de inicio comparte el mismo universo visual. Las secciones se delimitan mediante amplios espacios negativos (paddings verticales holgados) y líneas de separación microscópicas, permitiendo que las animaciones tipográficas letra por letra y los microcomponentes de cristal destaquen con elegancia.

## 📐 Reglas de Composición Estrictas para la IA

### 1. Desbloqueo del Canvas (`pointer-events`)
* Toda sección estructural debe declararse con `pointer-events-none`. 
* Únicamente los elementos interactivos reales (botones, tarjetas con enlaces, inputs, elementos del carrusel) deben reactivar la interactividad usando `pointer-events-auto`. Esto evita interferir con la física de inercia del ratón sobre la esfera 3D.

### 2. Tratamiento Multimedia Abstracto (`CultoHero`)
* Queda prohibido el uso de mockups comerciales literales o videos de stock corporativos genéricos. La sección Hero debe ser un espacio puro de texturas o geometría abstracta fluida.
* Los textos principales se anclan en la sección inferior izquierda, ejecutando su revelado letra por letra mediante GSAP.

### 3. Modulación de Bloques Críticos (`Cards` y `Grids`)
* En secciones densas como `DemosSection`, `AutomationsPreview` o `TestimonialsSection`, las tarjetas deben rechazar fondos planos. Forzar el uso de `bg-[var(--c-ink)]/30 backdrop-blur-md border-[rgba(255,255,255,0.03)]`.
* Los textos descriptivos de soporte deben implementar la tipografía Serif (*Times New Roman*) en variaciones cursivas (*Italics*) para marcar acentos sofisticados y pausas de lectura editoriales.

---

## 🛠️ Estructura Refactorizada y Uniforme para la IA

### 1. El Orquestador de la Landing Page (`src/pages/HomePage.jsx`)
Así es como la IA debe componer estructuralmente la página de inicio para asegurar el correcto flujo de transparencias:

```javascript
import React from 'react';
import { PremiumLayout } from '../components/layout/Layout';
import CultoHero from '../components/home/CultoHero';
import DidYouKnowSection from '../components/home/DidYouKnowSection';
import DemosSection from '../components/home/DemosSection';
import GallerySection from '../components/home/GallerySection';
import IndustriesMarquee from '../components/home/IndustriesMarquee';
import AutomationsPreview from '../components/home/AutomationsPreview';
import TestimonialsSection from '../components/home/TestimonialsSection';
import CTASection from '../components/home/CTASection';

const HomePage = () => {
  return (
    // PremiumLayout ya provee la capa UI transparente sobre el WebGLScene inmortal
    <PremiumLayout className="home-page-view">
      <div className="relative w-full flex flex-col bg-transparent">
        <CultoHero />
        <DidYouKnowSection />
        <DemosSection />
        <GallerySection />
        <IndustriesMarquee />
        <AutomationsPreview />
        <TestimonialsSection />
        <CTASection />
      </div>
    </PremiumLayout>

//Ejemplo de Seccion Refactorizada con Estetica de Cristal
import React from 'react';
import { Zap, Bot, BarChart3, Calendar, ArrowRight } from 'lucide-react';
import { Button } from '@/components/ui/button';
import { Card, CardContent } from '@/components/ui/card';

const features = [
  { icon: Zap, title: 'Flujos programados', desc: 'Sistemas inteligentes de captura.' },
  { icon: Bot, title: 'Chatbots WhatsApp', desc: 'Calificación automática 24/7.' },
  { icon: BarChart3, title: 'Dashboards Analíticos', desc: 'Visualización de métricas en tiempo real.' },
  { icon: Calendar, title: 'Agendas Inteligentes', desc: 'Gestión automatizada de citas.' }
];

export const PremiumAutomationsPreview = () => {
  return (
    <section className="py-24 lg:py-32 bg-transparent pointer-events-none">
      {/* Encabezado con Tensión Asimétrica */}
      <div className="mb-16 max-w-3xl">
        <h2 className="text-3xl lg:text-5xl font-light tracking-tight text-white mb-4">
          Operación <span className="font-serif italic text-[var(--c-accent)]">autónoma</span>
        </h2>
        <p className="text-sm lg:text-base text-[var(--c-text2)] font-sans leading-relaxed">
          Infraestructura digital diseñada para eliminar la fricción operativa de forma invisible.
        </p>
      </div>

      {/* Grid Suspendido sobre el Canvas */}
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
        {features.map((item, idx) => {
          const IconComponent = item.icon;
          return (
            <Card 
              key={idx} 
              className="bg-[var(--c-ink)]/30 backdrop-blur-md border-[rgba(255,255,255,0.04)] shadow-2xl rounded-none pointer-events-auto hover:border-[rgba(255,255,255,0.1)] transition-colors duration-500"
            >
              <CardContent className="p-8 flex flex-col gap-4">
                <div className="w-10 h-10 flex items-center justify-center bg-white/5 border border-white/10 text-[var(--c-accent)]">
                  <IconComponent className="w-5 h-5" />
                </div>
                <div>
                  <h3 className="text-lg font-medium text-white tracking-tight mb-2">{item.title}</h3>
                  <p className="text-xs text-[var(--c-text2)] font-sans leading-relaxed">{item.desc}</p>
                </div>
              </CardContent>
            </Card>
          );
        })}
      </div>
    </section>
  );
};
  );
};

export default HomePage;
