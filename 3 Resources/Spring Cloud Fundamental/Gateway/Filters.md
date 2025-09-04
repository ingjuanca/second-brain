
---

### Introducción a los filtros

El **API Gateway** permite implementar preocupaciones transversales (cross-cutting concerns) como seguridad, trazabilidad, límites de uso, etc., a través de **clases de filtros personalizados**.

- Un filtro puede ejecutar **lógica de preprocesamiento** antes de que la petición llegue al microservicio.
- También puede ejecutar **lógica de postprocesamiento** antes de que la respuesta regrese al cliente.
- Se pueden activar filtros solo para ciertas rutas o en caso de errores.
- Los filtros encapsulan toda la lógica transversal.

Para crearlos:

- Se implementa una interfaz o clase abstracta de API Gateway.
- Se sobreescriben sus métodos (principalmente `filter`).
- Algunos filtros requieren registrarse explícitamente, otros se cargan automáticamente en tiempo de ejecución.

El método `filter` recibe dos parámetros:

1. `ServerWebExchange` → contiene información de la petición y la respuesta.
2. `GatewayFilterChain` → gestiona la cadena de filtros; al llamar `chain.filter(exchange)`, la petición pasa al siguiente filtro o al microservicio.

- **Preprocesamiento** → se coloca antes de `chain.filter()`.
- **Postprocesamiento** → se agrega usando `.then(...)` después de `chain.filter()`, aplicando lógica sobre la respuesta (ej. encriptar, comprimir, añadir headers).

---

### Creación de un filtro personalizado

1. En el proyecto **API Gateway**, crear clase `MyFilter` en el paquete `com.bharath.springcloud.filters`.
    
2. Implementar la interfaz `GlobalFilter`.
    
3. Sobrescribir el método `filter(ServerWebExchange exchange, GatewayFilterChain chain)`.
    
4. En el método:
    
    - Añadir lógica de **preprocesamiento** (ejemplo: imprimir logs, validar seguridad, leer headers, desencriptar).
        
    - Pasar al siguiente filtro con:
        
        `return chain.filter(exchange)             .then(Mono.fromRunnable(() -> {                 // Lógica de postprocesamiento             }));`
        
    - Aquí se ejecuta la lógica **postprocesamiento** (ejemplo: modificar respuesta, comprimir, añadir headers).
        
5. Anotar con `@Component` para que el Gateway lo registre automáticamente.

```java
package com.bharath.springcloud.apigatewayservice.filters;  
  
import org.springframework.cloud.gateway.filter.GatewayFilterChain;  
import org.springframework.cloud.gateway.filter.GlobalFilter;  
import org.springframework.stereotype.Component;  
import org.springframework.web.server.ServerWebExchange;  
import reactor.core.publisher.Mono;  
  
@Component  
public class MyFilter implements GlobalFilter {  
    @Override  
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {  
        System.out.println("Pre Processing logic goes here: " + exchange.getRequest());  
        return chain.filter(exchange).then(Mono.fromRunnable(() -> {  
            System.out.println("Pro Processing logic goes here: " + exchange.getResponse());  
        }));  
    }  
}
```

---

### Ejecución y prueba

1. Reiniciar todo: Eureka Server, Coupon Service, Product Service y finalmente el API Gateway.
    
2. Enviar una petición con **Postman** a través del Gateway (`localhost:9095/productapi/products`).
    
3. En los **logs del API Gateway** se observarán las trazas del filtro:
    
    - **Preprocesamiento 1:** al invocar Product Service.
    - **Preprocesamiento 2:** al invocar Coupon Service a través del Gateway.
    - **Postprocesamiento 1:** al regresar la respuesta del Coupon Service hacia Product Service.
    - **Postprocesamiento 2:** al regresar la respuesta final del Product Service hacia el cliente.

```bash
Pre Processing logic goes here: org.springframework.http.server.reactive.ReactorServerHttpRequest@6cae6d42
Pre Processing logic goes here: org.springframework.http.server.reactive.ReactorServerHttpRequest@75429ff2
Pro Processing logic goes here: org.springframework.http.server.reactive.ReactorServerHttpResponse@71fae046
Pro Processing logic goes here: org.springframework.http.server.reactive.ReactorServerHttpResponse@6ea14746
```

Esto confirma que el filtro está interceptando correctamente tanto las peticiones como las respuestas.

---

## 📝 Resumen estructurado

🔹 **Problema:**  
Necesitamos centralizar lógica común (seguridad, trazabilidad, compresión, headers) en un único lugar sin duplicarla en todos los microservicios.

🔹 **Solución:**  
Crear **filtros en API Gateway** que manejen lógica de preprocesamiento y postprocesamiento.

🔹 **Pasos clave:**

1. Crear clase que implemente `GlobalFilter`.
2. Sobrescribir método `filter(exchange, chain)`.
3. Usar:
    
    - Antes de `chain.filter()` → lógica de **preprocesamiento**.
        
    - `.then(Mono.fromRunnable(...))` → lógica de **postprocesamiento**.
        
4. Anotar con `@Component` para que el Gateway lo registre.

🔹 **Beneficios:**

- Aplicar seguridad, tracing, rate limiting, encriptación/desencriptación en un único lugar.
- Reutilización y mantenimiento centralizados.
- Se ejecuta automáticamente para todas las rutas (o se puede condicionar a ciertas).

🔹 **Prueba exitosa:**  
En los logs se observan 4 eventos claros:

1. Preprocesamiento al llamar Product Service.
2. Preprocesamiento al llamar Coupon Service.
3. Postprocesamiento de respuesta de Coupon Service.
4. Postprocesamiento de respuesta de Product Service.

✅ Con esto, el API Gateway se convierte en un **punto de control centralizado** para manejar lógica transversal en los microservicios.

---