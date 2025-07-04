
---
### 🎯 Objetivo

Aprender a usar `Mono.just()`, `Mono.empty()` y `Mono.error()` para representar diferentes escenarios en programación reactiva:

---

### 🧪 Ejemplo: Método `getUsername(int userId)`

Este método simula una búsqueda de nombre de usuario por ID:

```java
private static Mono<String> getUsername(int userId){  
    return switch (userId){  
        case 1 -> Mono.just("sam");  // Valor válido
        case 2 -> Mono.empty(); // No hay datos
        default -> Mono.error(new RuntimeException("invalid input"));  // Error
    };  
}
```

---

### 📌 Comportamiento esperado

Para la prueba utilizamos el [[Subscriber Genérico]]

| Entrada (`userId`) | Resultado del `Mono` | Evento emitido                  |
| ------------------ | -------------------- | ------------------------------- |
| `1`                | `"Sam"`              | `onNext("Sam")`, `onComplete()` |

```java
public static void main(String[] args) {  
     getUsername(1)  
            .subscribe(Util.subscriber());  
 }
```

```bash
17:58:19.110 INFO  [           main] c.v.common.DefaultSubscriber   :  received: sam
17:58:19.123 INFO  [           main] c.v.common.DefaultSubscriber   :  received complete!
Process finished with exit code 0
```

| Entrada (`userId`) | Resultado del `Mono` | Evento emitido      |
| ------------------ | -------------------- | ------------------- |
| `2`                | Sin valor            | Solo `onComplete()` |

```java
public static void main(String[] args) {  
     getUsername(1)  
            .subscribe(Util.subscriber());  
 }
```

```bash
18:02:19.544 INFO  [           main] c.v.common.DefaultSubscriber   :  received complete!

Process finished with exit code 0
```

| Entrada (`userId`) | Resultado del `Mono` | Evento emitido |
| ------------------ | -------------------- | -------------- |
| `3` o más          | Error                | `onError()`    |
|                    |                      |                |

```java
public static void main(String[] args) {  
     getUsername(3)  
            .subscribe(Util.subscriber());  
 }
```

```bash
18:04:06.821 ERROR [           main] c.v.common.DefaultSubscriber   :  error
java.lang.RuntimeException: invalid input
	at com.vinsguru.sec02.Lec04MonoEmptyError.getUsername(Lec04MonoEmptyError.java:23)
	at com.vinsguru.sec02.Lec04MonoEmptyError.main(Lec04MonoEmptyError.java:14)

Process finished with exit code 0
```

---

### 🧠 Conceptos clave

- **`Mono.just(valor)`**: Emite un único valor y luego `onComplete()`.
- **`Mono.empty()`**: No emite ningún valor, solo `onComplete()`.
- **`Mono.error(Throwable)`**: Emite un error directamente con `onError()`.

> En programación reactiva **no se usa `null`**. En su lugar, se usa `Mono.empty()` para representar ausencia de datos.

---

### ✅ Conclusión

Este patrón es útil para representar:

- Datos disponibles (`Mono.just`)
- Datos no encontrados (`Mono.empty`)
- Errores de entrada o sistema (`Mono.error`)

---