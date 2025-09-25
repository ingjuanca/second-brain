
---

## Beans Internos (Inner Beans) – Creación y Configuración

En esta lección se muestra cómo **configurar y usar beans internos (inner beans)** en Spring.

### Caso de uso: Relación **Empleado – Dirección**

- La clase **Employee** tiene un **ID** y una relación **HAS-A** con una clase **Address**.
    
- `Address` contiene:
    
    - `hNumber` (número de casa, int),
        
    - `street` (String),
        
    - `city` (String).
        

---

### Pasos de implementación:

1. **Crear las clases**
    
    - Paquete: `com.bharath.springcore.innerbeans`.
        
    - Clase `Employee`:
        
        `private int id; private Address address;`
        
        Generar getters y setters.

```java
package com.bharath.spring.springcore.innerbeans;

public class Employee {

	private int id;
	private Address address;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public Address getAddress() {
		return address;
	}

	public void setAddress(Address address) {
		this.address = address;
	}

	@Override
	public String toString() {
		return "Employee [id=" + id + ", address=" + address + "]";
	}

}

```

    - Crear la clase `Address` en el mismo paquete:
        
        `private int hno;` 
        `private String street;`
        `private String city;`
        
        Generar getters y setters.
        
    - En ambas clases, crear también el método `toString()` para imprimir fácilmente los valores.

```java
package com.bharath.spring.springcore.innerbeans;

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

---

### Configuración XML

En el archivo `config.xml`:

- Definir el bean `employee` con el **namespace p**:
    
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
		name="employee" p:id="123">
		<property name="address">
			<bean class="com.bharath.spring.springcore.innerbeans.Address"
				p:hno="700" p:street="Mira Mesa Blvd" p:city="San Diego"/>
		</property>
	</bean>

</beans>
```
    
- Aquí, **Address** se declara como un **inner bean**:
    
    - Se define **dentro** de la propiedad `address` de `Employee`.
        
    - **No necesita nombre**, porque no se reutiliza fuera de `Employee`.
        

---

## Beans Internos – Prueba

1. Crear una clase `Test` con un método `main`:
    
```java
package com.bharath.spring.springcore.innerbeans;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcore/innerbeans/config.xml");
		Employee employee = (Employee) context.getBean("employee");
		System.out.println(employee);
		
	}

}

```
    
2. La salida muestra el **objeto Employee** con su **Address** correctamente inyectado.
    

---

### Conclusión

Spring ha:

- **Instanciado el inner bean `Address`**,
    
- **Asignado la instancia a la propiedad `address` de `Employee`**,
    
- Y el `toString()` de `Employee` muestra el objeto completo.
    

---

### Cuatro formas de inyectar un objeto en Spring:

1. **ref como elemento**,
    
2. **ref como atributo**,
    
3. **p schema**,
    
4. **inner bean** (este ejemplo).
    

Los **inner beans**:

- Solo se usan para **tipos de referencia**,
    
- Y cuando existe una **relación directa** entre el bean principal y el bean dependiente,
    
- Por ejemplo: `Employee` **tiene una** `Address`.
    

---

**Resumen completo del documento:**

El documento enseña cómo **configurar un bean interno (inner bean) en Spring**:

1. Crear dos clases con relación HAS-A: `Employee` y `Address`.
    
2. Configurar el bean `Employee` en el XML y declarar el bean `Address` **dentro de la propiedad `address`** como inner bean (sin nombre).
    
3. Crear un `Test` que cargue el contexto, obtenga el bean `employee` y muestre su información.
    

Los **inner beans** son una de las **cuatro formas principales de inyectar dependencias de tipo objeto** en Spring y se usan cuando el bean dependiente **solo se requiere dentro de otro bean**, no de forma independiente.
