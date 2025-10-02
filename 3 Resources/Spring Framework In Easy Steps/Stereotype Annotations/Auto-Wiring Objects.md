
---

## Autowiring de objetos (referencias) con anotaciones en Spring

En esta lección se muestra cómo inyectar **objetos de tipo referencia** en un bean de Spring utilizando **@Autowired**, además de las inyecciones de tipos primitivos y colecciones vistas previamente.

---

### 1️⃣ Crear la clase dependiente (`Profile`)

- En la clase `Instructor` se agrega una dependencia:
    
    `private Profile profile;`
    
- No se debe usar la clase `Profile` del framework Spring; se debe **crear una nueva clase** `Profile`.
    
- `Profile` contiene dos campos `String`:
    
    - `title` (título del instructor),
        
    - `company` (nombre de la compañía).
        
- Se generan **getters, setters y `toString()`**.
    

Ejemplo:

```java
package com.bharath.spring.springcoreadvanced.stereotype.annotations;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class Profile {

	@Value("Java Architect and Instructor")
	private String title;
	@Value("Vivekananda Consulting")
	private String company;

	@Override
	public String toString() {
		return "Profile [title=" + title + ", company=" + company + "]";
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getCompany() {
		return company;
	}

	public void setCompany(String company) {
		this.company = company;
	}

}
```

- `@Component`: le dice a Spring que esta clase debe convertirse en un **bean gestionado**.
    
- `@Value`: asigna valores iniciales a los atributos.
    

---

### 2️⃣ Inyectar la dependencia en `Instructor`

En la clase `Instructor`:

`@Autowired private Profile profile;`

- `@Autowired` le indica a Spring que debe **inyectar automáticamente un objeto `Profile`** cuando cree un `Instructor`.
    
- Esto funciona porque `Profile` también está marcado con `@Component`.
    

```java
package com.bharath.spring.springcoreadvanced.stereotype.annotations;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

@Component("inst")
@Scope("prototype")
public class Instructor {

	@Value("#{T(java.lang.Integer).MIN_VALUE}")
	private int id = 15;
	@Value("#{new java.lang.String('Bharath Thippireddy')}")
	private String name = "John";

	@Value("#{topics}")
	private List<String> topics;
	
	@Value("#{2+4>8?true:false}")
	private boolean active;

	@Autowired
	private Profile profile;

	@Override
	public String toString() {
		return "Instructor [id=" + id + ", name=" + name + ", topics=" + topics + ", active=" + active + ", profile="
				+ profile + "]";
	}

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

}

```

---

### 3️⃣ Configuración en `config.xml`

En el archivo de configuración se agrega:

`<context:component-scan base-package="com.bharath.springcoreadvanced.stereotype.annotations"/>`

- `component-scan` instruye a Spring para que **escanee el paquete base y sus subpaquetes**.
    
- Encuentra todas las clases anotadas con `@Component` (`Instructor` y `Profile`), las instancia y las gestiona como beans.
    
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p" xmlns:c="http://www.springframework.org/schema/c"
	xmlns:util="http://www.springframework.org/schema/util"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd
     http://www.springframework.org/schema/util
    http://www.springframework.org/schema/util/spring-util.xsd">

	<context:component-scan base-package="com.bharath.spring.springcoreadvanced.stereotype.annotations"/>
	
	<util:list list-class="java.util.LinkedList" id="topics">
		<value>Java Web Services</value>
		<value>Core java</value>
		<value>XSLT</value>
	</util:list>

</beans>
```

---

### 4️⃣ Ejecución de la prueba

- Al ejecutar la aplicación de prueba:
    
    - Spring crea los beans `Instructor` y `Profile`.
        
    - Inyecta automáticamente el objeto `Profile` dentro de `Instructor`.
        
    - Gracias al `toString()` actualizado en `Instructor`, se puede ver el objeto `Profile` con sus valores (`title`, `company`) en la salida.
        
```java
package com.bharath.spring.springcoreadvanced.stereotype.annotations;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcoreadvanced/stereotype/annotations/config.xml");
		Instructor instructor = (Instructor) context.getBean("inst");
		System.out.println(instructor);
		
		Instructor instructor2 = (Instructor) context.getBean("inst");
		System.out.println(instructor2);
	}

}

```


---

### Resultado

El objeto `Instructor` ahora contiene:

- Sus valores primitivos (`id`, `name`),
    
- Su colección (`topics`),
    
- Y un **objeto dependiente (`Profile`)** inyectado automáticamente con sus propios valores.
    

---

## **Resumen completo del documento**

El documento explica cómo usar **@Autowired** para inyectar objetos dependientes en un bean de Spring:

1. Se crea una clase dependiente (`Profile`) y se marca con `@Component`.
    
2. Se asignan valores a sus atributos con `@Value`.
    
3. En la clase principal (`Instructor`), se añade el atributo `Profile` y se marca con `@Autowired`.
    
4. En el `config.xml`, se configura `<context:component-scan>` para que Spring detecte las clases con anotaciones.
    
5. Al ejecutar la prueba, Spring:
    
    - Escanea los paquetes,
        
    - Crea los beans de `Instructor` y `Profile`,
        
    - Inyecta automáticamente el objeto `Profile` en `Instructor`,
        
    - Y muestra los datos gracias a `toString()`.
        

Esto demuestra el **poder de las anotaciones**, que permiten realizar inyección de dependencias sin necesidad de escribir mucho XML: basta con `@Component`, `@Autowired`, `@Value` y `component-scan`. ✅