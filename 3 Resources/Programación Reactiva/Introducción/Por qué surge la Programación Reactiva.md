
---
### 🌐 ¿Por qué surge la Programación Reactiva?

Con el crecimiento de aplicaciones complejas y dispositivos conectados (smartphones, relojes inteligentes, etc.), los usuarios esperan:

- **Actualizaciones en tiempo real**
- **Notificaciones automáticas**
- **Interacciones constantes con los servidores**

El modelo tradicional de **solicitud-respuesta** (request-response) ya no es suficiente ni eficiente, ya que no se puede estar consultando al servidor cada segundo.

---
### 🔁 ¿Qué propone la Programación Reactiva?

En lugar de que el cliente pregunte constantemente, el **servidor debe notificar al cliente** cuando haya actualizaciones. Esto requiere un modelo de comunicación más complejo y eficiente.

Por eso, en 2014 se creó la **especificación de flujo reactivo (Reactive Streams Specification)**, que define un estándar para:

- Procesamiento **asíncrono**
- Comunicación **no bloqueante**
- Manejo de **contrapresión** (backpressure)
- Procesamiento de **flujos de datos** (streams), no solo mensajes individuales

---
### 🧠 ¿Qué es la Programación Reactiva?

Es un **nuevo paradigma de programación** diseñado para manejar flujos de datos de forma:

- **Asíncrona**
- **No bloqueante**
- Con **contrapresión** (el sistema controla cuánto puede procesar para no saturarse)

Se basa en el **patrón de diseño del observador**, donde los componentes reaccionan a los cambios o eventos emitidos por otros.

---
### 🧱 Comparación con otros paradigmas

- **Lenguaje C**: Procedimental, reutiliza funciones.
- **Programación orientada a objetos (Java)**: Agrupa datos y métodos, pero no maneja bien llamadas I/O asíncronas.
- **Programación reactiva**: Complementa la POO con herramientas para manejar llamadas I/O no bloqueantes y asíncronas.

---
### 🧩 Conclusión

La programación reactiva **no reemplaza** a la programación orientada a objetos, sino que la **complementa** para enfrentar los desafíos modernos de comunicación en tiempo real y procesamiento eficiente de datos.

---