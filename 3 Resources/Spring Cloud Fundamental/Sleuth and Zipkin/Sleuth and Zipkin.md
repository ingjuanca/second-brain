
---

### Introducción

Cuando tenemos muchos microservicios en ejecución, es importante poder **rastrear las solicitudes de los clientes** para ver cómo fluyen de un microservicio a otro y detectar en qué punto fallan.

- **Sleuth** y **Zipkin** proporcionan capacidades de **trazabilidad distribuida**.

**Sleuth**:

- Se agrega como dependencia (`micrometer-tracing-bridge-brave`).
- Añade automáticamente un **Trace ID** y **Span ID** a los logs y a las cabeceras HTTP.
- Permite seguir una solicitud a través de todos los servicios.

**Zipkin**:

- Se agrega como dependencia (`zipkin-reporter-brave`).
- Recibe y centraliza todas las trazas enviadas por Sleuth.
- Ofrece un **dashboard web** (en `http://localhost:9411`) para visualizar el flujo de peticiones entre microservicios.

---

### Configuración de Sleuth y Zipkin

1. **Agregar dependencias en `pom.xml`**  
    En **API Gateway**, **Coupon Service** y **Product Service**, añadir:
    
```xml
<dependency>  
    <groupId>io.micrometer</groupId>  
    <artifactId>micrometer-tracing-bridge-brave</artifactId>  
</dependency>  
  
<dependency>  
    <groupId>io.zipkin.reporter2</groupId>  
    <artifactId>zipkin-reporter-brave</artifactId>  
</dependency>
```
    
2. **Ejecución de Zipkin Server**
    
    - Descargar el JAR con `curl`:
        
        `curl -sSL https://zipkin.io/quickstart.sh | bash -s java -jar zipkin.jar`
        
    - Zipkin corre en **puerto 9411**.
        
    - Acceder en navegador: `http://localhost:9411`.
        
    
    _(Si falla en Windows, se puede correr con Docker:)_
    
    `docker run -d -p 9411:9411 openzipkin/zipkin`
    
3. **Funcionamiento de Sleuth**
    
    - API Gateway añade el **Trace ID** en las cabeceras HTTP.
        
    - Product Service reutiliza ese Trace ID y lo incluye en sus logs.
        
    - Si Product Service llama a Coupon Service, también reenvía ese Trace ID.
        
    - Resultado: todos los logs contienen el mismo Trace ID → se puede seguir toda la ruta.
        

---

### Prueba en acción

1. **Orden de arranque:**
    
    - Eureka Server.
        
    - Coupon Service.
        
    - Product Service.
        
    - API Gateway.
        
2. **Enviar petición desde Postman** (crear producto).
    
    - La solicitud fluye: Cliente → API Gateway → Product Service → API Gateway → Coupon Service.
        
3. **Verificar en Zipkin Dashboard:**
    
    - En `http://localhost:9411`, seleccionar **Service Name** (ej. `gateway-service`, `product-service`, `coupon-service`).
        
    - Al hacer consulta (`Find Traces`), se muestra el flujo completo.
        
    - Ejemplo: `Gateway → Product → Gateway → Coupon`.
        

Esto permite rastrear con precisión dónde ocurrió un fallo en el flujo de microservicios.

---

## 📝 Resumen estructurado

🔹 **Problema:**  
Es difícil rastrear peticiones en sistemas con múltiples microservicios y detectar dónde fallan.

🔹 **Solución:**

- **Sleuth:** añade `Trace ID` y `Span ID` a logs y cabeceras.
    
- **Zipkin:** centraliza y muestra trazas en un dashboard.
    

🔹 **Pasos clave:**

1. Agregar dependencias (`micrometer-tracing-bridge-brave` y `zipkin-reporter-brave`) en todos los servicios.
    
2. Ejecutar Zipkin Server (JAR o Docker).
    
3. Configurar aplicaciones para enviar trazas a Zipkin.
    

🔹 **Prueba:**

- Enviar una petición de creación de producto.
    
- Consultar Zipkin Dashboard → muestra flujo Cliente → Gateway → Product → Gateway → Coupon.
    

🔹 **Beneficios:**

- Rastreabilidad total de solicitudes.
    
- Identificación rápida de microservicio donde ocurre un fallo.
    
- Visualización clara del flujo de llamadas entre servicios.
    

✅ Con esto se implementa **trazabilidad distribuida completa** en la arquitectura de microservicios.

---

¿Quieres que te prepare también un **diagrama visual del flujo de trazas con Trace ID** (Cliente → Gateway → Product → Coupon → Zipkin) para que quede aún más claro?