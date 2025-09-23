
---
### Paso 1: Crear el **Java Bean**

Con el proyecto Maven ya creado en Eclipse, la **inyección de dependencias** se hará en **tres pasos**.

1. **Crear la clase POJO (Java Bean)**:
    
    - En Eclipse, dentro del paquete del proyecto, crear una nueva clase llamada **`Employee`**.
        
    - Agregar dos campos privados:
        
        - `private int id;`
            
        - `private String name;`
            
    - Usar **Ctrl + 1** para generar automáticamente **getter y setter** para ambos campos.
        
    - Formatear el código con **Ctrl + Shift + F**.
        

Este será el **Employee Bean** que recibirá las dependencias que inyectará Spring.

```java
package com.bharath.spring.springcore;

public class Employee {

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

}
```

_(La clase `App.java` que viene por defecto en el proyecto Maven se puede eliminar porque no se usará)._

---

### Paso 2: Crear la **Configuración de Spring**

Ahora se debe indicar al **contenedor de Spring**:

- Cuál es el **POJO (Employee)**,
    
- Y cuáles son las **dependencias** que se inyectarán.
    

Para esto:

1. Crear un **archivo XML de configuración**, por ejemplo **`config.xml`** (el nombre puede variar).
    
    - Ir a `src/main/java` → **New → Other → XML File** y nombrarlo `config.xml`.
        
    - El archivo debe tener como elemento raíz `<beans>` con los **namespaces** de Spring.
        
2. Definir un **bean**:
    
    - Usar el elemento `<bean>` con un **atributo `name`** (por ejemplo, `"emp"`) que servirá para referenciarlo después en el código de prueba.
        
    - Especificar el nombre completo de la clase, incluyendo el **paquete**, por ejemplo:
        
        `<bean name="emp" class="com.ejemplo.Employee">`
        
3. **Inyectar las dependencias**:
    
    - Para el campo `id` (tipo `int`):
        
        `<property name="id">     <value>20</value> </property>`
        
    - Para el campo `name` (tipo `String`):
        
        `<property name="name">     <value>bharath</value> </property>`
        
    - Formatear el XML (Ctrl + Shift + F).

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">
    
	<bean name="emp" class="com.bharath.spring.springcore.Employee">
		<property name="id">     
			<value>20</value> 
		</property>
		<property name="name">     
			<value>bharath</value> 
		</property>
	</bean>
</beans>
```

Con esto, se le indica a Spring que debe **crear el bean `Employee`** e **inyectar los valores** en sus propiedades `id` y `name`.

---

### Paso 3: Crear y ejecutar la prueba

En la siguiente lección se creará una **clase de prueba**, que:

- **Obtendrá el bean `Employee`** desde el archivo `config.xml`,
    
- **Verificará la inyección de dependencias** (los valores de `id` y `name`).

```java
package com.bharath.spring.springcore;

import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {
		ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("com/bharath/spring/springcore/config.xml");
		Employee emp = (Employee) ctx.getBean("emp");
		System.out.println("Employee ID: " + emp.getId());
		System.out.println("Employee Name: " + emp.getName());
	}

}

```

---

**Resumen completo del documento:**

El proceso para implementar **inyección de dependencias en Spring** con un proyecto Maven en Eclipse consta de tres etapas:

1. **Creación del Java Bean (`Employee`)**: clase POJO con dos propiedades (`id` y `name`) y sus métodos getter y setter.
    
2. **Archivo de configuración `config.xml`**:
    
    - Elemento raíz `<beans>` con namespaces de Spring.
        
    - Definición de un `<bean>` para `Employee` con un **nombre único** (`EMP`) y la ruta completa de su clase.
        
    - Uso de `<property>` y `<value>` para **inyectar valores primitivos** (`id=20` y `name="TuNombre"`).
        
3. **Prueba de inyección**: se implementará una clase de prueba que recuperará el bean `Employee` del contenedor Spring para comprobar que las dependencias se inyectaron correctamente.
    

Este ejercicio prepara el entorno para comenzar a trabajar con **inyección de dependencias en Spring**.