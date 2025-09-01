
---

**Crear el Proyecto, Modelo y Repositorio**

En esta lección se empieza a trabajar en el **Product Service**. Se harán los dos primeros pasos juntos: crear el proyecto, el modelo y el repositorio.

1. En **STS → File → New → Spring Starter Project**:
    
    - Nombre del proyecto: _product-service_.
        
    - Group: `com.springcloud`.
        
    - Artifact: _product-service_.
        
    - Descripción: _product service_.
        
2. Seleccionar dependencias necesarias (las mismas que en el Coupon Service):
    
    - MySQL Driver
        
    - Spring Data JPA
        
    - Spring Web
        

Esto genera el proyecto **Product Service**.

---

**Modelo `Product`**

Crear clase `Product` en el paquete `springcloud.model`.

Campos de la tabla `product`:

- `id` (Long, autoincremental, @Id, @GeneratedValue con `GenerationType.IDENTITY`)
    
- `name` (String)
    
- `description` (String)
    
- `price` (BigDecimal)
    

Anotar con `@Entity`. Generar getters y setters.

```java
package com.bharath.springcloud.model;  
  
import jakarta.persistence.Entity;  
import jakarta.persistence.GeneratedValue;  
import jakarta.persistence.GenerationType;  
import jakarta.persistence.Id;  
  
import java.math.BigDecimal;  
  
@Entity  
public class Product {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Long id;  
    private String name;  
    private String description;  
    private BigDecimal price;  

// Add the getters and setters
 
}
```

---

**Repositorio `ProductRepo`**

Crear interfaz `ProductRepo` en `springcloud.repo`.

- Extiende `JpaRepository<Product, Long>`.
    
- Esto habilita operaciones CRUD sin necesidad de escribir SQL.
    

```java
package com.bharath.springcloud.repos;  
  
import com.bharath.springcloud.model.Product;  
import org.springframework.data.jpa.repository.JpaRepository;  
  
public interface ProductRepo extends JpaRepository<Product, Long> {  
}
```

---

**RestController**

Crear clase `ProductRestController`.

- Anotaciones: `@RestController`, `@RequestMapping("/productapi")`.
    

Método expuesto:

- **Crear producto (POST)**
    
    - URL: `/productapi/products`
        
    - Recibe un objeto `Product` vía `@RequestBody`.
        
    - Usa `repo.save(product)` para guardar en la BD.
        
    - Devuelve el producto guardado.
        

```java
package com.bharath.springcloud.controllers;  
  
import com.bharath.springcloud.model.Product;  
import com.bharath.springcloud.repos.ProductRepo;  
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
  
    @RequestMapping(value = "/products", method = RequestMethod.POST)  
    public Product create(@RequestBody Product product) {  
        return repo.save(product);  
    }  
}
```

⚠️ Nota:  
En esta etapa **no se aplica descuento** porque aún no se conecta con el **Coupon Service**. Más adelante se integrará para que, al crear el producto, consulte el descuento.

---

**Configurar DataSource**

En `application.properties`, reutilizar configuración del Coupon Service para conectar a MySQL.

Además, definir el puerto del Product Service:

`server.port=9090`

(para evitar conflicto con el Coupon Service que ya usa el puerto 8080).

```properties
spring.application.name=productservice  
spring.datasource.url=jdbc:mysql://localhost:3306/mydb  
spring.datasource.username=root  
spring.datasource.password=admin  
  
server.port=9090
```

---

**Prueba**

1. Ejecutar la app con **Run as Spring Boot App**.
    
2. Probar con **Postman**.
    
    - Endpoint: `http://localhost:9090/productapi/products`
        
    - Método: **POST**
        
    - Body (JSON):
        
        `{   "name": "iPhone",   "description": "Awesome (at least for some people)",   "price": 1000 }`
        
    - Respuesta:
        
        `{   "id": 1,   "name": "iPhone",   "description": "Awesome (at least for some people)",   "price": 1000 }`
        
    - Verificar en BD con:
        
        `SELECT * FROM product;`
        

Resultado: el producto se guarda correctamente.

---

## 📝 Resumen estructurado

🔹 **Objetivo:**  
Crear el **Product Service** con Spring Boot, exponer un API REST para crear productos y guardarlos en la base de datos.

🔹 **Componentes principales:**

- **Modelo:** `Product` → id, name, description, price.
    
- **Repositorio:** `ProductRepo` (extiende JpaRepository).
    
- **Controlador:** `ProductRestController` con endpoint:
    
    - POST `/productapi/products` → crea un producto.
        

🔹 **Configuración:**

- Puerto definido: **9090**.
    
- Conexión a MySQL vía `application.properties`.
    

🔹 **Pruebas:**

- Crear producto desde Postman (ejemplo: iPhone).
    
- Guardado exitoso en BD.
    

✅ El **Product Service** queda funcionando con la operación de **crear productos**.

---