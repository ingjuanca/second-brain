
---

### 🎯 Objetivo

Aprender cuándo usar `Mono.fromSupplier()` y cuándo usar `Mono.fromCallable()` para crear publishers que **ejecutan código de forma diferida (lazy)**.

---

### 🧪 ¿Qué hacen?

Ambos métodos permiten crear un `Mono` que **ejecuta una operación solo cuando alguien se suscribe**:

- `Mono.fromSupplier(() -> valor)`
- `Mono.fromCallable(() -> valor)`

Ambos retrasan la ejecución, pero hay una **diferencia clave**.

---

### 🔍 Diferencia entre `Supplier` y `Callable`

|Característica|`Supplier<T>`|`Callable<T>`|
|---|---|---|
|Introducido en|Java 8|Java 1.5|
|Método principal|`T get()`|`T call() throws Exception`|
|¿Lanza excepciones checked?|❌ No (solo unchecked)|✅ Sí (puede lanzar checked)|
|Uso común|Funciones simples sin errores|Tareas que pueden fallar (ej. I/O)|

---

### 🧪 Ejemplo práctico

Si tienes un método que **puede lanzar una excepción checked**:

```java
public static int sum(List<Integer> list) throws Exception {
    // lógica que puede fallar
	log.info("finding the sum of {}", list);  
    return list.stream().mapToInt(a -> a).sum();  
}
```

- ✅ `Mono.fromCallable(() -> sum())` → funciona sin problema.

```java
public static void main(String[] args) {
	var list = List.of(1, 2, 3);
	Mono<Integer> mono = Mono.fromCallable(() -> sum(list))
		.subscribe(Util.subscriber());
}
```

- ❌ `Mono.fromSupplier(() -> sum())` → Java obliga a manejar la excepción con `try-catch`.

---

### ✅ Conclusión

- Usa **`Mono.fromSupplier()`** cuando:
    
    - No hay excepciones checked.
    - Solo necesitas lógica simple y segura.
- Usa **`Mono.fromCallable()`** cuando:
    
    - El método puede lanzar excepciones checked.
    - Quieres evitar `try-catch` innecesarios.

---