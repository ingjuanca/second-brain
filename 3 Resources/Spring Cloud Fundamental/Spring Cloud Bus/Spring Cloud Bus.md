
---

### Introducción

Antes de usar **Spring Cloud Bus**, se analiza el problema con la configuración de microservicios sin usarlo:

- Los microservicios leen configuraciones desde un **Config Server**.
    
- Si cambia un valor en los archivos de configuración, ese cambio **no se refleja automáticamente** en las instancias en ejecución.
    
- Para actualizarlo, es necesario **reiniciar** el microservicio o usar el endpoint `/actuator/refresh` (habilitado con Spring Boot Actuator).
    

El problema: si hay varias instancias de un mismo microservicio (ej. en puertos 9091, 9092, 9093), el refresh se aplica **solo a la instancia donde se ejecuta la llamada**. El resto siguen usando la configuración antigua.

---

### Solución con Spring Cloud Bus

**Spring Cloud Bus** resuelve este problema:

- Conecta todos los microservicios y el Config Server mediante un **message broker** (ej. RabbitMQ o Kafka).
    
- Cuando se invoca `/actuator/bus-refresh`, el cambio se propaga a **todas las instancias** automáticamente.
    
- Los microservicios detectan el evento, vuelven a consultar al Config Server y cargan los valores actualizados.
    

---

### Ejemplo práctico

1. Se agrega una **propiedad personalizada** (`com.bharath.springcloud.prop`) en `product-service.properties` del repositorio de configuración.

```properties
com.bharath.springcloud.prop=value1
```

2. En el **Product Service**, se inyecta esa propiedad con `@Value` y se expone en un endpoint `/productapi/prop`.

```java
package com.bharath.springcloud.controllers;  
  
import com.bharath.springcloud.model.Coupon;  
import com.bharath.springcloud.model.Product;  
import com.bharath.springcloud.repos.ProductRepo;  
import com.bharath.springcloud.restclients.CouponClient;  
import io.github.resilience4j.retry.annotation.Retry;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.beans.factory.annotation.Value;  
import org.springframework.web.bind.annotation.RequestBody;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RequestMethod;  
import org.springframework.web.bind.annotation.RestController;  
  
@RestController  
@RequestMapping("productapi")  
public class ProductRestController {  
    
    @Value("${com.bharath.springcloud.prop}")  
    private String prop;    
    
    @RequestMapping(value = "/prop", method = RequestMethod.GET)  
    public String getProperty() {  
        return prop;  
    }  
}
```

    - Así, al acceder desde navegador, se ve el valor actual.
	        
3. Si se cambia el valor en Git:
    
    - Inicialmente no cambia en el navegador porque el microservicio sigue con la configuración cargada.
        
    - Con `/actuator/refresh` se actualiza, pero **solo en esa instancia**.
        
    - Con `/actuator/bus-refresh`, todas las instancias se sincronizan automáticamente.
        

---

### Configuración necesaria

1. **Spring Boot Actuator**
    
    - Dependencia: `spring-boot-starter-actuator`.

```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-actuator</artifactId>  
</dependency>
```

    - En `application.properties`:
        
        `management.endpoints.web.exposure.include=*`

```properties
com.bharath.springcloud.prop=value1
management.endpoints.web.exposure.include=*
```

    - El controlador debe tener anotación `@RefreshScope`.

```java
package com.bharath.springcloud.controllers;  
  
import com.bharath.springcloud.model.Coupon;  
import com.bharath.springcloud.model.Product;  
import com.bharath.springcloud.repos.ProductRepo;  
import com.bharath.springcloud.restclients.CouponClient;  
import io.github.resilience4j.retry.annotation.Retry;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.context.config.annotation.RefreshScope;  
import org.springframework.web.bind.annotation.RequestBody;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RequestMethod;  
import org.springframework.web.bind.annotation.RestController;  
  
@RestController  
@RequestMapping("productapi")
@RefreshScope  
public class ProductRestController {  
    
    @Value("${com.bharath.springcloud.prop}")  
    private String prop;    
    
    @RequestMapping(value = "/prop", method = RequestMethod.GET)  
    public String getProperty() {  
        return prop;  
    }  
}
```

Desplegar **Product Service** en multiples puertos: **9090** y **9091**.

Si hacemos un **POST** al la url:`http://localhost:9090/actuator/refresh` se actualiza, pero **solo en esa instancia**.

Para solucionar el problema debemos configurar un **message broker** (ej. RabbitMQ o Kafka)

2. **RabbitMQ** (message broker por defecto).
    
    - Descargar e instalar RabbitMQ.
        
    - Por defecto usa:
        
        `host=localhost port=5672 username=guest password=guest`
        
3. **Spring Cloud Bus**
    
    - Agregar en **Product Service** la dependencia en `pom.xml`:

```xml
<dependency>   
	<groupId>org.springframework.cloud</groupId>   
	<artifactId>spring-cloud-starter-bus-amqp</artifactId> 
</dependency>
```

- Configuración en `bootstrap.properties`:

```properties
spring.rabbitmq.host=localhost 
spring.rabbitmq.port=5672 
spring.rabbitmq.username=guest 
spring.rabbitmq.password=guest
```

4. **Endpoint de refresh global**
    
    - En lugar de `/actuator/refresh`, se usa:
        
        `POST /actuator/busrefresh`
        

---

### Prueba en acción

1. Levantar **Config Server**, **RabbitMQ** y varias instancias de **Product Service** (ej. en 9091 y 9092).
    
2. Cambiar el valor de la propiedad en el archivo de configuración Git.
    
3. Ejecutar:
    
    `POST http://localhost:9091/actuator/busrefresh`
    
4. Resultado:
    
    - Todas las instancias (9091, 9092, …) obtienen el nuevo valor automáticamente.
        
    - El navegador muestra la actualización en ambas sin necesidad de reiniciar o hacer refresh manual en cada una.
        

---

## 📝 Resumen estructurado

🔹 **Problema sin Cloud Bus:**

- Cambios en configuraciones requieren reinicio o `/actuator/refresh`.
    
- Refresh se aplica solo a **una instancia** del microservicio.
    

🔹 **Solución con Spring Cloud Bus:**

- Conecta microservicios con Config Server usando un **message broker** (RabbitMQ/Kafka).
    
- Propaga automáticamente cambios de configuración a todas las instancias con `/actuator/bus-refresh`.
    

🔹 **Pasos clave:**

1. Configurar Actuator (`management.endpoints.web.exposure.include=*`).
    
2. Anotar controladores con `@RefreshScope`.
    
3. Instalar y ejecutar RabbitMQ.
    
4. Agregar dependencia `spring-cloud-starter-bus-amqp`.
    
5. Usar `/actuator/bus-refresh` en lugar de `/actuator/refresh`.
    

🔹 **Beneficios:**

- Configuración centralizada y sincronizada en todos los entornos.
    
- Menos reinicios manuales.
    
- Escalabilidad en entornos con múltiples instancias.
    

✅ Con Spring Cloud Bus, cualquier cambio en configuración se refleja en **todas las instancias** de forma automática y transparente.

---