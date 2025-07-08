
---
### 🎯 ¿Qué es `Mono.defer()`?

Es un método que permite **retrasar la creación del publisher** hasta que alguien se suscriba.  
Esto es útil cuando **incluso la creación del `Mono` es costosa** o involucra lógica que no debería ejecutarse innecesariamente.

---

### 🧠 Contexto

Hasta ahora se ha dicho que:

- Crear un `Mono` es una operación **ligera**.
- La lógica costosa debe ir dentro de un `Supplier`, `Callable`, etc., y se ejecuta solo al suscribirse.

Pero… ¿y si **crear el `Mono` en sí mismo** también es costoso?

---

### 🧪 Ejemplo

```java
// time-consuming publisher creation  
private static Mono<Integer> createPublisher(){  
    log.info("creating publisher");  
    var list = List.of(1, 2, 3);  
    Util.sleepSeconds(1);  
    return Mono.fromSupplier(() -> sum(list));  
}  
  
// time-consuming business logic  
private static int sum(List<Integer> list) {  
    log.info("finding the sum of {}", list);  
    Util.sleepSeconds(3);  
    return list.stream().mapToInt(a -> a).sum();  
}

public static void main(String[] args) {  
     createPublisher()  
            .subscribe(Util.subscriber());   
}
```

- Si llamas a `createPublisher()` directamente, **se ejecuta inmediatamente**, incluso sin suscriptores.

```java
public static void main(String[] args) {    
    createPublisher();    
}
```

```bash
17:39:38.829 INFO  [           main] c.v.sec02.Lec10MonoDefer       : creating publisher

Process finished with exit code 0

```

---

### ✅ Solución: `Mono.defer()`

```java
public static void main(String[] args) {   
	Mono.defer(() -> createPublisher());
}
```

```bash

Process finished with exit code 0
```

- Ahora, **ni siquiera se crea el `Mono`** hasta que alguien se suscriba.

- Se retrasa tanto la **creación del publisher** como su **ejecución**.


```java
public static void main(String[] args) {   
	Mono.defer(() -> createPublisher())
	    .subscribe(Util.subscriber());
}
```

```bash
17:43:58.313 INFO  [           main] c.v.sec02.Lec10MonoDefer       : creating publisher
17:43:59.334 INFO  [           main] c.v.sec02.Lec10MonoDefer       : finding the sum of [1, 2, 3]
17:44:02.341 INFO  [           main] c.v.common.DefaultSubscriber   :  received: 6
17:44:02.344 INFO  [           main] c.v.common.DefaultSubscriber   :  received complete!

Process finished with exit code 0
```

---

### 📌 ¿Cuándo usar `Mono.defer()`?

|Situación|¿Usar `Mono.defer()`?|
|---|---|
|El `Mono` se crea con datos ya disponibles|❌ No es necesario|
|El `Mono` se crea con lógica costosa (consultas, cálculos, etc.)|✅ Sí|
|Quieres evitar cualquier ejecución hasta que haya un suscriptor|✅ Sí|

---

### ✅ Conclusión

- `Mono.defer()` es útil cuando **incluso la creación del `Mono` debe ser perezosa**.
- Asegura que **nada se ejecute** hasta que alguien se suscriba.
- Es una herramienta avanzada para mantener el flujo reactivo **eficiente y controlado**.

---