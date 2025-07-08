
---

A continuación se detalla el proceso del uso de Publisher de tipo Mono utilizando servicios externos:

### 🛠️ **Configuración Inicial**

- Se proporciona un archivo `.jar` que ejecuta una aplicación Spring Boot en el puerto **7070**.

	https://github.com/vinsguru/java-reactive-programming-course/blob/master/02-external-services/external-services-instructions.md

- Al acceder a `localhost:7070`, se abre una interfaz Swagger con varios endpoints.
- El endpoint de interés es: `/demo01/product/{id}`, que devuelve el nombre del producto correspondiente al ID proporcionado (de 1 a 100).
- Cada solicitud tarda **1 segundo** en responder, intencionalmente, para simular latencia real.

---
### 🧱 **Creación del Cliente HTTP**

- Se crea una clase abstracta `AbstractHttpClient` que configura un cliente HTTP usando **Reactor Netty**.
- Se personaliza el uso de **recursos de bucle de eventos (loop resources)** para usar **solo un hilo**, demostrando que un solo hilo puede manejar múltiples solicitudes concurrentes.
- Se define una clase `ExternalServiceClient` que extiende la clase abstracta y expone un método `getProductName(productId)` que devuelve un `Mono<String>` con el nombre del producto.

```java
public class ExternalServiceClient extends AbstractHttpClient {  
    public Mono<String> getProductName(int productId) {  
        return this.httpClient.get()  
                .uri("/demo01/product/" + productId)  
                .responseContent()  
                .asString()  
                .next();  
    }  
}
```

### ⚙️ **Ejecución de Solicitudes**

- En una clase principal (`Lecture11NonBlockingIO`), se crean múltiples solicitudes HTTP usando un bucle `for`.
- Se demuestra que:
    - Las solicitudes se envían casi simultáneamente.
    - Las respuestas pueden llegar en cualquier orden (no garantizado).
    - Todas las respuestas se manejan con **un solo hilo**.
    - Se pueden manejar **100 solicitudes concurrentes** en **menos de 2 segundos**.

```java
/*  
    To demo non-blocking IO    
    Ensure that the external service is up and running! 
*/
public class Lec11NonBlockingIO {  
  
    private static final Logger log = LoggerFactory.getLogger(Lec11NonBlockingIO.class);  
  
    public static void main(String[] args) {    
        var client = new ExternalServiceClient();    
        log.info("starting");  
        for (int i = 1; i <= 100; i++) {  
            client.getProductName(i)  
                    .subscribe(Util.subscriber());  
        }    
        Util.sleepSeconds(2);  
    }    
}
```

---

### 💡 **Conclusiones**

- La programación no bloqueante permite manejar múltiples solicitudes sin necesidad de múltiples hilos o largos tiempos de espera.
- Reactor Netty es más bajo nivel que Spring WebFlux, pero permite entender mejor cómo funciona la programación reactiva.
- En producción, se usaría Spring WebFlux para simplificar la serialización, deserialización y manejo de respuestas.

---