
---

## Uso de la anotación **@Qualifier** en Spring

Esta lección explica cómo utilizar la anotación **@Qualifier** junto con **@Autowired** para **especificar exactamente cuál bean inyectar** cuando hay más de uno del mismo tipo.

---

### Problema inicial

- Cuando se usa `@Autowired` y existen **varios beans del mismo tipo**, Spring **no sabe cuál elegir**.

```xml
<bean class="com.bharath.spring.springcoreadvanced.autowiring.annotations.Address"
		name="address123" p:hno="123" p:street="mira mesa" p:city="san diego" />
		
<bean class="com.bharath.spring.springcoreadvanced.autowiring.annotations.Address"
	name="address2" p:hno="567" p:street="mira mesa" p:city="san diego" />
```

- Si no se indica nada más, Spring lanza una excepción:
    
```bash
No qualifying bean of type 'Address' available
```
    
    (_NoSuchBeanDefinitionException_).
    

---

### Ejemplo práctico

1. En `config.xml` se definen **dos beans de tipo `Address`**:
    
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:c="http://www.springframework.org/schema/c"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">

	<context:annotation-config/>

	<bean class="com.bharath.spring.springcoreadvanced.autowiring.annotations.Address"
		name="address123" p:hno="123" p:street="mira mesa" p:city="san diego" />
		
	<bean class="com.bharath.spring.springcoreadvanced.autowiring.annotations.Address"
		name="address2" p:hno="567" p:street="mira mesa" p:city="san diego" />
		
		
	<bean class="com.bharath.spring.springcoreadvanced.autowiring.annotations.Employee"
		name="employee"/>

</beans>
```
    
2. En `Employee.java` existe:
    
```java
@Autowired 
private Address address;
```
    
    _Si ejecutamos la prueba, Spring lanza excepción porque hay **dos beans** de tipo `Address` y no sabe cuál inyectar._
    

---

### Solución: usar **@Qualifier**

Para indicar el **nombre exacto del bean** que se debe inyectar:

`@Autowired @Qualifier("address123") private Address address;`

- Importar `@Qualifier` desde:
    
    `org.springframework.beans.factory.annotation.Qualifier`
    
- Con esto Spring buscará específicamente el bean cuyo **id sea `address123`**.
    

```java
package com.bharath.spring.springcoreadvanced.autowiring.annotations;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;

public class Employee {

	@Qualifier("address123")
	private Address address;
	
	public Address getAddress() {
		return address;
	}
	
	@Override
	public String toString() {
		return "Employee [address=" + address + "]";
	}

}

```

---

### Pruebas

- Ejecutar la prueba:
    
    - Spring inyecta el bean `address123`.
        
    - El objeto `Employee` muestra que tiene el **hNumber = 123**.

```java
package com.bharath.spring.springcoreadvanced.autowiring.annotations;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcoreadvanced/autowiring/annotations/config.xml");
		Employee e = (Employee) context.getBean("employee");
		System.out.println(e);

	}

}

```

- Cambiar el valor en `@Qualifier` a `"address2"`:
    
    - Spring ahora inyecta el bean con **hNumber = 567**.

- Cambiar a un id inexistente, por ejemplo `"789"`:
    
    - Spring lanza:
        
        `No qualifying bean of type 'Address' available`
        
        (_NoSuchBeanDefinitionException_).
        

---

### Opción para evitar la excepción

- Si se quiere que **no falle cuando el bean no existe**, se puede usar **@Autowired(required=false)**:
    
```java
    @Autowired(required=false) 
    @Qualifier("789") 
    private Address address;
```
    
- Al ejecutar:
    
    - Spring **no encuentra el bean**,
        
    - Inyecta **null** en la propiedad,
        
    - La aplicación continúa sin lanzar excepción.
        

---

### Resumen completo del documento

La anotación **@Qualifier** se usa junto con **@Autowired** para **resolver conflictos** cuando hay **múltiples beans del mismo tipo**:

1. **@Autowired** por sí sola no puede decidir cuál bean inyectar si hay más de uno del mismo tipo.
    
2. **@Qualifier("beanId")** indica explícitamente a Spring **qué bean inyectar**.
    
3. Si el bean con ese id **no existe**, Spring lanza **NoSuchBeanDefinitionException**.
    
4. Para evitar la excepción, se puede usar `@Autowired(required=false)`, en cuyo caso Spring inyecta `null` si no encuentra el bean.
    

En resumen, **@Qualifier** permite **especificar el nombre exacto del bean** que Spring debe inyectar, garantizando que la dependencia correcta se asigne incluso cuando existen varios beans del mismo tipo en el contexto.