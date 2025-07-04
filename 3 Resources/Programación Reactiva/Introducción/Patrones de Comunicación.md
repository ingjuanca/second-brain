
---
### ⚙️ ¿Por qué usar Programación Reactiva?

Muchas personas se preguntan si realmente necesitan programación reactiva o si basta con usar **hilos virtuales**. Aunque los hilos virtuales son útiles y eficientes para manejar tareas concurrentes, **no siempre son suficientes** para ciertos tipos de comunicación más complejos.

---
### 🧵 Hilos Virtuales vs. Programación Reactiva

- ✅ **Hilos virtuales** son ideales para patrones simples de solicitud-respuesta.
- ❌ Pero **no resuelven** patrones más avanzados de comunicación asincrónica y en tiempo real.
- ✅ **Programación reactiva** permite manejar múltiples patrones de comunicación de forma eficiente y no bloqueante.

---
### 🔄 Patrones de Comunicación que Permite la Programación Reactiva

![[Pasted image 20250702131422.png]]

1. **Solicitud - Respuesta (clásico)**
    
    - Envío una solicitud y recibo una única respuesta.
    - Ejemplo: Consultar el clima.
    
2. **Solicitud - Respuesta en Streaming**
    
    - Envío una solicitud y recibo múltiples respuestas en tiempo real.
    - Ejemplo: Pedir una pizza y recibir actualizaciones como:
        - “Pizza en preparación”
        - “En camino”
        - “Entregada”
    
3. **Streaming de Solicitudes**
    
    - Envío múltiples mensajes al servidor a través de una sola conexión.
    - Ejemplo: Un Apple Watch enviando constantemente la frecuencia cardíaca al servidor.
    
4. **Streaming Bidireccional**
    
    - Ambas aplicaciones se comunican en tiempo real, enviando y recibiendo mensajes continuamente.
    - Ejemplo: Edición colaborativa en Google Docs.
---

### 🧠 Conclusión

- Si solo necesitas una comunicación simple, los **hilos virtuales** son suficientes.
- Si necesitas manejar **comunicaciones en tiempo real, streaming o bidireccionales**, la **programación reactiva** es la mejor opción.
- Lo ideal es combinar ambos enfoques cuando sea necesario: usar hilos virtuales **dentro de un modelo reactivo**.

---