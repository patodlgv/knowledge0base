# 📂 Módulo: Portales, Overlays y Formularios Translúcidos

## 🎯 Objetivo
Estandarizar el comportamiento y la estética de los componentes flotantes (Modales, Menús Desplegables, Drawers) y los elementos de captura de datos (Inputs, Formularios). Este módulo asegura que las ventanas emergentes mantengan la filosofía de "Cristal" mediante desenfoques de fondo (backdrop-blur) y que los formularios se integren orgánicamente sin usar cajas de fondo sólido que rompan la dirección de arte tridimensional.

## ⚙️ Stack / Dependencias
* **Framework:** React 19
* **Base Tecnológica:** Radix UI Primitives (Focus Scope, Portals, Dismissable Layer).
* **Gestión de Formularios:** `react-hook-form` estandarizado a través de `form.jsx`.

## 🌐 Filosofía de Diseño (Profundidad Óptica)
* **Lógica Clave:** Cuando se abre un Modal (`Dialog`) o un `Drawer`, el lienzo WebGL (Canvas 3D) no debe desaparecer. En su lugar, el fondo del modal debe actuar como un lente de cristal esmerilado que desenfoca la esfera 3D que sigue rotando detrás, creando una sensación de profundidad óptica premium.

## 📐 Reglas Arquitectónicas y Estéticas Estrictas

### 1. Modales y Overlays (`dialog.jsx`, `drawer.jsx`, `command.jsx`)
* **Telones de Fondo (Backdrops):** Prohibido usar `bg-black` o `bg-white` al 100%. Los componentes de `<*Overlay />` deben usar `bg-black/40 backdrop-blur-sm`.
* **Cajas de Contenido:** Los `<*Content />` deben usar fondos translúcidos (`bg-[var(--c-ink)]/80`), combinados con un desenfoque fuerte (`backdrop-blur-xl`) y bordes de 1px con alta transparencia (`border-[rgba(255,255,255,0.08)]`).
* **Animaciones:** Todos los portales deben respetar las animaciones de entrada nativas del archivo base (`data-[state=open]:animate-in fade-in-0 zoom-in-95`), las cuales sinergizan con la suavidad del ecosistema.

### 2. Menús Flotantes (`dropdown-menu.jsx`, `context-menu.jsx`, `hover-card.jsx`)
* **Sombras de Elevación:** Como flotan sobre el canvas 3D interactivo, deben tener sombras difusas agresivas (`shadow-[0_10px_40px_rgba(0,0,0,0.5)]`) para separarse de la geometría tridimensional y asegurar la legibilidad del texto.
* **Micro-interacciones:** Los elementos seleccionables (`<*Item />`) no deben tener colores de selección vibrantes genéricos; deben usar `data-[selected=true]:bg-white/10`.

### 3. Entradas de Datos y Formularios (`input.jsx`, `input-otp.jsx`, `form.jsx`)
* **Estética "Underline" o Borde Fino:** Los `<Input />` NUNCA deben tener fondos blancos o grises sólidos. Deben usar `bg-transparent` con un borde inferior o un borde perimetral muy tenue.
* **Focus States:** Al hacer foco en un input, el anillo de selección (`ring`) debe usar el color de acento formal (`ring-[var(--c-accent)]` o dorado) en lugar del azul genérico del navegador.
* **Tipografía de Input:** El texto ingresado por el usuario debe usar la tipografía Sans-Serif limpia (`Inter`), reservando la fuente Serif formal (`Times New Roman` en cursiva) exclusivamente para los `<FormLabel>`.

---

## 🛠️ Código Refactorizado: Ejemplo de Implementación Premium

Cuando la IA necesite generar un formulario de contacto dentro de un Modal, debe orquestarlo aplicando estas clases utilitarias para respetar la "Alta Costura":

```javascript
import { Dialog, DialogContent, DialogHeader, DialogTitle, DialogOverlay } from "@/components/ui/dialog"
import { Input } from "@/components/ui/input"
import { Label } from "@/components/ui/label"
import { Button } from "@/components/ui/button"

export const PremiumContactModal = ({ isOpen, onOpenChange }) => {
  return (
    <Dialog open={isOpen} onOpenChange={onOpenChange}>
      {/* El overlay permite ver la esfera 3D desenfocada */}
      <DialogOverlay className="bg-[var(--c-ink)]/40 backdrop-blur-md" />
      
      {/* El modal actúa como una lámina de cristal oscuro */}
      <DialogContent className="bg-[var(--c-ink)]/70 backdrop-blur-2xl border-[rgba(255,255,255,0.05)] shadow-2xl sm:max-w-[500px]">
        <DialogHeader>
          <DialogTitle className="font-serif italic text-3xl text-white tracking-tight">
            Inicia tu transformación
          </DialogTitle>
        </DialogHeader>

        <div className="grid gap-6 py-4">
          <div className="grid gap-2">
            <Label htmlFor="email" className="text-[var(--c-text2)] text-xs uppercase tracking-widest">
              Correo Electrónico
            </Label>
            {/* Input transparente con borde sutil y focus dorado */}
            <Input 
              id="email" 
              type="email" 
              placeholder="tu@empresa.com" 
              className="bg-transparent border-[rgba(255,255,255,0.1)] text-white placeholder:text-white/20 focus-visible:ring-[var(--c-accent)] rounded-none border-b-2 border-x-0 border-t-0 px-0 h-12 text-lg"
            />
          </div>
          
          {/* El botón dispara la automatización documentada en el módulo de Webhooks */}
          <Button className="w-full bg-white text-[var(--c-ink)] hover:bg-[var(--c-accent)] hover:text-white transition-colors h-12 font-medium">
            Solicitar Auditoría
          </Button>
        </div>
      </DialogContent>
    </Dialog>
  )
}
