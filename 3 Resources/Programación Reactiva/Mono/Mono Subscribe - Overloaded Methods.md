
---

A continuación se explora las diferentes formas de suscribirse a un `Mono` en **Project Reactor** y cómo manejar eventos como datos recibidos, errores y finalización:

### ✅ Suscripción básica

```java
Mono.just(1)
    .subscribe(value -> System.out.println("Recibido: " + value));
```

- Solo se maneja el valor emitido (`onNext`).
- No se imprime `onComplete` porque no se pasó un manejador para ello.

```bash
Recibido: 1
Process finished with exit code 0
```

---

### 🔄 Suscripción completa

```java
Mono.just(1)
    .subscribe(
        value -> System.out.println("Recibido: " + value),       // onNext
        error -> System.out.println("Error: " + error),          // onError
        () -> System.out.println("Completado")                   // onComplete
    );
```

- Ahora se manejan todos los eventos.
- El `Mono` emite el valor y luego la señal de completado.

---

### ❓ ¿Por qué no se necesita `request()`?

- Cuando usas `subscribe()` con lambdas, **Reactor internamente hace la solicitud automáticamente** (`request(Long.MAX_VALUE)`).
- Por eso no necesitas llamar manualmente a `request()`.

---

### 🔧 Control manual con `Subscription`

Si quieres controlar la suscripción tú mismo:

```java
Mono.just(1)
    .subscribe(
        value -> System.out.println("Recibido: " + value),
        error -> System.out.println("Error: " + error),
        () -> System.out.println("Completado"),
        subscription -> subscription.request(1) // o subscription.cancel()
    );
```

- Puedes cancelar antes de recibir el valor → no se emite nada.
- Puedes solicitar explícitamente cuántos valores deseas.

```bash
Recibido: 1
Completado
Process finished with exit code 0
```

---

### 🔁 Transformaciones con `map()`

```java
Mono.just(1)
    .map(i -> i + "A")
    .subscribe(System.out::println);
```

- Transforma el valor antes de emitirlo.

```bash
1A
Process finished with exit code 0
```

---

### ⚠️ Manejo de errores

```java
Mono.just(1)
    .map(i -> i / 0) // Provoca una excepción
    .subscribe(
        value -> System.out.println("Recibido: " + value),
        error -> System.out.println("Error: " + error)
    );
```

- Si ocurre una excepción en `map()`, se invoca `onError`.

```bash
Error: java.lang.ArithmeticException: / by zero
Process finished with exit code 0
```

---

### 🧠 Conclusión

- `Mono.subscribe()` tiene múltiples formas de uso, desde simples hasta avanzadas.
- Puedes manejar eventos de forma declarativa y funcional.
- Reactor automatiza muchas tareas, pero también te da control total si lo necesitas.

---