# 📂 Módulo: Navegación, Selección y Estructuras de Interfaz

## 🎯 Objetivo
Estandarizar el uso de componentes de enrutamiento (Menús de Navegación, Paginación), controles de selección complejos (Selects, Radio Groups) y contenedores estructurales (Scroll Areas, Paneles Redimensionables). Este módulo garantiza que la navegación se sienta como tipografía flotante y que los elementos estructurales no inyecten estilos nativos del navegador que rompan la inmersión WebGL.

## ⚙️ Stack / Dependencias
* **Framework:** React 19
* **Base Tecnológica:** Radix UI Primitives (`@radix-ui/react-navigation-menu`, `@radix-ui/react-select`, `@radix-ui/react-scroll-area`).
* **Estilizado:** Tailwind CSS + utilidades personalizadas.

## 🌐 Filosofía de Diseño (Flujo Ininterrumpido)
* **Lógica Clave:** La navegación no debe estar contenida en "cajas" o "headers" opacos. Los menús deben flotar libremente sobre el lienzo. Las barras de desplazamiento (scrollbars) nativas del sistema operativo están estrictamente prohibidas; todo desbordamiento de contenido debe manejarse con barras superpuestas ultrafinas.

## 📐 Reglas Arquitectónicas y Estéticas Estrictas

### 1. Navegación Flotante (`navigation-menu.jsx`, `menubar.jsx`, `pagination.jsx`)
* **Triggers Transparentes:** Los botones que despliegan menús (`NavigationMenuTrigger`) deben tener fondos totalmente transparentes (`bg-transparent`) en todos sus estados (hover, focus, open). El feedback visual debe darse mediante cambios sutiles de color de texto hacia el tono de acento (`var(--c-accent)`).
* **Tipografía de Menú Principal:** Para la navegación superior global, usar estrictamente la fuente Serif formal (*Times New Roman*, cursiva) en tamaños considerables, dándole un peso editorial.
* **Contenedores Desplegables:** Los submenús que aparecen al hacer hover deben seguir la "Regla de Cristal" (fondos translúcidos con `backdrop-blur` y bordes de 1px al 5% de opacidad).

### 2. Controles de Selección (`select.jsx`, `radio-group.jsx`, `label.jsx`)
* **Selects como Portales:** Al igual que los modales, el menú desplegable de un `<Select />` se inyecta al final del DOM. Debe aplicarse un desenfoque de fondo al contenedor de opciones para no tapar la geometría 3D de forma tosca.
* **Indicadores Minimalistas:** En los `<RadioGroupItem />`, el indicador de selección interno (el círculo) debe usar el color de acento formal en lugar del azul o negro por defecto.
* **Etiquetas (Labels):** Los `<Label />` asociados a estos controles deben usar la tipografía secundaria limpia (`Inter`) en mayúsculas pequeñas con amplio espaciado (tracking-widest) para crear jerarquía sin robar atención.

### 3. Estructuras y Desplazamiento (`scroll-area.jsx`, `resizable.jsx`, `progress.jsx`)
* **Scrollbars Invisibles:** Cualquier lista larga dentro de un componente debe estar envuelta en `<ScrollArea />`. La barra de desplazamiento debe ser una línea finísima (`w-1.5`) de color blanco/gris con muy baja opacidad, que solo se vuelva visible al interactuar.
* **Indicadores de Progreso:** El componente `<Progress />` debe evitar las esquinas redondeadas extremas si el diseño actual tiende a lo arquitectónico/asimétrico. Su riel de fondo debe ser una línea de 1px con un relleno (`Indicator`) en color de contraste.

---

## 🛠️ Código Refactorizado: Ejemplo de Implementación Premium (Navegación)

Cuando la IA deba generar el menú superior principal (TopNav) para la plataforma, debe estructurarlo de la siguiente manera para mantener el estilo "Alta Costura":

```javascript
import {
  NavigationMenu,
  NavigationMenuContent,
  NavigationMenuItem,
  NavigationMenuLink,
  NavigationMenuList,
  NavigationMenuTrigger,
} from "@/components/ui/navigation-menu"
import { cn } from "@/lib/utils"

export const PremiumTopNav = () => {
  return (
    // Posicionamiento absoluto, flotando sobre el lienzo 3D
    <NavigationMenu className="absolute top-0 left-0 w-full p-8 z-50 pointer-events-auto justify-start">
      <NavigationMenuList className="flex gap-12">
        <NavigationMenuItem>
          <NavigationMenuTrigger className="bg-transparent text-white font-serif italic text-2xl hover:text-[var(--c-accent)] hover:bg-transparent focus:bg-transparent data-[state=open]:bg-transparent transition-colors">
            Servicios
          </NavigationMenuTrigger>
          
          {/* El contenido desplegable aplica el efecto de cristal oscuro */}
          <NavigationMenuContent className="bg-[var(--c-ink)]/70 backdrop-blur-xl border-[rgba(255,255,255,0.05)] shadow-2xl p-6 min-w-[300px]">
            <ul className="flex flex-col gap-4">
              <li>
                <NavigationMenuLink asChild>
                  <a href="/automations" className="block text-white hover:text-[var(--c-accent)] font-sans text-sm transition-colors">
                    Automatización de Flujos
                  </a>
                </NavigationMenuLink>
              </li>
              <li>
                <NavigationMenuLink asChild>
                  <a href="/design" className="block text-white hover:text-[var(--c-accent)] font-sans text-sm transition-colors">
                    Arquitectura Digital
                  </a>
                </NavigationMenuLink>
              </li>
            </ul>
          </NavigationMenuContent>
        </NavigationMenuItem>
        
        <NavigationMenuItem>
          <a href="/pricing" className="text-white font-serif italic text-2xl hover:text-[var(--c-accent)] transition-colors">
            Cotización
          </a>
        </NavigationMenuItem>
      </NavigationMenuList>
    </NavigationMenu>
  )
}
