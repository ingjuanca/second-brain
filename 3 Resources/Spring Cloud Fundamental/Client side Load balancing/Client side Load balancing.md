
---

**Introducción**

En esta sección aprenderás cómo hacer **balanceo de carga del lado del cliente**.  
A medida que aumentan las llamadas a los microservicios, se deben escalar desplegando diferentes instancias del mismo microservicio en distintos servidores. Las solicitudes del cliente se enviarán a una de esas instancias en lugar de que todas vayan a una sola, distribuyendo así la carga.

Spring Cloud nos provee un **componente de balanceador de carga**, que funciona automáticamente junto con **Feign Client**.  
Este balanceador decide a qué instancia del microservicio se debe enviar cada solicitud.

Configurar el balanceador es muy sencillo:

- Basta con agregar la dependencia de **Spring Cloud Load Balancer** en el `pom.xml` del Product Service.
- No se necesita ninguna otra configuración adicional.
- Cuando el Product Service haga una llamada al Coupon Service mediante Feign Client, el balanceo se realizará automáticamente si existen múltiples instancias del Coupon Service.
    

---

**Configuración paso a paso**

1. **Agregar dependencia**

- En el `pom.xml` de Product Service, copiar la dependencia de Feign y reemplazarla por:


```xml
<dependency>   
	<groupId>org.springframework.cloud</groupId>   
	<artifactId>spring-cloud-starter-loadbalancer</artifactId> 
</dependency>
```

2. **Ejecutar múltiples instancias de Coupon Service**

- La primera instancia corre por defecto en el puerto **8080**.

	- Para la segunda instancia, en `application.properties`:
        
        `server.port=8081`
        
    - Modificar el `CouponRestController` para imprimir en consola si es “Server 1” (8080) o “Server 2” (8081), lo que permitirá distinguir qué instancia está atendiendo las peticiones.
        
3. **Levantar servicios**
    
    - Iniciar **Eureka Server** (8761).
    - Iniciar primera instancia de **Coupon Service** (8080).
    - Iniciar segunda instancia de **Coupon Service** (8081).
    - Iniciar **Product Service** (9090).
        
4. **Verificar en Eureka**
    
    - En `http://localhost:8761`, se deben listar dos instancias de **Coupon Service** registradas:
        
        - Una en puerto 8080
            
        - Otra en puerto 8081
            

---

**Prueba con Postman**

1. Enviar petición POST a:
    
    `http://localhost:9090/productapi/products`
    
    con datos de un producto.
    
2. Observar en consola:
    
    - Primera petición → atendida por instancia en 8080 (se imprime _Server 1_).
        
    - Segunda petición → atendida por instancia en 8081 (se imprime _Server 2_).
        

El balanceador distribuye las peticiones entre las dos instancias automáticamente.

---

## 📝 Resumen estructurado

🔹 **Problema:**  
Un solo microservicio no puede manejar muchas peticiones → se requiere distribuir la carga en múltiples instancias.

🔹 **Solución con Spring Cloud:**

- Usar **Spring Cloud Load Balancer** integrado con **Feign Client**.
    
- Permite balanceo automático entre instancias registradas en Eureka.
    

🔹 **Pasos clave:**

1. Agregar dependencia `spring-cloud-starter-loadbalancer`.
    
2. Levantar múltiples instancias del Coupon Service en diferentes puertos (ej. 8080 y 8081).
    
3. Registrar instancias en Eureka Server.
    
4. Product Service hace las llamadas vía Feign → balanceo automático entre instancias.
    

🔹 **Resultado:**

- Primera petición → atendida por instancia en 8080.
    
- Segunda petición → atendida por instancia en 8081.
    
- Peticiones siguientes se reparten entre ambas.
    

✅ Así, el balanceo de carga en el cliente ocurre de forma **automática** gracias a Spring Cloud.

---