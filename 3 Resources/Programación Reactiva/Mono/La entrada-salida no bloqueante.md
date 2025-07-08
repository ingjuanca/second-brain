
---

La **entrada/salida no bloqueante (non-blocking I/O)** es un modelo de programación que permite que un solo hilo maneje múltiples operaciones de entrada/salida (como solicitudes HTTP, lectura de archivos, etc.) **sin quedarse esperando** a que cada operación termine antes de continuar con la siguiente.

---

### 🧠 ¿Cómo funciona?

1. **Modelo tradicional (bloqueante):**
    
    - El hilo envía una solicitud (por ejemplo, pedir datos a un servidor).
    - Luego **espera** (bloqueado) hasta recibir la respuesta.
    - Esto desperdicia tiempo y recursos si la respuesta tarda.
    
2. **Modelo no bloqueante:**
    
    - El hilo envía la solicitud.
    - **No espera** la respuesta. En su lugar, continúa con otras tareas.
    - Cuando llega la respuesta, el sistema operativo **notifica** al hilo.
    - El hilo procesa la respuesta cuando está lista.

---

### 🔄 ¿Qué es el Event Loop?

- El **event loop** es un **hilo único** que gestiona una **cola de tareas**.
- Cuando se envían solicitudes (por ejemplo, para obtener nombres de productos por ID), estas se colocan en una **cola de salida (outbound queue)**.
- El hilo del event loop **no espera** la respuesta de una solicitud antes de enviar la siguiente. En lugar de eso:
    - Envía la solicitud.
    - Toma la siguiente tarea de la cola.
    - Repite el proceso.

---

### 🎯 Ventajas

- **Alta eficiencia**: un solo hilo puede manejar cientos o miles de solicitudes.
- **Escalabilidad**: ideal para aplicaciones web, microservicios y sistemas reactivos.
- **Menor consumo de recursos**: no se necesitan múltiples hilos para múltiples tareas.

---

### 🧪 Ejemplo práctico:

- Se envían 100 solicitudes HTTP a un servicio externo.
- Cada solicitud tarda 1 segundo en responder.
- Con I/O bloqueante: se necesitarían 100 segundos o 100 hilos.
- Con I/O no bloqueante: **se completan en 1 segundo usando solo 1 hilo**.

---
### 📤 Envío de Solicitudes Concurrentes

- Se pueden enviar **100 solicitudes casi al mismo tiempo**, como lanzar 100 pelotas al aire.
- No hay garantía de **en qué orden** llegarán las respuestas.
- Esto no es un problema de Java ni de la programación reactiva, sino una **característica inherente de las redes** (latencia, carga del servidor, etc.).

---

### 📥 Recepción de Respuestas

- Las respuestas entrantes se colocan en una **cola de entrada (inbound queue)**.
- El sistema operativo notifica al hilo cuando llega una respuesta.
- El hilo procesa las respuestas **a medida que llegan**, sin importar el orden en que se enviaron las solicitudes.

---

### ✅ Conclusión

- **Un solo hilo** puede manejar **cientos de solicitudes concurrentes** gracias al modelo de E/S no bloqueante.
- Este enfoque es eficiente y escalable, ideal para aplicaciones reactivas.

---