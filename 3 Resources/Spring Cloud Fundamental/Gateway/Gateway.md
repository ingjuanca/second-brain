
---

### Introducción

En arquitecturas de microservicios existen varios **requisitos no funcionales**:

- **Seguridad**: autenticación, autorización, encriptación, desencriptación.
- **Trazabilidad**: seguir el flujo de una petición entre múltiples microservicios.
- **Agregación de servicios**: cuando un cliente debe invocar diferentes microservicios.
- **Control de consumo de recursos**: aplicar límites de uso (rate limits) en la nube (AWS, Azure).

Estos requisitos comunes se conocen como **cross-cutting concerns** (preocupaciones transversales).  
En lugar de implementarlos repetidamente en cada microservicio, se centralizan usando el componente **Spring Cloud API Gateway**.

El API Gateway actúa como punto único de entrada:

- Todo tráfico pasa por el Gateway.
- Aplica seguridad, trazabilidad, agregación, filtros personalizados, etc.
- Ejemplo: si Product Service quiere usar Coupon Service, la llamada pasa primero por el Gateway.

---

### Creación del proyecto API Gateway

1. En **Spring Tool Suite (STS)** → _File → New → Spring Starter Project_.
    
    - Nombre: **api-gateway-service**.
    - Grupo: `com.bharath.springcloud`.
    - Descripción: _API Gateway_.
    
2. **Dependencias necesarias**:
    
    - `spring-cloud-starter-gateway-server-webflux` (Spring Cloud Routing).
    - `spring-cloud-starter-netflix-eureka-client` (descubrimiento de servicios).
    
3. En `application.properties`:
    
    - Copiar configuración de conexión a Eureka (como en Coupon Service).
    - Cambiar el nombre de la aplicación a **gateway-service**.
    - Definir puerto, por ejemplo:
        `server.port=9095`
	- Habilitar las siguientes propiedades en caso de error: `UnknownHostException`

```properties
spring.cloud.gateway.discovery.locator.enabled=true  
eureka.instance.prefer-ip-address=true
```

- `eureka.instance.prefer-ip-address=true` también se debe agregar en `product-service` 


El Gateway se registrará en **Eureka** como `gateway-service`.

---

### Configuración de rutas

En `application.properties` del API Gateway:

- Definir rutas con la propiedad:

```properties
spring.cloud.gateway.routes[0].id=coupon-service spring.cloud.gateway.routes[0].uri=lb://COUPON-SERVICE spring.cloud.gateway.routes[0].predicates[0]=Path=/couponapi/**
```

- Otra ruta para Product Service:
   
```properties
spring.cloud.gateway.routes[1].id=product-service spring.cloud.gateway.routes[1].uri=lb://PRODUCT-SERVICE spring.cloud.gateway.routes[1].predicates[0]=Path=/productapi/**
```

Explicación:

- `id` = identificador único de la ruta.
- `uri` = servicio registrado en Eureka (con soporte de load balancer).
- `predicates` = define los paths que manejará el Gateway para cada servicio.

Versión final del archivo de propiedades:

```properties
spring.application.name=gateway-service  
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/  
server.port=9095  
  
spring.cloud.gateway.routes[0].id=coupon-module  
spring.cloud.gateway.routes[0].uri=lb://COUPON-SERVICE  
spring.cloud.gateway.routes[0].predicates[0]=Path=/couponapi/**  
  
spring.cloud.gateway.routes[1].id=product-module  
spring.cloud.gateway.routes[1].uri=lb://PRODUCT-SERVICE  
spring.cloud.gateway.routes[1].predicates[0]=Path=/productapi/**  
  
spring.cloud.gateway.discovery.locator.enabled=true  
eureka.instance.prefer-ip-address=true
```

---

### Integración con Product Service

- En `CouponClient` del Product Service, cambiar el Feign Client de:
    
    `@FeignClient("coupon-service")`
    a:
    `@FeignClient("gateway-service")`

```java
package com.bharath.springcloud.restclients;  
  
import com.bharath.springcloud.model.Coupon;  
import org.springframework.cloud.openfeign.FeignClient;  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.PathVariable;  
  
@FeignClient("GATEWAY-SERVICE")  
public interface CouponClient {  
  
    @GetMapping("/couponapi/coupons/{code}")  
    Coupon getCoupon(@PathVariable("code") String code);  
  
}
```

- Así, todas las llamadas pasan por el Gateway, que se encarga de redirigirlas al Coupon Service.
    

### Habilitar el registro del servicio en Eureka

En `Application.java` agregar la anotación `@EnableDiscoveryClient`

```java 
package com.bharath.springcloud.apigatewayservice;  
  
import org.springframework.boot.SpringApplication;  
import org.springframework.boot.autoconfigure.SpringBootApplication;  
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;  
  
@SpringBootApplication  
@EnableDiscoveryClient  
public class ApigatewayserviceApplication {  
  
    public static void main(String[] args) {  
       SpringApplication.run(ApigatewayserviceApplication.class, args);  
    }  
  
}
```

---

### Ejecución y prueba

1. Detener todo y reiniciar en orden:
    
    - Eureka Server (8761).
    - Coupon Service.
    - Product Service.
    - API Gateway Service (9095).
        
2. En **Postman**, antes se llamaba directamente a Product Service (`localhost:9090`).  
    Ahora se usa el puerto del Gateway:
    
    `http://localhost:9095/productapi/products`
    
3. Flujo de ejecución:
    
    - El cliente llama al Gateway (9095).
    - El Gateway reenvía al Product Service.
    - El Product Service llama al Coupon Client.
    - Coupon Client ahora apunta al Gateway, que reenvía la petición al Coupon Service.
    - Coupon Service responde → Gateway → Product Service → Gateway → Cliente.

Todo el tráfico fluye a través del API Gateway.

---

## 📝 Resumen estructurado

🔹 **Problema:**  
Cross-cutting concerns (seguridad, trazabilidad, agregación, rate limiting) no deberían repetirse en cada microservicio.

🔹 **Solución:**  
Usar **Spring Cloud API Gateway** como punto único de entrada.

🔹 **Configuración del Gateway:**

- Dependencias: `spring-cloud-starter-gateway-server-webflux`, `spring-cloud-starter-netflix-eureka-client`.
    
- Nombre de aplicación: `gateway-service`.
    
- Puerto: 9095.
    
- Rutas configuradas en `application.properties` para Coupon y Product Service.
    

🔹 **Beneficios del Gateway:**

- Seguridad y trazabilidad centralizadas.
- Enrutamiento dinámico con Eureka.
- Balanceo de carga automático.
- Posibilidad de filtros personalizados.


🔹 **Flujo:**  
Cliente → API Gateway (9095) → Product Service → API Gateway → Coupon Service → respuesta → API Gateway → Cliente.

✅ Todo el tráfico pasa por el Gateway, lo que centraliza control y simplifica mantenimiento.

---