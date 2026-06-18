# 📂 Módulo: Pipeline de Automatización y Orquestación Backend

## 🎯 Objetivo
Desacoplar completamente la lógica de negocio y el procesamiento de datos del hilo principal (Frontend). Este módulo establece el estándar para conectar la capa visual (React 19) con la infraestructura de automatización a través de Webhooks asíncronos. Esto garantiza que la esfera 3D y las animaciones mantengan sus 60 FPS estables mientras el servidor hace el trabajo pesado.

## ⚙️ Stack / Dependencias
* **Frontend:** Fetch API, Hooks asíncronos en React.
* **Orquestador Principal:** n8n (Manejo de rutas, condicionales y webhooks).
* **Procesamiento y Datos:** Python (Microservicios, algoritmos pesados), Outscraper (Extracción de datos).
* **Comunicación Cliente:** Manychat (Flujos automatizados de WhatsApp).

## 🌐 Filosofía de Diseño (Fire and Forget)
* **Lógica Clave:** La interfaz web actúa únicamente como un "gatillo". Cuando un usuario envía un formulario en `ContactPage` o selecciona una opción en `AutomationsPage`, React dispara un POST a un webhook de n8n y se olvida del proceso. No hay "polling" pesado. Las respuestas al cliente se manejan de forma externa (ej. un mensaje automático por WhatsApp).

## 📐 Reglas Arquitectónicas Estrictas
1. **Delegación de Nodos (n8n):** Las llamadas a APIs de terceros (como iniciar una tarea de scraping con Outscraper) NUNCA deben hacerse desde el frontend por seguridad de las API Keys y por rendimiento. El frontend llama a n8n, y n8n orquesta los servicios.
2. **Microservicios Aislados (Python):** Si la automatización requiere transformar grandes volúmenes de datos o inteligencia artificial compleja, n8n debe ejecutar un script de Python dedicado en el servidor, recoger la respuesta y continuar el flujo.
3. **Manejo de Estado Efímero:** Usar el módulo `toaster.jsx` (Librería Atómica) para dar feedback inmediato al usuario ("Solicitud en proceso"), mientras Manychat toma el control de la conversación asíncrona real.

---

## 🛠️ Código Refactorizado: Hook Integrador de Automatización

Este es el patrón que la IA debe usar cada vez que necesite conectar un botón o formulario de la interfaz a tus flujos de trabajo.

```javascript
import { useState } from 'react';
import { useToast } from '@/hooks/use-toast';

/**
 * Hook de Orquestación Backend
 * Permite disparar webhooks de n8n de forma silenciosa.
 */
export const useAutomationWebhook = (webhookUrl) => {
  const [isProcessing, setIsProcessing] = useState(false);
  const { toast } = useToast();

  const triggerFlow = async (payload, successMessage = "Proceso iniciado correctamente.") => {
    setIsProcessing(true);

    try {
      // Llamada asíncrona no bloqueante ('Fire and Forget' ligero)
      const response = await fetch(webhookUrl, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          timestamp: new Date().toISOString(),
          source: 'web-canvas-ui',
          data: payload
        }),
      });

      if (!response.ok) throw new Error('Error en el nodo de entrada');

      // Notificación minimalista en pantalla
      toast({
        title: "Automatización activada",
        description: successMessage,
        className: "font-serif italic border-[rgba(255,255,255,0.1)] backdrop-blur-md"
      });

      /* A partir de aquí, el Frontend termina su trabajo.
        En el Backend:
        1. n8n recibe el payload.
        2. Llama a Python / Outscraper si necesita enriquecer los datos.
        3. Dispara Manychat para enviar el PDF/Cotización al WhatsApp del usuario.
      */

      return true;
    } catch (error) {
      console.error("Fallo en la comunicación con el orquestador:", error);
      toast({
        variant: "destructive",
        title: "Error de conexión",
        description: "No se pudo iniciar el flujo. Intente nuevamente.",
      });
      return false;
    } finally {
      setIsProcessing(false);
    }
  };

  return { triggerFlow, isProcessing };
};
