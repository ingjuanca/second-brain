
---
### 🧠 Contexto

En programación reactiva, es fundamental ser **perezoso (lazy)**:

> **No ejecutar nada hasta que sea necesario**, es decir, hasta que alguien se suscriba.

---

### 🧪 Problema con `Mono.just()`

```java
public static void main(String[] args) {
	var list = List.of(1, 2, 3);
	Mono<Integer> mono = Mono.just(sum(list));
}
private static int sum(List<Integer> list) {  
    log.info("finding the sum of {}", list);  
    return list.stream().mapToInt(a -> a).sum();  
}
```

```bash
09:59:03.653 INFO  [           main] c.v.s.Lec05MonoFromSupplier    : finding the sum of [1, 2, 3]

Process finished with exit code 0

```

- Aunque no haya suscriptores, el método `sum()` **se ejecuta inmediatamente**.
- Esto **desperdicia recursos**, especialmente si la operación es costosa (por ejemplo, cálculos intensivos).

---

### ✅ Solución: `Mono.fromSupplier()`

```java
public static void main(String[] args) {
	var list = List.of(1, 2, 3);
	Mono<Integer> mono = Mono.fromSupplier(() -> sum(list));
}
private static int sum(List<Integer> list) {  
    log.info("finding the sum of {}", list);  
    return list.stream().mapToInt(a -> a).sum();  
}
```

```bash

Process finished with exit code 0

```

- La ejecución de `sum(list)` **se retrasa** hasta que alguien se suscriba.

```java
public static void main(String[] args) {
	var list = List.of(1, 2, 3);
	Mono<Integer> mono = Mono.fromSupplier(() -> sum(list))  
        .subscribe(Util.subscriber());
}
private static int sum(List<Integer> list) {  
    log.info("finding the sum of {}", list);  
    return list.stream().mapToInt(a -> a).sum();  
}
```

```bash
09:57:59.806 INFO  [           main] c.v.s.Lec05MonoFromSupplier    : finding the sum of [1, 2, 3]
09:57:59.812 INFO  [           main] c.v.common.DefaultSubscriber   :  received: 6
09:57:59.820 INFO  [           main] c.v.common.DefaultSubscriber   :  received complete!

Process finished with exit code 0
```
- Esto permite un uso más eficiente de los recursos.

---

### 📌 ¿Cuándo usar cada uno?

|Método|Uso recomendado|
|---|---|
|`Mono.just(valor)`|Cuando ya tienes el valor **en memoria**|
|`Mono.fromSupplier()`|Cuando necesitas **calcular o generar** el valor bajo demanda|

---

### 🧠 Conclusión

- Usa `Mono.fromSupplier()` para **diferir operaciones costosas** hasta que realmente se necesiten.
- Esto sigue el principio reactivo de **evaluación perezosa**, mejorando el rendimiento y evitando trabajo innecesario.