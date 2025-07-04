
---
### ⚠️ Problema: `Operator called default onErrorDropped`

Este mensaje aparece cuando:

- Te suscribes a un `Mono` o `Flux`.
- Solo proporcionas un **manejador de datos** (`onNext`).
- El publisher **emite un error** (`onError`).
- Pero **no proporcionaste un manejador de errores**.

---

### 🧪 Ejemplo del problema

```bash
Mono.error(new RuntimeException("Fallo"))
    .subscribe(value -> System.out.println("Recibido: " + value));
```

- Resultado: aparece el mensaje de advertencia `default onErrorDropped`.

```bash
18:32:18.051 ERROR [           main] r.core.publisher.Operators     : Operator called default onErrorDropped
reactor.core.Exceptions$ErrorCallbackNotImplemented: java.lang.RuntimeException: Fallo
Caused by: java.lang.RuntimeException: Fallo
	at com.vinsguru.sec02.Lec04MonoEmptyError.main(Lec04MonoEmptyError.java:15)

Process finished with exit code 0
```

---
### ✅ Solución

Proporcionar un manejador de errores al suscribirse:

```java
Mono.error(new RuntimeException("Fallo"))
    .subscribe(
        value -> System.out.println("Recibido: " + value),
        error -> System.out.println("Error: " + error.getMessage())
    );
```

- Ahora Reactor sabe qué hacer con el error y **no lanza la advertencia**.

```bash
Error: Fallo
Process finished with exit code 0
```

---
### 🧠 Conclusión

- Siempre que exista la posibilidad de que un `Mono` o `Flux` emita un error, **debes proporcionar un manejador de errores**.
- Si no lo haces, Reactor lo notificará con un mensaje como `onErrorDropped`.

---