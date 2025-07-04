
---
### 🧠 Resumen General de Programación Reactiva

#### 🧵 Hilos y CPU

- Más hilos **no significan mejor rendimiento**.
- Una CPU solo puede ejecutar **un hilo a la vez**.
- Lo ideal: **un hilo por CPU**.
- Se crean muchos hilos por culpa de llamadas de red lentas y bloqueantes.
- Solución: usar **comunicación no bloqueante** para aprovechar mejor la CPU.

---

### 🔁 Programación Tradicional vs. Reactiva

|Tradicional (Pull)|Reactiva (Push + Pull)|
|---|---|
|Basada en solicitud-respuesta|Soporta múltiples patrones de comunicación|
|El cliente debe pedir constantemente|El servidor puede enviar datos automáticamente|

---

### 📡 Patrones de Comunicación en Programación Reactiva

1. **Solicitud - Respuesta**  
    → Modelo clásico.
    
2. **Solicitud - Respuesta en Streaming**  
    → Ejemplo: precios de acciones o criptomonedas que cambian cada segundo.
    
3. **Streaming de Solicitudes**  
    → Ejemplo: micrófono que envía audio en tiempo real a un servidor.
    
4. **Streaming Bidireccional**  
    → Ejemplo: videojuegos en línea.
    
---
### ⚙️ Principios Clave de la Programación Reactiva

- Procesamiento de **flujos de datos**.
- Comunicación **asíncrona** y **no bloqueante**.
- Manejo de **contrapresión** (backpressure).
- Visualización de componentes como:
    - **Publisher**: quien emite datos.
    - **Subscriber**: quien consume datos.
---
### 📌 Reglas Importantes

- El **suscriptor** debe iniciar la comunicación con `subscribe()` y `request()`.
- El **productor** (publisher) **no debe producir nada hasta que se le pida**.
- El suscriptor puede **cancelar** en cualquier momento.
- El productor debe:
    - Enviar datos con `onNext()`.
    - Finalizar con `onComplete()` si no hay más datos.
    - Notificar errores con `onError()`.
- Después de `onComplete()` o `onError()`, **no se debe enviar nada más**.
