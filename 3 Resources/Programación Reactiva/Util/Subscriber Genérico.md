
---
### 🎯 Objetivo

Evitar escribir repetidamente la lógica de suscripción en cada demo. Para ello, se crea una clase genérica llamada `DefaultSubscriber<T>` que:

- Implementa `Subscriber<T>`
- Puede manejar cualquier tipo de dato (`String`, `Integer`, etc.)
- Imprime los eventos recibidos (`onNext`, `onComplete`, `onError`)
- Permite identificar múltiples suscriptores mediante un **nombre personalizado**

---

### 🧱 Estructura del Proyecto

- **Clases creadas**:
    - `DefaultSubscriber<T>`: implementación genérica de `Subscriber`
    - `Util`: clase utilitaria para crear instancias de `DefaultSubscriber`

---

### 🧪 ¿Qué hace `DefaultSubscriber<T>`?

```java
public class DefaultSubscriber<T> implements Subscriber<T> {  
  
    private static final Logger log = LoggerFactory.getLogger(DefaultSubscriber.class);  
    private final String name;  
  
    public DefaultSubscriber(String name) {  
        this.name = name;  
    }  
  
    @Override  
    public void onSubscribe(Subscription subscription) {  
        subscription.request(Long.MAX_VALUE);  
    }  
  
    @Override  
    public void onNext(T item) {  
        log.info("{} received: {}", this.name, item);  
    }  
  
    @Override  
    public void onError(Throwable throwable) {  
        log.error("{} error", this.name, throwable);  
    }  
  
    @Override  
    public void onComplete() {  
        log.info("{} received complete!", this.name);  
    }  
  
}
```

- Se suscribe automáticamente con `request(Long.MAX_VALUE)`
- Imprime mensajes como:
    - `"subscriber1 received: valor"`
    - `"subscriber1 received complete"`
- Permite diferenciar entre múltiples suscriptores conectados al mismo `Publisher`

---

### 🛠️ Clase `Util`

Contiene métodos estáticos para crear suscriptores:

```java 
public class Util {

	public static <T> Subscriber<T> subscriber() {
	    return new DefaultSubscriber<>("");
	}

	public static <T> Subscriber<T> subscriber(String name) {
	    return new DefaultSubscriber<>(name);
	}
}
```

---

### 🧪 Ejemplo de uso

```java
public static void main(String[] args) {
	Mono.just("valor")
	    .subscribe(Util.subscriber("subscriber1"));
	
	Mono.just("otro valor")
	    .subscribe(Util.subscriber("subscriber2"));
}
```

Resultado esperado en consola:

```bash
subscriber1 received: valor
subscriber1 received complete
subscriber2 received: otro valor
subscriber2 received complete
```

---

### ✅ Conclusión

Esta clase `DefaultSubscriber`:

- Mejora la productividad en las demos.
- Permite observar claramente el comportamiento de múltiples suscriptores.
- Será reutilizada en futuras secciones del curso.

---