
---

**Introducción**

En esta sección aprenderás sobre **registro y descubrimiento de microservicios**, una característica muy poderosa.

Cuando tenemos múltiples microservicios o múltiples instancias de los mismos ejecutándose, los consumidores necesitan saber detalles como la **URL y el puerto** de cada servicio para poder comunicarse. Esto es difícil de mantener porque:

- Puede haber varias instancias del mismo servicio.
- Una instancia puede estar caída en un momento dado.

Aquí entra **Spring Cloud** con **Eureka**, un servidor de nombres (naming server).

- Cada microservicio se **registra automáticamente** en Eureka al iniciar, usando un nombre único de aplicación (application ID).
- Eureka mantiene la información de URL, puertos y estado de cada servicio.
- Los consumidores solo necesitan el nombre de la aplicación para descubrir y comunicarse con el servicio.

Ejemplo:

- El **Coupon Service** se registra en Eureka.
- El **Product Service** consulta Eureka con el nombre de aplicación y obtiene la información para conectarse al Coupon Service.

---

**Creación de un servidor Eureka**

1. Crear un proyecto **Eureka Server** con dependencia `spring-cloud-starter-netflix-eureka-server`.

	- Nombre del proyecto: _coupon-service_.
            
        - Group: `com.barath.springcloud.com`.
        - Artifact: _coupon-service_.
        - Descripción: _coupon service_.
        - Package: igual al group.
	        
	-  Agregar dependencia al archivo pom.xml

```xml
<properties>  
    <java.version>17</java.version>  
    <spring-cloud.version>2025.0.0</spring-cloud.version>  
</properties>
<dependencyManagement>  
    <dependencies>       
	    <dependency>          
		  <groupId>org.springframework.cloud</groupId>  
		  <artifactId>spring-cloud-dependencies</artifactId>  
		  <version>${spring-cloud.version}</version>  
		  <type>pom</type>  
		  <scope>import</scope>  
	    </dependency>    
    </dependencies>
</dependencyManagement>

<dependencies>  
    <dependency>       
	    <groupId>org.springframework.cloud</groupId>  
	    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>  
    </dependency>
</dependencies>
```

2. En la clase principal agregar la anotación `@EnableEurekaServer`.

```java
package com.bharath.springcloud;  
  
import org.springframework.boot.SpringApplication;  
import org.springframework.boot.autoconfigure.SpringBootApplication;  
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;  
  
@SpringBootApplication  
@EnableEurekaServer  
public class EurekaServerApplication {  
  
    public static void main(String[] args) {  
       SpringApplication.run(EurekaServerApplication.class, args);  
    }  
  
}
```

3. Configuración en `application.properties`:

```properties
spring.application.name=EurekaServer  
server.port=8761  
eureka.client.register-with-eureka=false  
eureka.client.fetch-registry=false
```

_(Estas banderas son `false` porque el servidor no debe registrarse a sí mismo)._

---

**Ejecución del servidor**

- Al ejecutar el proyecto, Eureka corre en el puerto **8761**.
    
- Accediendo a `http://localhost:8761` se muestra la consola de Eureka.
    
- Inicialmente no habrá instancias registradas hasta que los microservicios (ej. Coupon y Product) actúen como clientes.
    

---

**Configurar microservicios como clientes Eureka**

1. En los proyectos `coupon-service` y `product-service`:
    
    - Agregar dependencia `spring-cloud-starter-netflix-eureka-client`.
        
    - Copiar la gestión de dependencias de Spring Cloud (dependency management) desde el `pom.xml` del Eureka Server.
```xml
<properties>  
    <java.version>17</java.version>  
    <spring-cloud.version>2025.0.0</spring-cloud.version>  
</properties>
<dependencyManagement>  
    <dependencies>       
	    <dependency>          
		  <groupId>org.springframework.cloud</groupId>  
		  <artifactId>spring-cloud-dependencies</artifactId>  
		  <version>${spring-cloud.version}</version>  
		  <type>pom</type>  
		  <scope>import</scope>  
	    </dependency>    
    </dependencies>
</dependencyManagement>

<dependencies>  
    <dependency>       
	    <groupId>org.springframework.cloud</groupId>  
	    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>  
    </dependency>
</dependencies>
```
1. En la clase principal de cada microservicio:
    
    - Usar la anotación `@EnableDiscoveryClient`.

```java
package com.bharath.springcloud;  
  
import org.springframework.boot.SpringApplication;  
import org.springframework.boot.autoconfigure.SpringBootApplication;  
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;  
  
@SpringBootApplication  
@EnableDiscoveryClient  
public class CouponserviceApplication {  
  
    public static void main(String[] args) {  
       SpringApplication.run(CouponserviceApplication.class, args);  
    }  
  
}
```

2. En `application.properties`:

```properties
spring.application.name=coupon-service  # o product-service
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
```

---

**Prueba en acción**

1. Ejecutar el **Eureka Server**.
2. Ejecutar el **Coupon Service** → aparecerá registrado en Eureka.
3. Ejecutar el **Product Service** → también aparecerá en Eureka.

Resultado:

- Ambos microservicios quedan registrados.
- Ahora pueden **descubrirse entre sí dinámicamente** a través de Eureka, sin necesidad de manejar manualmente URLs o puertos.

---

## 📝 Resumen estructurado

🔹 **Problema:**  
Múltiples instancias y servicios hacen difícil manejar URLs y puertos de forma manual.

🔹 **Solución:**  
Usar **Eureka Server** como registro central de microservicios.

🔹 **Configuración del servidor (8761):**

- Dependencia: `spring-cloud-starter-netflix-eureka-server`.
- Anotación: `@EnableEurekaServer`.
- Flags: `register-with-eureka=false`, `fetch-registry=false`.


🔹 **Configuración de clientes (Coupon y Product):**

- Dependencia: `spring-cloud-starter-netflix-eureka-client`.

- Anotación: `@EnableDiscoveryClient`.
- Propiedades:
   
```properties
spring.application.name=nombre-del-servicio 
eureka.client.service-url.defaultZone=http://localhost:8761/eureka
```
   

🔹 **Resultado esperado:**

- Coupon Service y Product Service se registran en Eureka.
    
- Los servicios se descubren entre sí mediante el nombre de aplicación.
    

✅ Esto desacopla los microservicios y simplifica la comunicación dinámica.

---