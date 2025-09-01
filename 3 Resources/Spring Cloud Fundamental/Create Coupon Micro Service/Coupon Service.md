
---

**Crear el proyecto**

1. El primer paso es crear el proyecto Spring Boot del **Coupon Service**.
    
    - En **Spring Tools Suite (STS)**:
        
        - Menú _File > New > Spring Starter Project_.
            
        - Nombre del proyecto: _coupon-service_.
            
        - Group: `com.barath.springcloud.com`.
            
        - Artifact: _coupon-service_.
            
        - Descripción: _coupon service_.
            
        - Package: igual al group.
            
2. **Dependencias necesarias:**
    
    - **Spring Web** → para exponer servicios REST.
        
    - **Spring Data JPA** → para el mapeo objeto-relacional.
        
    - **MySQL Driver** → para conectarse a la base de datos.
        

Esto genera el proyecto con todas las dependencias en el `pom.xml`.

---

**Crear modelo y repositorio**

- Crear la clase `Coupon` en el paquete `springcloud.model`.
    
- La tabla `coupon` tiene los campos:
    
    - `id` (long, autoincremental, @Id, @GeneratedValue con `GenerationType.IDENTITY`)
        
    - `code` (String)
        
    - `discount` (BigDecimal)
        
    - `expDate` (String)
        
- Generar getters y setters.
    
- Anotar con `@Entity`.
    

```java
package com.bharath.springcloud.model;  
  
  
import jakarta.persistence.Entity;  
import jakarta.persistence.GeneratedValue;  
import jakarta.persistence.GenerationType;  
import jakarta.persistence.Id;  
  
import java.math.BigDecimal;  
  
@Entity  
public class Coupon {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Long id;  
    private String code;  
    private BigDecimal discount;  
    private String expDate;  
  
    // Add getters and setters
}
```


Crear el repositorio:

- Interface `CouponRepo` en el paquete `springcloud.repos`.
    
- Extiende de `JpaRepository<Coupon, Long>`.
    
- Spring Data JPA permite consultas automáticas (ejemplo: `findByCode`).
    

```java
package com.bharath.springcloud.repos;  
  
import com.bharath.springcloud.model.Coupon;  
import org.springframework.data.jpa.repository.JpaRepository;  
  
public interface CouponRepo extends JpaRepository<Coupon, Long> {  
    Coupon findByCode(String code);  
}
```

---

**Crear RestController**

- Clase `CouponRestController` en `springcloud.controllers`.
    
- Anotaciones: `@RestController` y `@RequestMapping("/couponapi")`.
    

Métodos expuestos:

1. **Crear cupón (POST)**
    
    - `@RequestMapping(value="/coupons", method=RequestMethod.POST)`
        
    - Recibe un objeto `Coupon` como `@RequestBody`.
        
    - Usa `repo.save(coupon)` para guardarlo en BD.
        
    - Retorna el cupón guardado (incluyendo el ID generado).
        
2. **Obtener cupón (GET)**
    
    - `@RequestMapping(value="/coupons/{code}", method=RequestMethod.GET)`
        
    - Recibe el `code` como `@PathVariable`.
        
    - Retorna el cupón correspondiente usando `repo.findByCode(code)`.
        

```java
package com.bharath.springcloud.controllers;  
  
import com.bharath.springcloud.model.Coupon;  
import com.bharath.springcloud.repos.CouponRepo;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.web.bind.annotation.*;  
  
@RestController  
@RequestMapping("/couponapi")  
public class CouponRestController {  
  
    @Autowired  
    private CouponRepo repo;  
  
    @RequestMapping(value = "/coupons", method = RequestMethod.POST)  
    public Coupon create(@RequestBody Coupon coupon) {  
        return repo.save(coupon);  
    }  
  
    @RequestMapping(value = "/coupons/{code}", method = RequestMethod.GET)  
    public Coupon getCoupon(@PathVariable("code") String code) {  
        return repo.findByCode(code);  
    }  
  
}
```

---

**Configurar DataSource**

En `application.properties` dentro de `src/main/resources`:

`spring.datasource.url=jdbc:mysql://localhost:3306/mydb spring.datasource.username=root spring.datasource.password=test1234`

_(con puerto y credenciales ajustadas según la instalación local de MySQL)_

Esto conecta la aplicación a la base de datos.

```properties
spring.application.name=couponservice  
spring.datasource.url=jdbc:mysql://localhost:3306/mydb  
spring.datasource.username=root  
spring.datasource.password=admin
```

---

**Prueba**

- Ejecutar la app con **Run as Spring Boot App**.
    
- Probar con **Postman**.
    

1. **Crear cupón (POST)**
    
    - URL: `http://localhost:8080/couponapi/coupons`
        
    - Body (JSON):
        
        `{   "code": "SUPERSALE",   "discount": 10,   "expDate": "12-12-2020" }`
        
    - Respuesta: cupón creado con ID autogenerado.
        
    
    Verificar en la base de datos:
    
    `SELECT * FROM coupon;`
    
    Se observa el registro guardado.
    
2. **Obtener cupón (GET)**
    
    - URL: `http://localhost:8080/couponapi/coupons/SUPERSALE`
        
    - Respuesta: devuelve los datos del cupón.
        

Ambos endpoints funcionan correctamente.

---

## 📝 Resumen estructurado

🔹 **Objetivo:**  
Crear un microservicio **Coupon Service** con Spring Boot que expone APIs REST para crear y consultar cupones.

🔹 **Componentes principales:**

- **Modelo:** `Coupon` → campos: id, code, discount, expDate.
    
- **Repositorio:** `CouponRepo` (extiende JpaRepository).
    
- **Controlador REST:** `CouponRestController` con endpoints:
    
    - POST `/couponapi/coupons` → crea cupón.
        
    - GET `/couponapi/coupons/{code}` → obtiene cupón por código.
        

🔹 **Configuración:**

- Base de datos MySQL conectada vía `application.properties`.
    

🔹 **Pruebas:**

- POST → crea cupón y guarda en la base.
    
- GET → recupera cupón por código.
    

✅ Con esto, el **Coupon Service** queda funcional y listo para integrarse con otros microservicios (ej. Product Service).

---