
---

### Introducción

Cuando implementamos una arquitectura de microservicios y varios de ellos están en ejecución, si uno falla no queremos que el sistema entero colapse.

Ejemplo:

- En el caso de **Product Service** y **Coupon Service**, si el Coupon Service cae, no queremos que el Product Service también falle devolviendo excepciones al cliente.
- Queremos que Product Service reintente y maneje los errores de manera **tolerante a fallos**.

Para esto se usa **Resilience4j**, que permite:

- Reintentos automáticos.
- Métodos de fallback (alternativos).
- Configuración flexible.

---

### Configuración de Resilience4j

1. **Dependencias Maven**
    
    - Asegurarse de tener `spring-boot-starter-actuator`.
```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-actuator</artifactId>  
</dependency>
```


    - Agregar dependencia:
        

```xml
<dependency>   
	<groupId>io.github.resilience4j</groupId>   
	<artifactId>resilience4j-spring-boot2</artifactId> 
</dependency>
```
        
2. **Uso en Product Service**
    
    - En el `ProductRestController`, el método que llama a `CouponClient` puede fallar si Coupon Service no responde.
        
    - Anotar el método con:
        
        `@Retry(name = "product-api")`
        
    - Esto hace que Resilience4j intente la ejecución varias veces en caso de error.
        
3. **Configuración en `application.properties`**
    
    - Ejemplo:

```properties
resilience4j.retry.instances.product-api.maxAttempts=2 resilience4j.retry.instances.product-api.waitDuration=3s
```

- `maxAttempts`: número máximo de intentos (ej. 2).
- `waitDuration`: tiempo de espera entre intentos (ej. 3 segundos).
- Se pueden definir excepciones específicas para reintentar o ignorar.

---

### Prueba de reintentos

- Modificar `CouponClient` para provocar un error (ej. cambiar la URL del endpoint).
- Al crear un producto, el Product Service intentará varias veces antes de lanzar excepción.
- Se observará un **delay** (ej. 6 segundos con 2 reintentos de 3s).
- En consola se registran los errores de Feign Client (ej. 404).

---

### Métodos de Fallback

Los reintentos ayudan, pero si después de varios intentos sigue fallando, el cliente aún recibiría una excepción fea.

Para evitar esto:

- Usar atributo `fallbackMethod` en la anotación:
    
    `@Retry(name = "product-api", fallbackMethod = "handleError")`
    
- Implementar el método `handleError` con la **misma firma** que el original:
    
```java
public Product handleError(Product product, Exception ex) {
     System.out.println("Inside handleError: " + ex.getMessage());     
     return product; // Se devuelve un producto dummy (sin ID) 
}
```
    
- Así, en lugar de error, el cliente recibe una respuesta controlada (aunque el producto no se guarde en BD).
    

Version final de `ProductRestController`:

```java
package com.bharath.springcloud.controllers;  
  
import com.bharath.springcloud.model.Coupon;  
import com.bharath.springcloud.model.Product;  
import com.bharath.springcloud.repos.ProductRepo;  
import com.bharath.springcloud.restclients.CouponClient;  
import io.github.resilience4j.retry.annotation.Retry;  
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
    @Retry(name = "product-api", fallbackMethod = "handleError")  
    public Product create(@RequestBody Product product) {  
        Coupon coupon = couponClient.getCoupon(product.getCouponCode());  
        product.setPrice(product.getPrice().subtract(coupon.getDiscount()));  
        return repo.save(product);  
    }  
  
    public Product handleError(Product product, Exception ex) {  
        System.out.println("Inside handleError: " + ex.getMessage());  
        return product; // Se devuelve un producto dummy (sin ID)  
    }  
}
```

---

## 📝 Resumen estructurado

🔹 **Problema:**  
Si un microservicio falla (ej. Coupon Service), no queremos que Product Service también colapse.

🔹 **Solución:**  
Integrar **Resilience4j** para:

- Reintentos automáticos (`@Retry`).
- Métodos de fallback cuando los reintentos fallan.
    

🔹 **Pasos clave:**

1. Agregar dependencia `resilience4j-spring-boot2`.
2. Anotar métodos con `@Retry(name="...")`.
3. Configurar en `application.properties`:
    
    - `maxAttempts`
    - `waitDuration`
    - Excepciones específicas (incluir o ignorar).
        
4. Definir método de **fallback** con misma firma que el original.
    

🔹 **Beneficios:**

- Sistema tolerante a fallos.
- Respuestas más amigables para el cliente.
- Evita que un microservicio caído tumbe al resto.
    

🔹 **Prueba exitosa:**

- Con URL errónea, Product Service reintenta 2 veces (con 3s de espera cada una).
- Finalmente, entra en `handleError` y devuelve un producto dummy en vez de error 500.
    

✅ Resultado: los microservicios se vuelven **más resilientes y confiables**.

---