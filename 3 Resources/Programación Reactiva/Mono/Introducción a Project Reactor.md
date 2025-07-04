
---
### 🚀 Introducción a Project Reactor

- **Reactive Streams** es una **especificación**.
- **Reactor** es una **implementación** de esa especificación, como Hibernate lo es para JPA.
```java
// Example with JPA
@Repository
public interface CustomerRepository extends JpaRepository<Customer, Integer> {
    List<Customer> findByName(String name);
    Optional<Customer> findById(Integer id);
}
```

```java
// Example with Reactor
@Repository
public interface CustomerRepository extends ReactiveCrudRepository<Customer, Integer> {
    Flux<Customer> findByName(String name);
    Mono<Customer> findById(String email);
}
```

- Reactor ofrece dos tipos principales de publishers:
    - **Mono**
    - **Flux**

---

### 🧩 ¿Qué es Mono?

- **Mono** puede emitir **0 o 1 elemento**, seguido de:
    - `onComplete()` si no hay error.
    - `onError()` si ocurre una excepción.
- Ideal para operaciones donde se espera **un solo resultado** o **ninguno**.
- Ejemplo: buscar un cliente por ID en una base de datos.
    - Si existe → se emite un cliente.
    - Si no existe → se emite `onComplete()` sin datos.
- **No hay backpressure** (presión de flujo), ya que no es un flujo continuo.

---

### 🔁 ¿Qué es Flux?

- **Flux** puede emitir **0 a N elementos**, incluso de forma **infinita**.
- Ejemplo: precios de Bitcoin que cambian cada segundo.
- El suscriptor puede pedir datos en lotes:
    - “Dame 3 elementos” → Flux responde con 3.
    - “Ahora dame 5 más” → Flux responde con 5.
- Puede terminar con `onComplete()` o `onError()`.
- **Incluye manejo de backpressure**: útil cuando el productor emite más datos de los que el consumidor puede procesar.

---

### ❓ ¿Por qué usar Mono si ya existe Flux?

- **Mono** es más simple y ligero.
- Útil cuando sabes que solo recibirás **un único resultado**.
- Ejemplo:
    - Buscar por ID → `Mono<Customer>`
    - Buscar por nombre (puede haber varios) → `Flux<Customer>`

---

### ⚙️ Diferencias clave

|Característica|Mono|Flux|
|---|---|---|
|Elementos emitidos|0 o 1|0 a N|
|Flujo continuo|No|Sí|
|Backpressure|No aplica|Sí|
|Uso típico|Consulta por ID|Consulta por nombre, streaming|
|Complejidad|Simple|Más complejo, más flexible|

---

### 🧠 Conclusión

- Usa **Mono** para operaciones simples de solicitud-respuesta.
- Usa **Flux** para flujos de datos múltiples o continuos.
- Ambos trabajan de forma **asíncrona y no bloqueante**, pero Flux ofrece más herramientas para manejar flujos complejos.

---