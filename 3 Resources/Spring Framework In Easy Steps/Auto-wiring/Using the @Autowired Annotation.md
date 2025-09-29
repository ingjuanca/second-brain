
---

## Uso de la anotación **@Autowired** en Spring

Esta lección enseña a realizar **autowiring con anotaciones**, en lugar de usar el atributo `autowire` en el XML.

---

### 1️⃣ Preparación del paquete

- Crear un nuevo paquete:
    
    `com.bharath.spring.springcoreadvanced.autowiring`
    
- Copiar en este paquete:
    
    - `Address.java`
```java
package com.bharath.spring.springcoreadvanced.autowiring.annotations;

public class Address {
	
	private int hno;
	private String street;
	private String city;	

	public int getHno() {
		return hno;
	}

	public void setHno(int hno) {
		this.hno = hno;
	}

	public String getStreet() {
		return street;
	}

	public void setStreet(String street) {
		this.street = street;
	}

	public String getCity() {
		return city;
	}

	public void setCity(String city) {
		this.city = city;
	}

	@Override
	public String toString() {
		return "Address [hno=" + hno + ", street=" + street + ", city=" + city + "]";
	}	

}

```
    - `Employee.java`

```java
package com.bharath.spring.springcoreadvanced.autowiring.annotations;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;

public class Employee {

	@Override
	public String toString() {
		return "Employee [address=" + address + "]";
	}

	public Address getAddress() {
		return address;
	}

	@Autowired()
	private Address address;

}

```

    - `Test.java`

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

    - `config.xml`

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
			
	<bean class="com.bharath.spring.springcoreadvanced.autowiring.annotations.Employee"
		name="employee"/>

</beans>
```

- En `config.xml` **eliminar el atributo `autowire`**, porque se usará la anotación `@Autowired`.
    

---

### 2️⃣ Autowiring con `@Autowired` en el **setter**

- En la clase `Employee`, en el método `setAddress`, agregar:
    
```java
@Autowired 
public void setAddress(Address address) { ... }
```
    
(la anotación proviene de `org.springframework.beans.factory.annotation.Autowired`).
    
- Se puede eliminar el constructor, porque se está usando **setter injection**.  
    Aunque no es obligatorio, se puede dejar para usarlo más tarde.
    
        
- En `config.xml`:
    
    - Habilitar las anotaciones agregando:
        
        `<context:annotation-config/>`
        
        (y asegurarse de tener el `xmlns:context` en la cabecera).
        
- Corregir en el XML las rutas de las clases para que incluyan el paquete `autowiring.annotations`.
    
- Ejecutar la prueba: la salida es correcta, demostrando que la dependencia `Address` fue inyectada en `Employee` mediante **@Autowired en el setter**.
    

---

### 3️⃣ @Autowired en el **campo (field)**

- Se puede mover la anotación `@Autowired` directamente al atributo:
    
```java
@Autowired 
private Address address;
```
    
- En este caso:
    
    - Se puede eliminar el método setter.
        
    - Spring inyectará la dependencia directamente en el campo.
        
- Al ejecutar la prueba de nuevo, el resultado es el mismo.
    

---

### 4️⃣ @Autowired en el **constructor**

- Se puede colocar la anotación en el **constructor**:
    
```java
@Autowired 
public Employee(Address address) {     
	this.address = address; 
}
```
    
- Con esto:
    
    - Spring realiza **inyección por constructor** automáticamente.
        
- Al ejecutar la prueba, todo funciona igual.
    

---

### Notas clave

- Para usar `@Autowired` es **obligatorio habilitar el soporte de anotaciones** en el `config.xml` con:
    
    `<context:annotation-config/>`
    
- No se requiere ningún otro cambio en el XML.
    
- Spring inyecta la dependencia automáticamente:
    
    - **en el setter**,
        
    - **en el campo**,
        
    - o **en el constructor**, según dónde esté colocada la anotación.
        

---

**Resumen completo del documento:**

El documento explica cómo realizar **autowiring con anotaciones** en Spring usando **@Autowired**:

1. **Preparar el paquete** y copiar las clases y el archivo de configuración.
    
2. **Eliminar el atributo `autowire` en el XML** y habilitar las anotaciones con `<context:annotation-config/>`.
    
3. **Colocar `@Autowired`**:
    
    - En el **setter** (setter injection),
        
    - En el **campo** (field injection),
        
    - O en el **constructor** (constructor injection).
        
4. Ejecutar la prueba tras cada cambio para verificar el correcto funcionamiento.
    

Con `@Autowired`, Spring inyecta automáticamente las dependencias sin necesidad de `<ref>` en el XML.  
La elección del nivel (setter, campo o constructor) depende de las necesidades:

- **Constructor**: cuando la dependencia es obligatoria.
    
- **Campo**: cuando se quiere inyección directa.
    
- **Setter**: para mayor flexibilidad o dependencias opcionales.
    

Así, **@Autowired simplifica la configuración** y elimina la necesidad de wiring manual en XML.