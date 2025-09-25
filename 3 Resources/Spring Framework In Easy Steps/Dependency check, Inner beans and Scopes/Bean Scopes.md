
---

## Scopes en acción

Para entenderlo, se reutilizan las clases del ejemplo de **inner beans** (`Employee`, `Address`, `Test` y `config.xml`).

### 1️⃣ Comportamiento por defecto: Singleton

- En `config.xml` no se define ningún scope, por lo que el bean es **singleton**.
    
- En `Test.java` se obtienen **dos veces** el bean `employee`:
    
    `Employee employee1 = (Employee) context.getBean("employee");` 
    `Employee employee2 = (Employee) context.getBean("employee");`
    
- Se imprime el **hash code** de cada objeto:
    
    `System.out.println(employee1.hashCode());` `System.out.println(employee2.hashCode());`
    
- El **hash code** es un identificador único de la instancia en memoria.
    
- Ambos hash codes son **idénticos**, lo que demuestra que **Spring devuelve siempre la misma instancia**.
    

---

### 2️⃣ Cambiar a Prototype

- En `config.xml` modificar el bean y definir `scope="prototype"`:
    
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">

	<bean class="com.bharath.spring.springcore.innerbeans.Employee"
		name="employee" p:id="123" scope="prototype">
		<property name="address">
			<bean class="com.bharath.spring.springcore.innerbeans.Address"
				p:hno="700" p:street="Mira Mesa Blvd" p:city="San Diego"/>
		</property>
	</bean>

</beans>
```
    
- Ejecutar la prueba:
    
    - Los **hash codes** son **diferentes**, porque ahora Spring **crea un nuevo objeto cada vez** que se solicita el bean.
        

```java
package com.bharath.spring.springcore.innerbeans;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcore/innerbeans/config.xml");
		Employee employee = (Employee) context.getBean("employee");
		System.out.println(employee.hashCode());
		
		Employee employee2 = (Employee) context.getBean("employee");
		System.out.println(employee2.hashCode());
	
	}

}
```

`362423424`
`234234234`


---

### 3️⃣ Volver a Singleton

- Si se cambia de nuevo a `scope="singleton"`:
    

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">

	<bean class="com.bharath.spring.springcore.innerbeans.Employee"
		name="employee" p:id="123" scope="singleton">
		<property name="address">
			<bean class="com.bharath.spring.springcore.innerbeans.Address"
				p:hno="700" p:street="Mira Mesa Blvd" p:city="San Diego"/>
		</property>
	</bean>

</beans>
```

    
    (o se omite el atributo),  
    los **hash codes vuelven a ser iguales**, confirmando que existe **una sola instancia**.
    

`362423424`
`362423424`


---

**Resumen completo del documento:**

El **scope** de un bean en Spring determina **cuántas instancias crea y administra el contenedor**:

- **Singleton (por defecto):** una sola instancia en toda la aplicación.
    
- **Prototype:** una nueva instancia cada vez que se solicita el bean.
    
- **Request:** una instancia por cada petición HTTP (solo en Spring MVC web).
    
- **Session:** una instancia por sesión de usuario (solo en Spring MVC web).
    
- **Global Session:** una instancia global para todos los portlets en aplicaciones Portlet.
    

En la práctica:

- Se demuestra que **por defecto** los beans son **singleton**, ya que dos solicitudes devuelven el **mismo hash code**.
    
- Al cambiar a **prototype**, cada solicitud genera un **hash code distinto**, mostrando que Spring crea una **nueva instancia** cada vez.
    

Esta demostración ilustra cómo el **scope** controla la **cantidad de objetos creados y su ciclo de vida** dentro de una aplicación Spring.