
---
### ❓ Pregunta frecuente

> “Si `Mono.fromSupplier()` retrasa la ejecución, ¿por qué al llamar al método que lo contiene se imprime un mensaje inmediatamente?”

---

### 🧠 Aclaración clave

Hay que **distinguir entre dos cosas**:

|Concepto|¿Cuándo ocurre?|¿Qué implica?|
|---|---|---|
|**Creación del publisher**|Al llamar al método que devuelve el `Mono`|Es una operación **ligera** y **rápida**|
|**Ejecución del publisher**|Solo cuando alguien se **suscribe**|Aquí ocurre la **lógica de negocio real**|

---

### 🧪 Ejemplo explicado

```java
Mono<String> getName() {
    System.out.println("Entrando al método");
    return Mono.fromSupplier(() -> {
        // lógica costosa
        log.info("generating name");
        Util.sleepSeconds(3);  
		return Util.faker().name().firstName();
    });
}
```

- Al llamar a `getName()`, se imprime `"Entrando al método"` → esto es parte de la **creación del publisher**.

```java
public static void main(String[] args) {  
     getName();  
}
```

```bash
17:18:54.102 INFO  [           main] ec09PublisherCreateVsExecution : entered the method

Process finished with exit code 0
```

- Pero la lógica dentro del `Supplier` (como `Thread.sleep`) **no se ejecuta** hasta que alguien se **suscriba**.

```java 
public static void main(String[] args) {  
     getName().subscribe(Util.subscriber());    
}
```

```bash 
17:21:10.542 INFO  [           main] ec09PublisherCreateVsExecution : entered the method
17:21:10.759 INFO  [           main] ec09PublisherCreateVsExecution : generating name
17:21:13.911 INFO  [           main] c.v.common.DefaultSubscriber   :  received: Pamella
17:21:13.921 INFO  [           main] c.v.common.DefaultSubscriber   :  received complete!

Process finished with exit code 0
```
---

### ✅ Conclusión

- **Crear un `Mono` no significa ejecutarlo**.
- La **ejecución real** (como cálculos, llamadas a servicios, etc.) **se retrasa** hasta que se llama a `.subscribe()`.
- Esto es lo que significa que `Mono.fromSupplier()` es **perezoso (lazy)**.

---