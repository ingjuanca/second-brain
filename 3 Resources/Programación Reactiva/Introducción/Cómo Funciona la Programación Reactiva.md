
---
### 🔁 ¿Cómo Funciona la Programación Reactiva?

La programación reactiva se basa en el **patrón observador**, donde los componentes **observan** y **reaccionan** a los cambios o eventos.

#### 📱 Ejemplo con Twitter (ahora X):

![[Pasted image 20250702134845.png]]

- Un actor famoso publica un tweet → él es el **publicador**.
- Tú lo sigues → eres el **suscriptor**.
- Si tú retuiteas, tus seguidores también lo ven → tú te conviertes en un **procesador** (actúas como suscriptor y publicador).

---
### 🧠 Ejemplo Técnico (con Excel como analogía)

![[Pasted image 20250702134644.png]]

Supongamos que haces una llamada a un servicio remoto para obtener un texto. Tu lógica de negocio es:

1. Obtener la longitud del texto.
2. Multiplicarla por 2.
3. Sumarle 1.

En Excel:

- Cada celda **observa** otra celda.
- Cuando llega el dato, las fórmulas reaccionan automáticamente.
- Esto simula cómo funciona la programación reactiva: **reaccionar en cadena** a los datos que llegan.

---

### 🔧 Especificación Reactive Streams

![[Pasted image 20250702134801.png]]

Define **interfaces estándar** para manejar flujos de datos de forma asíncrona y no bloqueante, con **contrapresión** (backpressure). Los componentes clave son:

- **Publisher**: emite datos (como el actor).
- **Subscriber**: recibe datos (como tú).
- **Subscription**: gestiona la relación entre ambos (puede solicitar más datos o cancelar).
- **Processor**: actúa como puente, siendo a la vez suscriptor y publicador (como tú con tus seguidores).

![[Pasted image 20250702134946.png]]

---

### 🧩 Implementaciones de Reactive Streams

![[Pasted image 20250702134538.png]]

Reactive Streams es solo una **especificación**, no una biblioteca. Algunas implementaciones populares son:

- **Reactor** (usado en este curso, respaldado por Spring)
- **RxJava 2**
- **Akka Streams**

También hay soporte para muchas tecnologías como:

- Bases de datos: PostgreSQL, MySQL, H2 (vía R2DBC)
- Sistemas de mensajería: Kafka, RabbitMQ
- Almacenamiento: Redis, Elasticsearch, MongoDB
- Comunicación: WebSockets

---

### ✅ Conclusión

La programación reactiva permite construir sistemas que **reaccionan automáticamente** a los datos entrantes, de forma **eficiente, no bloqueante y escalable**. Es ideal para aplicaciones modernas que manejan flujos de datos en tiempo real.

---