# 📂 Módulo: Feedback Visual, Paneles Laterales y Datos (UI Extendida)

## 🎯 Objetivo
Estandarizar los componentes de notificación (Toasts/Sonner), ventanas deslizantes (Sheets), organización de información (Tables, Tabs) y controles de entrada extendidos. Este módulo garantiza que los elementos informativos mantengan el rigor del diseño de "Alta Costura" y la materialidad de cristal, asegurando que la esfera 3D interactiva de fondo nunca quede oculta tras bloques de color opaco.

## ⚙️ Stack / Dependencias
* **Framework:** React 19
* **Base Tecnológica:** Radix UI Primitives, Sonner (Notificaciones efímeras).
* **Estilizado:** Tailwind CSS + utilidades de fusión (`cn`).

## 🌐 Filosofía de Diseño (Superposición Orgánica)
* **Lógica Clave:** La información densa (Tablas, Paneles) y las alertas deben sentirse como capas de información proyectadas en vidrio. Las transiciones de los `Sheets` deben deslizarse suavemente sin alterar los 60 FPS del entorno WebGL.

## 📐 Reglas Arquitectónicas y Estéticas Estrictas

### 1. Paneles Laterales (`sheet.jsx`)
* **Overlays Esmerilados:** El telón de fondo (`SheetOverlay`) debe usar `bg-black/40 backdrop-blur-md`. 
* **Fondos de Panel:** El `SheetContent` NUNCA debe ser sólido. Implementar `bg-[var(--c-ink)]/75 backdrop-blur-2xl` con un borde interior casi invisible (`border-l border-[rgba(255,255,255,0.05)]`).
* **Tipografía de Cabecera:** Los títulos principales del panel (`SheetTitle`) requieren obligatoriamente la familia Serif formal (*Times New Roman*, cursiva) para mantener la jerarquía elegante.

### 2. Notificaciones y Feedback (`toast.jsx`, `sonner.jsx`, `skeleton.jsx`)
* **Toasts Flotantes:** Las notificaciones no deben tener fondos blancos o negros planos. Utilizar la utilidad de cristal oscuro, bordes finos translúcidos y sombras difusas (`shadow-[0_8px_30px_rgba(0,0,0,0.3)]`).
* **Skeletons (Estados de Carga):** El parpadeo (`animate-pulse`) de los skeletons (`skeleton.jsx`) debe ser extremadamente sutil, utilizando colores de muy baja opacidad (ej. `bg-white/5`) para no generar destellos agresivos en la pantalla.

### 3. Estructuración de Datos (`table.jsx`, `tabs.jsx`, `separator.jsx`)
* **Tablas Limpias:** Las cabeceras (`TableHeader`) no deben tener colores de fondo invertidos. Todo se estructura mediante separadores (`separator.jsx`) súper finos (`h-[1px] bg-[rgba(255,255,255,0.1)]`). Las filas en *hover* solo deben aumentar ligeramente la luminosidad del cristal (`hover:bg-white/5`).
* **Pestañas (Tabs):** La lista de pestañas (`TabsList`) debe evitar el clásico fondo encapsulado gris. Los triggers (`TabsTrigger`) activos deben destacarse con el color de acento (`var(--c-accent)`) y una línea sutil, no con un bloque de color.

### 4. Controles Extendidos (`slider.jsx`, `switch.jsx`, `textarea.jsx`)
* **Focos de Interacción:** Al arrastrar un `Slider` o activar un `Switch`, los indicadores deben utilizar el color de acento (dorado/premium) eliminando los anillos de selección (rings) azules predeterminados del navegador.
* **Textareas:** Siguen la misma regla de los inputs: fondo transparente (`bg-transparent`), texto en fuente Sans-Serif limpia y bordes inferiores o perimetrales de baja opacidad.

---

## 🛠️ Código Refactorizado: Ejemplo de Implementación Premium (Panel Lateral)

Cuando la IA necesite generar un menú de configuración, un carrito de compras o un resumen de automatización deslizante, debe estructurar el `Sheet` aplicando la estética de la siguiente manera:

```javascript
import {
  Sheet,
  SheetContent,
  SheetDescription,
  SheetHeader,
  SheetTitle,
  SheetTrigger,
  SheetOverlay
} from "@/components/ui/sheet"
import { Button } from "@/components/ui/button"
import { Separator } from "@/components/ui/separator"

export const PremiumSidePanel = () => {
  return (
    <Sheet>
      <SheetTrigger asChild>
        <Button variant="outline" className="border-[rgba(255,255,255,0.2)] text-white hover:text-[var(--c-accent)] bg-transparent">
          Configuración
        </Button>
      </SheetTrigger>
      <SheetContent className="bg-[var(--c-ink)]/75 backdrop-blur-2xl border-l-[rgba(255,255,255,0.05)] text-white shadow-2xl sm:max-w-md">
        <SheetHeader className="mb-6">
          <SheetTitle className="font-serif italic text-3xl font-normal tracking-wide text-white">
            Parámetros del Sistema
          </SheetTitle>
          <SheetDescription className="text-[var(--c-text2)] font-sans text-sm">
            Ajusta los detalles de tu orquestación.
          </SheetDescription>
        </SheetHeader>
        
        <Separator className="bg-[rgba(255,255,255,0.08)] mb-6" />
        
        <div className="grid gap-6">
          {/* Contenido UI interactivo (Switches, Sliders, etc.) */}
        </div>
      </SheetContent>
    </Sheet>
  )
}
