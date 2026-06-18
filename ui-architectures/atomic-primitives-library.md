# 📂 Módulo: Librería de Primitivos Atómicos (UI Base)

## 🎯 Objetivo
Establecer las reglas de renderizado estético para la capa más baja de componentes de la interfaz (Botones, Tarjetas, Badges, Carruseles, Avatares, etc.). Este módulo asegura que cuando la IA ensamble vistas complejas usando estos "bloques de lego", mantengan la estética de "Alta Costura" y la transparencia necesaria para no asfixiar el lienzo WebGL de fondo.

## ⚙️ Stack / Dependencias
* **Framework:** React 19
* **Base Tecnológica:** Radix UI Primitives (`@radix-ui/react-*`), Embla Carousel.
* **Estilizado:** Tailwind CSS + `class-variance-authority` (cva) + `clsx` (utilidad `cn`).

## 🌐 Filosofía de Diseño (La Regla de Cristal)
* **Lógica Clave:** Ningún componente primitivo debe sentirse "pesado". Las tarjetas (`Card`), los botones (`Button`) y los contenedores deben actuar como láminas de cristal sobre el entorno 3D. 

## 📐 Reglas Arquitectónicas y Estéticas Estrictas

### 1. Botones y Badges (`button.jsx`, `badge.jsx`)
* **Prohibido el "Solid Fill" agresivo:** A menos que sea el Call to Action (CTA) principal, los botones deben usar variantes `outline` o `ghost` con bordes ultrafinos (`rgba(255,255,255,0.2)`).
* **Tipografía de Acento:** Los botones principales o insignias premium deben forzar la fuente formal: `font-serif italic tracking-wide`.

### 2. Tarjetas y Contenedores (`card.jsx`, `alert-dialog.jsx`)
* **Fondos Translúcidos:** Nunca usar `bg-white` o `bg-black` puro. Reemplazar por colores de la paleta con opacidad (ej. `bg-[var(--c-ink)]/60`) combinados con `backdrop-blur-md`.
* **Ausencia de Sombras Comunes:** Evitar `shadow-md` o `shadow-lg` estándar del navegador. Si requiere profundidad, usar sombras difusas de color (ej. un resplandor dorado sutil para elementos premium).
* **Eventos de Puntero:** Todo contenedor de la tarjeta debe ser transparente a los clics (`pointer-events-none`), reactivando la interacción (`pointer-events-auto`) única y exclusivamente en sus botones internos o enlaces.

### 3. Carruseles y Navegación (`carousel.jsx`, `breadcrumb.jsx`)
* **Asimetría:** Los controles del carrusel (flechas) no deben estar forzados a los bordes de la pantalla. Deben integrarse de forma asimétrica, preferiblemente flotando cerca del texto descriptivo.
* **Inercia Táctil:** El carrusel de Embla ya provee físicas de arrastre naturales, lo cual hace sinergia perfecta con nuestro Ticker de interpolación (`lerp`).

---

## 🛠️ Código Refactorizado: Ejemplo de Aplicación (Card Premium)

Cuando la IA necesite generar una tarjeta descriptiva flotante, debe inyectar las utilidades de estilo de esta manera:

```javascript
import { Card, CardHeader, CardTitle, CardDescription } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"

export const PremiumGlassCard = ({ title, description, badgeText }) => {
  return (
    <Card className="bg-[var(--c-ink)]/40 backdrop-blur-md border-[rgba(255,255,255,0.05)] shadow-[0_8px_32px_rgba(0,0,0,0.3)] pointer-events-auto hover:-translate-y-1 transition-transform duration-500">
      <CardHeader>
        {badgeText && (
          <Badge variant="outline" className="w-fit mb-4 border-[var(--c-accent)] text-[var(--c-accent)] font-serif italic">
            {badgeText}
          </Badge>
        )}
        <CardTitle className="font-serif italic text-2xl text-white tracking-tight">
          {title}
        </CardTitle>
        <CardDescription className="text-[var(--c-text2)] font-sans text-sm leading-relaxed">
          {description}
        </CardDescription>
      </CardHeader>
    </Card>
  )
}
