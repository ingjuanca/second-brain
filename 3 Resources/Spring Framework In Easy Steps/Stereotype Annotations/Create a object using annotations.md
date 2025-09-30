
---

## Creación de un objeto usando **@Component** en Spring

En esta lección se explica cómo crear un objeto en Spring mediante **anotaciones estereotipo**, sin necesidad de declarar `<bean>` en el XML.

---

### Pasos principales

1. **Crear la clase**
    
    - Clase: `Instructor` (o `Teacher`).
        
    - Paquete: `com.bharath.springcoreadvanced.stereotype.annotations`.
        
    - Atributos: `int id`, `String name`.
        
    - Generar getters, setters y `toString()`.

```java
package com.bharath.spring.springcoreadvanced.stereotype.annotations;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

@Component
public class Instructor {

	private int id;
	private String name;

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

	@Override
	public String toString() {
		return "Instructor [id=" + id + ", name=" + name + "]";
	}

}

```

2. **Marcar la clase con `@Component`**
    
    - Agregar la anotación `@Component` (de `org.springframework.stereotype.Component`).
        
    - Esto indica al contenedor de Spring que debe crear un objeto de esta clase.
        
3. **Crear el archivo de configuración (`config.xml`)**
    
    - Incluir solo:
        
        `<context:component-scan base-package="com.bharath.springcoreadvanced.stereotype.annotations"/>`

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

</beans>
```

    - `component-scan` le dice a Spring **qué paquetes debe escanear** para buscar clases con `@Component`.
        
    - No es necesario usar `context:annotation-config`, porque `component-scan` ya lo habilita internamente.
        
4. **Crear la clase de prueba (`Test`)**
    
    - Obtener el bean con:
        
        `Instructor instructor = context.getBean("instructor", Instructor.class);`
        
    - Recordar: Spring por defecto usa el **nombre de la clase en camelCase** como id del bean.
        
        - Ejemplo: `Instructor` → `instructor`.
            
    - Al ejecutar, se obtiene el objeto con valores por defecto.
        
```java
package com.bharath.spring.springcoreadvanced.stereotype.annotations;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcoreadvanced/stereotype/annotations/config.xml");
		Instructor instructor = (Instructor) context.getBean("instructor");
		System.out.println(instructor);
		
	}

}

```
---

### Personalizar el nombre del bean

- Por defecto, Spring usa el nombre en **camelCase**.
    
    - Ejemplo: `Instructor` → `instructor`.
        
- Se puede sobrescribir el nombre del bean en la anotación:
    
    `@Component("inst") public class Instructor { ... }`

```java
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

@Component("inst")
public class Instructor {

	private int id;
	private String name;

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

	@Override
	public String toString() {
		return "Instructor [id=" + id + ", name=" + name + "]";
	}

}
```

- En este caso, en el test se debe usar:
    
    `context.getBean("inst", Instructor.class);`

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
		
	}

}

```

- Si se intenta usar `"instructor"`, Spring lanza:
    
    `NoSuchBeanDefinitionException: No bean named 'instructor' available`
    

---

### Resumen completo del documento

El documento enseña cómo **crear objetos con anotaciones en Spring**:

1. Crear una clase (`Instructor`) con atributos, getters/setters y `toString()`.
    
2. Marcarla con `@Component` para que Spring la gestione automáticamente como un bean.
    
3. Configurar el archivo XML con `<context:component-scan>` para indicar a Spring qué paquetes debe escanear.
    
    - Esto habilita también el uso de anotaciones.
        
4. Crear una clase de prueba para obtener el objeto del contenedor.
    
5. El **nombre por defecto del bean** es el de la clase en camelCase (`Instructor` → `instructor`).
    
6. Si se quiere un nombre diferente, se puede definir en `@Component("nombre")`.
    

En conclusión, **@Component junto con component-scan elimina la necesidad de declarar beans manualmente en XML**, permitiendo a Spring detectar y gestionar automáticamente los objetos de nuestra aplicación. ✅