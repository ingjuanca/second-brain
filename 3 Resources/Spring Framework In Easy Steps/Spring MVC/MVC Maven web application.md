
---

En esta lección se construye una **aplicación web Spring MVC basada en Maven**, que luego se ejecutará y probará de forma completa.

---

## 🧱 **Paso 1: Crear el proyecto Maven**

1. En **Eclipse**, ir a:
    
    `File → New → Maven Project`
    
2. Seleccionar el **arquetipo web**, no el “quickstart”.
    
    - Usar: `maven-archetype-webapp`
        
3. Configurar:
    
    - **Group Id:** `com.bharath.spring`
        
    - **Artifact Id:** `springmvc`
        
4. Finalizar la creación. Esto genera una aplicación web Maven.
    

---

### 🧩 **Configuración inicial**

- Abrir el archivo `pom.xml`.
    
- Copiar desde otro proyecto (por ejemplo _springorm_) la sección desde `<properties>` hasta `<build>` y pegarla.
    
- Dejar solo una dependencia esencial:
    
```xml
<properties>
	<springframework.version>4.3.6.RELEASE</springframework.version>
</properties>
<dependencies>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-webmvc</artifactId>
		<version>${springframework.version}</version>
	</dependency>
</dependencies>
```
    
- Guardar y formatear (`Ctrl + Shift + F`).
    

---

### ⚙️ **Solución de error: javax.servlet.http.HttpServlet no encontrada**

El error ocurre porque el proyecto **no tiene la dependencia del Servlet API**.  
Hay dos maneras de corregirlo:

1. **Agregar la dependencia de Servlets en el `pom.xml`.**
    
2. **(Recomendada)** Configurar el **servidor Tomcat** en Eclipse:
    
    - Click derecho en el proyecto → **Properties** → Buscar _Targeted Runtimes_.
        
    - Seleccionar **Apache Tomcat** → OK.
        
    - Actualizar Maven: `Right click → Maven → Update Project`.
        

Una vez hecho, los errores desaparecerán.

---

### 🗂️ **Estructura del proyecto Maven web**

- `src/main/java` → Código fuente (controladores, lógica).
    
- `src/main/resources` → Archivos de configuración.
    
- `src/main/webapp/WEB-INF` → Contiene `web.xml` (descriptor de despliegue) y las vistas JSP.
    

Crear la carpeta `src/main/java` manualmente si no existe.

---

## ⚙️ **Paso 2: Configurar el Dispatcher Servlet**

Abrir `web.xml` y agregar:

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >
<web-app>
	<display-name>Hello Spring MVC</display-name>
	<servlet>
		<servlet-name>dispatcher</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet </servlet-class>
	</servlet>
	
	<servlet-mapping>
		<servlet-name>dispatcher</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
</web-app>
```

Esto indica que **todas las solicitudes ("/") serán manejadas por el DispatcherServlet**, el controlador frontal de Spring MVC.

---

## ⚙️ **Paso 3: Crear el archivo de configuración Spring**

En `WEB-INF`, crear el archivo de configuración de Spring con nombre:

`dispatcher-servlet.xml`

> 🔸 El nombre debe coincidir con el del servlet definido en `web.xml` (`dispatcher`) seguido de `-servlet.xml`.

Por ahora, dejarlo vacío dentro del elemento `<beans>`.

---

## ⚙️ **Paso 4: Configurar el View Resolver**

En `dispatcher-servlet.xml`, agregar:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p" xmlns:c="http://www.springframework.org/schema/c"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd
    http://www.springframework.org/schema/tx
    http://www.springframework.org/schema/tx/spring-tx.xsd">
    
	<context:component-scan base-package="com.bharath.spring.springmvc.controller" />
	<bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver"
		name="viewResolver">
		<property name="prefix">
			<value>/WEB-INF/views/</value>
		</property>
		<property name="suffix">
			<value>.jsp</value>
		</property>
	</bean>
</beans>
```

👉 Este bean:

- Añade un **prefijo** y un **sufijo** a las vistas devueltas por el controlador.
    
- Ejemplo: si el controlador retorna `"hello"`, Spring buscará `/WEB-INF/views/hello.jsp`.
    

---

## ⚙️ **Paso 5: Crear el controlador**

En `src/main/java/com/bharath/spring/springmvc/controller`, crear la clase:

```java
package com.bharath.spring.springmvc.controller;  
import org.springframework.stereotype.Controller; 
import org.springframework.web.bind.annotation.RequestMapping; 
import org.springframework.web.servlet.ModelAndView;  
@Controller public class HelloController {      
@RequestMapping("/hello")     
public ModelAndView hello() {         
	ModelAndView modelAndView = new ModelAndView();         
	modelAndView.setViewName("hello");         
	return modelAndView;     
	} 
}
```

📌 Explicación:

- `@Controller` indica que esta clase maneja solicitudes HTTP.
- `@RequestMapping("/hello")` vincula el método con la URL `/hello`.
- Retorna un `ModelAndView` con la vista `"hello"`.
    

---

## ⚙️ **Habilitar el escaneo de anotaciones**

En `dispatcher-servlet.xml`, agregar:

```xml
<context:component-scan base-package="com.bharath.spring.springmvc.controller" />
```

Esto permite que Spring detecte automáticamente las clases anotadas con `@Controller`.

---

## ⚙️ **Paso 6: Crear la vista JSP**

En `WEB-INF`, crear la carpeta `views`, y dentro de ella un archivo:

`hello.jsp`

Contenido básico:

```jsp
<html> 
	<head>
		<title>Hello</title>
	</head> 
	<body>     
		<h1>Hello from Spring MVC!</h1> 
	</body> 
</html>
```

---

## ▶️ **Paso 7: Ejecutar la aplicación**

1. Asegurarse de que en `dispatcher-servlet.xml` el prefijo tenga el **“/” final**:
    
    `/WEB-INF/views/`
    
2. Ejecutar:
    
    `Run As → Run on Server`
    
3. Acceder en el navegador a:
    
    `http://localhost:8080/springmvc/hello`
    

Resultado esperado:

> “**Hello from Spring MVC!**” — proveniente del archivo `hello.jsp`.

---

## 🔄 **Flujo interno de la aplicación**

|Etapa|Componente|Descripción|
|---|---|---|
|**1**|**Navegador**|Envia solicitud a `/hello`|
|**2**|**DispatcherServlet**|Intercepta la solicitud|
|**3**|**HandlerMapper**|Encuentra el controlador adecuado|
|**4**|**HelloController**|Ejecuta el método `hello()` y retorna `ModelAndView("hello")`|
|**5**|**ViewResolver**|Combina `/WEB-INF/views/` + `hello` + `.jsp`|
|**6**|**Vista JSP**|Renderiza la página con el mensaje|
|**7**|**Respuesta**|Se envía al navegador|

---

## ✅ **Conclusión general**

Esta práctica demuestra cómo crear desde cero una aplicación web **Spring MVC** con **Maven**, abarcando toda la arquitectura del framework:

1. **Proyecto Maven** estructurado correctamente.
    
2. **Configuración XML** del `DispatcherServlet` y `ViewResolver`.
    
3. **Controlador con anotaciones** (`@Controller`, `@RequestMapping`).
    
4. **Vista JSP** bajo `/WEB-INF/views`.
    
5. **Ejecución en Tomcat** mostrando el flujo completo MVC.
    

> 💡 En resumen:  
> Spring MVC organiza las aplicaciones web en tres capas desacopladas —Modelo, Vista y Controlador— y Maven automatiza las dependencias y estructura del proyecto, facilitando el desarrollo limpio, modular y profesional.