
---
### 🎯 Objetivo

Aunque en la práctica se usa **Project Reactor** (una implementación lista de Reactive Streams), el propósito de esta sección es **aprender desde cero** cómo funciona el modelo publicador-suscriptor implementándolo manualmente.

---
### 🧪 Caso de Uso Simulado

- **Publisher**: contiene direcciones de correo electrónico de clientes.
- **Subscriber**: quiere recibir esos correos para enviar promociones (solo se imprimen en consola, no se envían correos reales).

---
### 👤 Implementación del Subscriber

1. Crea una clase que implemente `Subscriber<String>`.
2. Define los métodos:
    - `onNext(String email)`: imprime el correo recibido.
    - `onError(Throwable t)`: imprime el error.
    - `onComplete()`: imprime "completado".
    - `onSubscribe(Subscription s)`: guarda el objeto `Subscription`.
3. Agrega un **logger** para seguimiento.
4. Expón el objeto `Subscription` con un getter para poder hacer solicitudes desde una clase de prueba.


```java
import org.reactivestreams.Subscriber;  
import org.reactivestreams.Subscription;  
import org.slf4j.Logger;  
import org.slf4j.LoggerFactory;  
  
public class SubscriberImpl implements Subscriber<String> {  
  
    private static final Logger log = LoggerFactory.getLogger(SubscriberImpl.class);  
    private Subscription subscription;  
  
    public Subscription getSubscription() {  
        return subscription;  
    }  
  
    @Override  
    public void onSubscribe(Subscription subscription) {  
        this.subscription = subscription;  
    }  
  
    @Override  
    public void onNext(String email) {  
        log.info("received: {}", email);  
    }  
  
    @Override  
    public void onError(Throwable throwable) {  
        log.error("error", throwable);  
    }  
  
    @Override  
    public void onComplete() {  
        log.info("completed!");  
    }  
}
```


---
### 📤 Implementación del Publisher

1. Crea una clase que implemente `Publisher<String>`.
2. En el método `subscribe(Subscriber<? super String> s)`:
    - Crea una instancia de `SubscriptionImpl`.
    - Pasa el `Subscription` al `Subscriber`.

```java
import org.reactivestreams.Publisher;  
import org.reactivestreams.Subscriber;  
  
public class PublisherImpl implements Publisher<String> {  
  
    @Override  
    public void subscribe(Subscriber<? super String> subscriber) {  
        var subscription = new SubscriptionImpl(subscriber);  
        subscriber.onSubscribe(subscription);  
    }    
}
```

---
### 🔗 Implementación del Subscription

1. Crea una clase que implemente `Subscription`.
2. Guarda una referencia al `Subscriber`.
3. Implementa los métodos:
    - `request(long n)`: inicia la producción de datos.
    - `cancel()`: detiene la producción.
4. Se usa **Java Faker** para generar correos electrónicos aleatorios.
5. Se simula una base de datos con un máximo de 10 ítems (`MAX_ITEMS = 10`).
6. Se controla el estado con variables como `isCanceled` y `count`.
7. Agrega un **logger** para seguimiento.

```java 
import com.github.javafaker.Faker;  
import org.reactivestreams.Subscriber;  
import org.reactivestreams.Subscription;  
import org.slf4j.Logger;  
import org.slf4j.LoggerFactory;  
  
public class SubscriptionImpl implements Subscription {  
  
    private static final Logger log = LoggerFactory.getLogger(SubscriptionImpl.class);  
    private static final int MAX_ITEMS = 10;  
    private final Faker faker;  
    private final Subscriber<? super String> subscriber;  
    private boolean isCancelled;  
    private int count = 0;  
  
    public SubscriptionImpl(Subscriber<? super String> subscriber){  
        this.subscriber = subscriber;  
        this.faker = Faker.instance();  
    }  
  
    @Override  
    public void request(long requested) {  
        if(isCancelled){  
            return;  
        }  
        log.info("subscriber has requested {} items", requested);  
        if(requested > MAX_ITEMS){  
            this.subscriber.onError(new RuntimeException("validation failed"));  
            this.isCancelled = true;  
            return;  
        }  
        for (int i = 0; i < requested && count < MAX_ITEMS; i++) {  
            count++;  
            this.subscriber.onNext(this.faker.internet().emailAddress());  
        }  
        if(count == MAX_ITEMS){  
            log.info("no more data to produce");  
            this.subscriber.onComplete();  
            this.isCancelled = true;  
        }  
    }  
  
    @Override  
    public void cancel() {  
        log.info("subscriber has cancelled");  
        this.isCancelled = true;  
    }  
  
}
```

---
### 🔄 Flujo de Comunicación

1. El **Subscriber** se suscribe al **Publisher**.
2. El **Publisher** entrega un objeto `Subscription`.
3. El **Subscriber** solicita datos con `request(n)`.
4. El **Publisher** responde con `onNext()` hasta alcanzar el número solicitado o el límite.
5. Si no hay más datos, se llama a `onComplete()`.
6. Si ocurre un error, se llama a `onError()`.  

---
### 🧪 Casos de Prueba (Demo)

#### ✅ **Demo 1**: Sin solicitud → No se produce nada

- Se valida que el `Publisher` no emite datos hasta que se le solicite.

```java
import com.vinsguru.sec01.publisher.PublisherImpl;  
import com.vinsguru.sec01.subscriber.SubscriberImpl;  
  
import java.time.Duration;  
  
public class Demo {    
    public static void main(String[] args) throws InterruptedException {  
	    /* 
		    1. publisher does not produce data unless 
			subscriber requests for it. 
	    */
        var publisher = new PublisherImpl();  
        var subscriber = new SubscriberImpl();  
        publisher.subscribe(subscriber);  
    }
}
```

```bash
Process finished with exit code 0
```

#### ✅ **Demo 2**: Solicitudes en lotes

- Se hacen varias solicitudes de 3 ítems cada una.
- El `Publisher` produce hasta un máximo de 10 ítems.
- Se valida que no se excede ese límite.

```java
import com.vinsguru.sec01.publisher.PublisherImpl;  
import com.vinsguru.sec01.subscriber.SubscriberImpl;  
  
import java.time.Duration;  
  
public class Demo {    
    public static void main(String[] args) throws InterruptedException {
	    /* 
		    2. publisher will produce only <= subscriber requested items. 
		    publisher can also produce 0 items! 
	    */  
        var publisher = new PublisherImpl();  
		var subscriber = new SubscriberImpl();  
		publisher.subscribe(subscriber);  
		subscriber.getSubscription().request(3);  
		Thread.sleep(Duration.ofSeconds(2));  
		subscriber.getSubscription().request(3);  
		Thread.sleep(Duration.ofSeconds(2));  
		subscriber.getSubscription().request(3);  
		Thread.sleep(Duration.ofSeconds(2));  
		subscriber.getSubscription().request(3);  
		Thread.sleep(Duration.ofSeconds(2));  
		subscriber.getSubscription().request(3);
    }
}
```


```bash
19:12:36.602 INFO  [           main] c.v.s.p.SubscriptionImpl       : subscriber has requested 3 items
19:12:37.004 INFO  [           main] c.v.s.s.SubscriberImpl         : received: adolph.gerlach@hotmail.com
19:12:37.006 INFO  [           main] c.v.s.s.SubscriberImpl         : received: twanda.kuhic@hotmail.com
19:12:37.007 INFO  [           main] c.v.s.s.SubscriberImpl         : received: maryetta.gislason@hotmail.com
19:12:39.008 INFO  [           main] c.v.s.p.SubscriptionImpl       : subscriber has requested 3 items
19:12:39.008 INFO  [           main] c.v.s.s.SubscriberImpl         : received: hung.dooley@gmail.com
19:12:39.010 INFO  [           main] c.v.s.s.SubscriberImpl         : received: keven.pfannerstill@hotmail.com
19:12:39.010 INFO  [           main] c.v.s.s.SubscriberImpl         : received: cameron.hirthe@yahoo.com
19:12:41.016 INFO  [           main] c.v.s.p.SubscriptionImpl       : subscriber has requested 3 items
19:12:41.016 INFO  [           main] c.v.s.s.SubscriberImpl         : received: quentin.robel@gmail.com
19:12:41.016 INFO  [           main] c.v.s.s.SubscriberImpl         : received: lyndon.blick@hotmail.com
19:12:41.017 INFO  [           main] c.v.s.s.SubscriberImpl         : received: tyler.volkman@hotmail.com
19:12:43.022 INFO  [           main] c.v.s.p.SubscriptionImpl       : subscriber has requested 3 items
19:12:43.022 INFO  [           main] c.v.s.s.SubscriberImpl         : received: sonny.morar@yahoo.com
19:12:43.022 INFO  [           main] c.v.s.p.SubscriptionImpl       : no more data to produce
19:12:43.022 INFO  [           main] c.v.s.s.SubscriberImpl         : completed!
```

#### ✅ **Demo 3**: Cancelación de la suscripción

- Se solicita 3 ítems, luego se cancela.
- Se intenta solicitar más, pero no se produce nada.

```java
import com.vinsguru.sec01.publisher.PublisherImpl;  
import com.vinsguru.sec01.subscriber.SubscriberImpl;  
  
import java.time.Duration;  
  
public class Demo {    
    public static void main(String[] args) throws InterruptedException {  
	    /* 
		    3. subscriber can cancel the subscription. 
		    producer should stop at that moment as subscriber 
		    is no longer interested in consuming the data
	    */
        var publisher = new PublisherImpl();  
		var subscriber = new SubscriberImpl();  
		publisher.subscribe(subscriber);  
		subscriber.getSubscription().request(3);  
		Thread.sleep(Duration.ofSeconds(2));  
		subscriber.getSubscription().cancel();  
		subscriber.getSubscription().request(3);  
		Thread.sleep(Duration.ofSeconds(2));
    }
}
```

```bash
19:14:35.053 INFO  [           main] c.v.s.p.SubscriptionImpl       : subscriber has requested 3 items
19:14:35.462 INFO  [           main] c.v.s.s.SubscriberImpl         : received: cody.jacobson@yahoo.com
19:14:35.464 INFO  [           main] c.v.s.s.SubscriberImpl         : received: gaylord.kozey@yahoo.com
19:14:35.464 INFO  [           main] c.v.s.s.SubscriberImpl         : received: luke.heathcote@gmail.com
19:14:37.471 INFO  [           main] c.v.s.p.SubscriptionImpl       : subscriber has cancelled

Process finished with exit code 0
```
#### ✅ **Demo 4**: Manejo de errores

- Se solicita más de 10 ítems en una sola petición.
- Se lanza una excepción (`onError()`).
- Se valida que no se emiten más datos después del error.

```java
import com.vinsguru.sec01.publisher.PublisherImpl;  
import com.vinsguru.sec01.subscriber.SubscriberImpl;  
  
import java.time.Duration;  
  
public class Demo {    
    public static void main(String[] args) throws InterruptedException {  
	    /* 
		    4. producer can send the error signal
	    */
        var publisher = new PublisherImpl();  
		var subscriber = new SubscriberImpl();  
		publisher.subscribe(subscriber);  
		subscriber.getSubscription().request(3);  
		Thread.sleep(Duration.ofSeconds(2));  
		subscriber.getSubscription().request(11);  
		Thread.sleep(Duration.ofSeconds(2));  
		subscriber.getSubscription().request(3);  
		Thread.sleep(Duration.ofSeconds(2));
    }
}

```

```bash
19:16:06.088 INFO  [           main] c.v.s.p.SubscriptionImpl       : subscriber has requested 3 items
19:16:06.599 INFO  [           main] c.v.s.s.SubscriberImpl         : received: quintin.hodkiewicz@hotmail.com
19:16:06.601 INFO  [           main] c.v.s.s.SubscriberImpl         : received: kristal.tillman@hotmail.com
19:16:06.603 INFO  [           main] c.v.s.s.SubscriberImpl         : received: andy.skiles@yahoo.com
19:16:08.613 INFO  [           main] c.v.s.p.SubscriptionImpl       : subscriber has requested 11 items
19:16:08.613 ERROR [           main] c.v.s.s.SubscriberImpl         : error
java.lang.RuntimeException: validation failed
	at com.vinsguru.sec01.publisher.SubscriptionImpl.request(SubscriptionImpl.java:30)
	at com.vinsguru.sec01.Demo.demo4(Demo.java:61)
	at com.vinsguru.sec01.Demo.main(Demo.java:19)

Process finished with exit code 0
```

---
### ✅ Conclusión

Este ejercicio permite entender cómo funciona internamente el modelo de programación reactiva, antes de usar herramientas como **Reactor**. En la siguiente lección se completará la lógica de producción de datos.

---