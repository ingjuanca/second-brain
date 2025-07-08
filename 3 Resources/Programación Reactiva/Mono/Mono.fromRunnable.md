
---
### 🎯 ¿Qué es `Mono.fromRunnable()`?

Es una forma de crear un `Mono` que:

- **No emite ningún valor** (`onNext` nunca se llama).
- Solo ejecuta una **acción (Runnable)** cuando alguien se suscribe.
- Luego emite una señal de **completado (`onComplete`)**.

---

### 🧠 ¿Por qué usarlo?

- Similar a `Mono.empty()`, pero con una **acción previa**.
- Útil cuando necesitas **ejecutar algo sin devolver datos**, pero **solo si hay suscriptores**.

---

### 📌 Comparación con otros métodos

|Método|¿Emite valor?|¿Ejecuta lógica?|¿Cuándo se ejecuta?|
|---|---|---|---|
|`Mono.just(valor)`|✅ Sí|❌ No|Inmediatamente|
|`Mono.fromSupplier()`|✅ Sí|✅ Sí|Solo al suscribirse|
|`Mono.empty()`|❌ No|❌ No|Inmediatamente|
|`Mono.fromRunnable()`|❌ No|✅ Sí|Solo al suscribirse|

---

### 🧪 Ejemplo práctico

Supongamos que tienes un método que **notifica al equipo de negocio** cuando un producto no está disponible:

```java
private static void notifyBusiness(int productId) {
    log.info("Notificando producto no disponible: " + productId);
}
```

Y un método que devuelve el nombre del producto:

```java
private static Mono<String> getProductName(int productId){  
    if(productId == 1){  
        return Mono.fromSupplier(() -> Util.faker().commerce().productName());  
    }  
    return Mono.fromRunnable(() -> notifyBusiness(productId));  
}
```

- Si el producto existe → se devuelve un nombre.

```java
public static void main(String[] args) {  
     getProductName(1)  
            .subscribe(Util.subscriber());   
}
```

```bash
11:00:04.424 INFO  [           main] c.v.common.DefaultSubscriber   :  received: Ergonomic Rubber Pants
11:00:04.446 INFO  [           main] c.v.common.DefaultSubscriber   :  received complete!

Process finished with exit code 0
```

- Si no existe → se ejecuta la notificación y se emite `onComplete()` sin valor.

```bash
11:00:57.356 INFO  [           main] c.v.s.Lec07MonoFromRunnable    : notifying business on unavailable product 2
11:00:57.367 INFO  [           main] c.v.common.DefaultSubscriber   :  received complete!

Process finished with exit code 0
```

---

### ✅ Conclusión

- `Mono.fromRunnable()` es útil cuando necesitas **ejecutar una acción sin devolver datos**, pero **solo si hay suscriptores**.
- Es una forma elegante de **combinar lógica de negocio con flujo reactivo**, sin desperdiciar recursos.

---