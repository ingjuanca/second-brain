
---

`CompletableFuture` es una clase de Java que forma parte del paquete `java.util.concurrent` y se utiliza para **programación asincrónica y concurrente**. Permite ejecutar tareas en segundo plano sin bloquear el hilo principal, y manejar sus resultados cuando estén disponibles.

---

### 🧠 ¿Para qué se utiliza `CompletableFuture`?

#### 1. **Ejecutar tareas en segundo plano**

Permite ejecutar código de forma asíncrona, por ejemplo:

```java
CompletableFuture.supplyAsync(() -> {
    // tarea costosa
    return "resultado";
});
```
#### 2. **Encadenar operaciones**

Puedes encadenar múltiples pasos de procesamiento:

```java
CompletableFuture.supplyAsync(() -> "Hola")
    .thenApply(s -> s + " mundo")
    .thenAccept(System.out::println);
```
#### 3. **Manejo de errores**

Permite capturar y manejar excepciones de forma fluida:

```java
future.exceptionally(ex -> {
    System.out.println("Error: " + ex.getMessage());
    return "valor por defecto";
});
```
#### 4. **Composición de tareas**

Puedes combinar múltiples `CompletableFuture`:

```java
CompletableFuture<String> f1 = ...;
CompletableFuture<String> f2 = ...;

f1.thenCombine(f2, (a, b) -> a + b);
```
#### 5. **Esperar resultados (si es necesario)**

Aunque es asincrónico, puedes bloquear y esperar el resultado si lo necesitas:

```java
String resultado = future.get(); // bloqueante
```

---

### ✅ ¿Cuándo usarlo?

- Cuando necesitas **paralelizar tareas** sin bloquear el hilo principal.
- Para **consultas a servicios externos**, como APIs o bases de datos.
- En aplicaciones que requieren **alta concurrencia** o **bajo tiempo de respuesta**.
- Para **procesamiento por etapas** (pipeline) de datos.

---