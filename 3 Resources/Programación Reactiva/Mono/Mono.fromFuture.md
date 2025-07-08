
---

`Mono.fromFuture()` se utiliza en **programación reactiva con Project Reactor** para **convertir un [[CompletableFuture]] en un `Mono`**, lo que permite integrar código asincrónico tradicional de Java dentro de un flujo reactivo.

---

### 🧠 ¿Qué hace `Mono.fromFuture()`?

Convierte un objeto `CompletableFuture<T>` en un `Mono<T>`, permitiendo que su resultado (o error) se maneje dentro del ecosistema reactivo.

---

### ✅ ¿Cuándo usarlo?

1. **Integración con APIs existentes**
    
    - Si estás usando una biblioteca que devuelve `CompletableFuture` (como llamadas HTTP, acceso a base de datos, etc.), puedes convertirlo en `Mono` para seguir trabajando de forma reactiva.
2. **Reutilizar lógica asincrónica**
    
    - Si ya tienes lógica basada en `CompletableFuture`, puedes integrarla sin reescribirla.
3. **Encadenar con operadores reactivos**
    
    - Una vez convertido en `Mono`, puedes aplicar operadores como `.map()`, `.flatMap()`, `.filter()`, etc.

---
### 🎯 Objetivo

**`Mono.fromFuture`** permite ejecutar tareas en segundo plano.

- Permite ejecutar código de forma asíncrona

Aprender a convertir un `CompletableFuture<T>` en un `Mono<T>` usando:

- `Mono.fromFuture(future)`
- `Mono.fromFuture(Supplier<CompletableFuture<T>>)` (forma perezosa)


---

### 🧪 Ejemplo básico

```java
    public static void main(String[] args) {  
  
        Mono.fromFuture(getName())  //No perezoso
                .subscribe(Util.subscriber());  
        Util.sleepSeconds(1);  
    }  
  
    private static CompletableFuture<String> getName(){  
        return CompletableFuture.supplyAsync(() -> {  
            log.info("generating name");  
            return Util.faker().name().firstName();  
        });  
    }    
```

```bash
12:50:16.343 INFO  [onPool-worker-1] c.v.sec02.Lec08MonoFromFuture  : generating name
12:50:17.019 INFO  [onPool-worker-1] c.v.common.DefaultSubscriber   :  received: Clifford
12:50:17.071 INFO  [onPool-worker-1] c.v.common.DefaultSubscriber   :  received complete!

Process finished with exit code 0
```

- Esto funciona, pero tiene un problema: **el `CompletableFuture` se ejecuta inmediatamente**, incluso si nadie se suscribe.

```java
   public static void main(String[] args) {  
  
        Mono.fromFuture(getName()); //Nadie se suscribe
  
        Util.sleepSeconds(1);  
    }  
  
    private static CompletableFuture<String> getName(){  
        return CompletableFuture.supplyAsync(() -> {  
            log.info("generating name");  
            return Util.faker().name().firstName();  
        });  
    }  
```

```bash
12:33:36.575 INFO  [onPool-worker-1] c.v.sec02.Lec08MonoFromFuture  : generating name

Process finished with exit code 0

```
---

### ⚠️ Problema: falta de pereza

- `CompletableFuture` **no es perezoso**.
- Se ejecuta tan pronto como se crea, lo cual **viola el principio reactivo** de ejecutar solo cuando es necesario.

---

### ✅ Solución: usar un `Supplier<CompletableFuture>`

```java
public class Lec08MonoFromFuture {  
  
    private static final Logger log = LoggerFactory.getLogger(Lec08MonoFromFuture.class);  
  
    public static void main(String[] args) {  
  
        Mono.fromFuture(() -> getName())  
                .subscribe(Util.subscriber());  
  
        Util.sleepSeconds(1);  
    }  
  
    private static CompletableFuture<String> getName(){  
        return CompletableFuture.supplyAsync(() -> {  
            log.info("generating name");  
            return Util.faker().name().firstName();  
        });  
    }    
}
```

```bash
12:50:16.343 INFO  [onPool-worker-1] c.v.sec02.Lec08MonoFromFuture  : generating name
12:50:17.019 INFO  [onPool-worker-1] c.v.common.DefaultSubscriber   :  received: Clifford
12:50:17.071 INFO  [onPool-worker-1] c.v.common.DefaultSubscriber   :  received complete!

Process finished with exit code 0
```

- Ahora, la ejecución **solo ocurre cuando alguien se suscribe**.

```java
public class Lec08MonoFromFuture {  
  
    private static final Logger log = LoggerFactory.getLogger(Lec08MonoFromFuture.class);  
  
    public static void main(String[] args) {  
  
        Mono.fromFuture(() -> getName()); //Nadie se suscribe
  
        Util.sleepSeconds(1);  
    }  
  
    private static CompletableFuture<String> getName(){  
        return CompletableFuture.supplyAsync(() -> {  
            log.info("generating name");  
            return Util.faker().name().firstName();  
        });  
    }    
}
```

```bash

Process finished with exit code 0
```

- Esto respeta el modelo reactivo.

---

### 🧩 Extra: esperar para ver el resultado

- Como `CompletableFuture` se ejecuta en un hilo separado, el hilo principal puede terminar antes de que se vea el resultado.
- Se recomienda usar una utilidad como:

```java
Util.sleepSeconds(1);
```

Para **bloquear el hilo principal** y permitir que el resultado se imprima.

---

### ✅ Conclusión

|Enfoque|¿Perezoso?|¿Recomendado?|
|---|---|---|
|`Mono.fromFuture(future)`|❌ No|⚠️ Solo si ya tienes el `CompletableFuture` listo|
|`Mono.fromFuture(() -> future)`|✅ Sí|✅ Ideal para mantener el flujo reactivo|

---