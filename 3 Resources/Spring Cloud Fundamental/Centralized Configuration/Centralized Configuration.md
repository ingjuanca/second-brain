
---

### Introducción

Cada aplicación de microservicios tiene configuraciones asociadas:

- Conexión a base de datos.
- Información de un _message broker_ (si aplica).
- Configuración específica de la aplicación.

Estas configuraciones varían según el entorno:

- Local del desarrollador.
- Servidores de QA, _staging_ y producción.
- Cada uno con sus propias bases de datos, _brokers_ y parámetros.

El reto: garantizar que cada entorno reciba la configuración correcta.

**Solución → Configuración centralizada**:  
Spring Cloud provee un **Config Server**.

- Los microservicios consultan al Config Server para obtener la configuración del entorno.
- El Config Server obtiene los archivos desde un repositorio Git (local o remoto).
- Los desarrolladores solo deben actualizar los archivos de configuración en Git.

---

### Pasos principales

1. **Crear Config Server**
    
    - Proyecto Spring Boot con dependencia `spring-cloud-config-server`.
        
    - Clase principal anotada con `@EnableConfigServer`.
        
    - Configuración básica en `application.properties`:

```properties
spring.application.name=config-server 
server.port=8888
```

_(En versiones nuevas, agregar dependencia `spring-cloud-starter-bootstrap` para que cargue siempre la última configuración de Git)._

```xml
<dependency>  
    <groupId>org.springframework.cloud</groupId>  
    <artifactId>spring-cloud-config-server</artifactId>  
</dependency>

<dependency>         
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bootstrap</artifactId>
</dependency>
```

2. **Crear repositorio Git para configuraciones**

    - Crear carpeta local (ej. `local-git-repo`).
    - Inicializar con `git init`.
	- Crear archivo `product-service.properties` con configuraciones iniciales, ejemplo:

```properties
management.security.enabled=false 
application.url=http://local
```

    - Hacer `gqit add -A` y `git commit -m "default properties"`.
        
    - Se pueden añadir archivos específicos por entorno, siguiendo la convención:
        
        - `product-service.properties` (default).
            
        - `product-service-dev.properties` (desarrollo).
            
        - `product-service-test.properties` (QA).
            
        - `product-service-prod.properties` (producción).
            
3. **Configurar acceso del Config Server al repositorio Git**
    
    - En `application.properties` del Config Server:
        
        `spring.cloud.config.server.git.uri=file:///${user.home}/local-git-repo`
        
    - Probar en navegador:
        
        - `http://localhost:8888/product-service/default` → devuelve configuración por defecto.
            
        - `http://localhost:8888/product-service/dev` → devuelve configuración para perfil _dev_.
            

---

### Configurar el Config Client (microservicio)

1. En el microservicio (ej. `product-service`):

    - Agregar dependencia

```xml
<dependency>  
    <groupId>org.springframework.cloud</groupId>  
    <artifactId>spring-cloud-starter-bootstrap</artifactId>  
</dependency>

<dependency>  
    <groupId>org.springframework.cloud</groupId>  
    <artifactId>spring-cloud-starter-config</artifactId>  
</dependency>
```

    - Mover configuraciones de `application.properties` al archivo en Git.
    - Dejar solo:

```properties
spring.application.name=product-service spring.cloud.config.uri=http://localhost:8888
```

        
    - Renombrar `application.properties` → `bootstrap.properties`.
        
2. Arrancar servicios en orden:
    
    - Eureka Server.
        
    - Config Server.
        
    - Product Service.
        
3. Verificar:
    
    - Si en Git el puerto está configurado como `1990`, el Product Service inicia en ese puerto.
        
    - Cambiar a `9090` en Git y reiniciar → arranca en `9093`.
        
    - Activar perfil `dev`:
        
        `spring.profiles.active=dev`
        
        → ahora toma configuración de `product-service-dev.properties`.
        

---

## 📝 Resumen estructurado

🔹 **Problema:**  

Cada entorno (local, QA, staging, prod) necesita configuraciones distintas. Mantenerlas en cada microservicio es complejo.

🔹 **Solución:**

- Usar **Spring Cloud Config Server**.
- Centralizar todas las configuraciones en un repositorio Git.
- Microservicios se convierten en **Config Clients** que obtienen su configuración desde el Config Server.


🔹 **Pasos clave:**

1. Crear **Config Server** con `spring-cloud-config-server`.
    
2. Crear repositorio Git (`product-service.properties`, `product-service-dev.properties`, etc.).
    
3. Configurar URI del repositorio en el Config Server.
    
4. Convertir microservicios en **Config Clients** con dependencia `spring-cloud-starter-config` y archivo `bootstrap.properties`.
    

🔹 **Beneficios:**

- Gestión centralizada de configuraciones.
    
- Soporte de múltiples entornos (perfiles).
    
- Cambios aplicados desde Git sin modificar cada microservicio.
    
- Simplificación en despliegues continuos.
    

✅ Con esto, los microservicios cargan dinámicamente la configuración correcta según el entorno (default, dev, test, prod).

---

¿Quieres que te prepare un **ejemplo gráfico** (diagrama) mostrando cómo Config Server obtiene las propiedades de Git y cómo los microservicios (ej. Product Service) las consumen?