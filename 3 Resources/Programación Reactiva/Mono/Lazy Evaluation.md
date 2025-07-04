
---
### 🧠 Concepto Clave: **(Lazy Evaluation)**

- Aunque esta clase **no trata directamente de programación reactiva**, se usa para explicar un concepto fundamental que **sí aplica**: la **evaluación perezosa**.
- En **Java 8**, las **Streams** son **perezosas por defecto**: no ejecutan nada hasta que se conecta un **operador terminal**.

---

### 🧪 Ejemplo con Java Stream

```java
Stream.of(1)
    .peek(i -> log.info("Recibido: " + i)); // No se ejecuta aún
```

- Aunque se define un `peek()` para imprimir, **no se ejecuta** hasta que se añade un operador terminal como:

```java
Stream.of(1)
    .peek(i -> log.info("Recibido: " + i))
    .toList(); // Ahora sí se ejecuta
```

---

### 🔄 Relación con Programación Reactiva

- **Mono** y **Flux** en programación reactiva funcionan de forma similar:
    - No se ejecutan hasta que se **suscriben** (`subscribe()`).
    - Esto permite construir flujos de datos de forma **declarativa y eficiente**.

---

### ✅ Conclusión

- Tanto **Java Streams** como **Reactor (Mono/Flux)** comparten el principio de **evaluación perezosa**.
- Este comportamiento es esencial para entender cómo y cuándo se ejecuta el código en programación funcional y reactiva.

---