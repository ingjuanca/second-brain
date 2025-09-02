
---

**Introducción**

Hasta ahora se han creado dos microservicios: **Coupon Service** y **Product Service**.  
Para que el Product Service funcione, necesita llamar al Coupon Service y obtener un cupón, de modo que pueda aplicar el descuento.  
Esto convierte al Product Service en un **cliente REST** del Coupon Service.

Spring MVC ofrece el **RestTemplate** para realizar llamadas REST, pero requiere mucho código.  
En cambio, **Spring Cloud** ofrece **Feign Client**, que permite crear clientes REST de forma declarativa, fácil y con integración directa con Eureka.

---

**Pasos para usar Feign en Product Service**

1. **Agregar dependencia Maven**
    
    - Editar el `pom.xml` de Product Service.
        
    - Añadir: spring-cloud-starter-openfeign

```xml
<dependency>  
    <groupId>org.springframework.cloud</groupId>  
    <artifactId>spring-cloud-starter-openfeign</artifactId>  
</dependency>
```


2. **Habilitar soporte Feign**
    
    - En la clase principal de Product Service, agregar: `@EnableFeignClients`

```java
package com.bharath.springcloud;  
  
import org.springframework.boot.SpringApplication;  
import org.springframework.boot.autoconfigure.SpringBootApplication;  
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;  
import org.springframework.cloud.openfeign.EnableFeignClients;  
  
@SpringBootApplication  
@EnableDiscoveryClient  
@EnableFeignClients  
public class ProductserviceApplication {  
  
    public static void main(String[] args) {  
       SpringApplication.run(ProductserviceApplication.class, args);  
    }  
  
}
```

3. **Crear Feign REST Client (interfaz)**
    
    - Crear interfaz `CouponClient` en `springcloud.restclients`.
        
    - Anotarla con:
        
        `@FeignClient("COUPON-SERVICE")`
        
    - Definir método: 

```java
package com.bharath.springcloud.restclients;  
  
import com.bharath.springcloud.model.Coupon;  
import org.springframework.cloud.openfeign.FeignClient;  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.PathVariable;  
  
@FeignClient("COUPON-SERVICE")  
public interface CouponClient {  
  
    @GetMapping("/couponapi/coupons/{code}")  
    Coupon getCoupon(@PathVariable("code") String code);  
  
}
```

    - Crear clase `Coupon` (DTO) en Product Service, basada en la de Coupon Service, pero sin anotaciones JPA (solo POJO para serializar JSON).

```java
package com.bharath.springcloud.model;  
  
import java.math.BigDecimal;  
  
public class Coupon {  
  
    private Long id;  
    private String code;  
    private BigDecimal discount;  
    private String expDate;  
  
    // Add the Getters and Setters
}
```

4. **Modificar Product Service**
    
    - En el modelo `Product`, agregar campo `couponCode` marcado con `@Transient` (no se guarda en la BD).

```java
package com.bharath.springcloud.model;  
  
import jakarta.persistence.*;  
  
import java.math.BigDecimal;  
  
@Entity  
public class Product {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Long id;  
    private String name;  
    private String description;  
    private BigDecimal price;  
    @Transient  
    private String couponCode;  
	
	// Add Getters and Setters
}
```

    - En `ProductRestController`:
        
        - Inyectar `CouponClient`.
            
        - En el método de creación:
            
            - Usar el `couponCode` enviado por el cliente.
                
            - Llamar a `couponClient.getCoupon(couponCode)`.
                
            - Restar el descuento al precio antes de guardar en la BD.

```java
package com.bharath.springcloud.controllers;  
  
import com.bharath.springcloud.model.Coupon;  
import com.bharath.springcloud.model.Product;  
import com.bharath.springcloud.repos.ProductRepo;  
import com.bharath.springcloud.restclients.CouponClient;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.web.bind.annotation.RequestBody;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RequestMethod;  
import org.springframework.web.bind.annotation.RestController;  
  
@RestController  
@RequestMapping("productapi")  
public class ProductRestController {  
  
    @Autowired  
    private ProductRepo repo;  
  
    @Autowired  
    private CouponClient couponClient;  
  
    @RequestMapping(value = "/products", method = RequestMethod.POST)  
    public Product create(@RequestBody Product product) {  
        Coupon coupon = couponClient.getCoupon(product.getCouponCode());  
        product.setPrice(product.getPrice().subtract(coupon.getDiscount()));  
        return repo.save(product);  
    }  
}
```

---

**Prueba**

1. Ejecutar en orden:
    
    - Eureka Server (puerto 8761).
        
    - Coupon Service (puerto 8080).
        
    - Product Service (puerto 9090).
        
2. En **Postman**, enviar un POST a:
    
    `http://localhost:9090/productapi/products`
    
    Con body (JSON):
    
    `{   "name": "iPhone",   "description": "Awesome",   "price": 1000,   "couponCode": "SUPERSALE" }`
    
3. Resultado:
    
    - El Product Service llama al Coupon Service vía Feign Client.
        
    - El cupón aplica un descuento de 10.
        
    - El producto se guarda con precio **990** en la base de datos.
        

Todo esto ocurre gracias a la integración automática de **Feign + Eureka + Spring Cloud**.

---

## 📝 Resumen estructurado

🔹 **Problema:**  
El Product Service necesita comunicarse con Coupon Service. Usar RestTemplate es tedioso.

🔹 **Solución:**  
Spring Cloud Feign Client permite crear clientes REST declarativos e integrados con Eureka.

🔹 **Pasos clave:**

1. Agregar dependencia `spring-cloud-starter-openfeign`.
    
2. Anotar Product Service con `@EnableFeignClients`.
    
3. Crear `CouponClient` con `@FeignClient("coupon-service")`.
    
4. Modificar modelo `Product` → añadir `couponCode` (`@Transient`).
    
5. En `ProductRestController`:
    
    - Obtener cupón con `couponClient.getCoupon(couponCode)`.
        
    - Restar descuento al precio.
        
    - Guardar producto actualizado en BD.
        

🔹 **Prueba exitosa:**

- POST con `couponCode` aplica descuento automáticamente.
    
- Precio final se guarda en BD (ejemplo: 1000 → 990).
    

✅ Con esto, Product Service queda totalmente integrado con Coupon Service mediante Feign y Eureka.

---