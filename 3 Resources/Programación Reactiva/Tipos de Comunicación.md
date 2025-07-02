
---

![[Pasted image 20250702125135.png]]

### 📞 Tipos de Comunicación I/O

#### 1. ** 🧍‍♂️ Comunicación Sincrónica y Bloqueante**

- **En sistemas**: Una aplicación envía una solicitud y el hilo queda inactivo hasta recibir respuesta.
- **Características**:
    - Simple pero ineficiente.
    - El hilo no puede hacer otra cosa mientras espera.
- **Analogía del documento:** 

```text
Llamas a la aseguradora y esperas en la línea hasta que alguien conteste. No puedes hacer nada más mientras tanto.
```

#### 2. ** 👬Comunicación Asíncrona**

- **En sistemas**: Un hilo delega la tarea a otro hilo.
- **Características**:
    - El hilo principal no se bloquea.
    - El hilo delegado sí queda bloqueado esperando la respuesta.
- **Analogía del documento:** 

```text
Le pides a un amigo que llame a la aseguradora por ti. Tú sigues con tus cosas, pero tu amigo tiene que esperar en la línea.
```

#### 3. ** 📞Comunicación No Bloqueante**

- **En sistemas**: El hilo envía una solicitud y sigue trabajando. El sistema operativo lo notifica cuando llega la respuesta.
- **Características**:
    - El hilo nunca se bloquea.
    - Alta eficiencia en el uso de recursos.
- **Analogía del documento:** 

```text
Llamas a la aseguradora, dejas tu número y ellos te llaman cuando estén disponibles. Tú puedes hacer otras cosas mientras tanto.
```

#### 4. **🔄Comunicación Asíncrona y No Bloqueante**

- **En sistemas**: Un hilo envía la solicitud y otro hilo maneja la respuesta cuando llega.
- **Características**:
    - Ningún hilo se bloquea.
    - Ideal para sistemas con múltiples núcleos de CPU.
    - Mayor complejidad, pero máxima eficiencia.
- **Analogía del documento:** 

```text
Das el número de tu amigo a la aseguradora para que lo llamen cuando estén disponibles. Ni tú ni tu amigo están bloqueados, pero él recibirá la llamada cuando llegue la respuesta.
```

---
### 🧠 Conclusión

- Estos modelos de I/O se pueden clasificar por complejidad:
    1. Sincrónico bloqueante (más simple)
    2. Asíncrono
    3. No bloqueante
    4. Asíncrono + No bloqueante (más complejo)
- **Programación reactiva** es un enfoque diseñado para manejar este último modelo de forma más sencilla y eficiente.

---