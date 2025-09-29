
---

## Autoconexión (Autowiring) **por constructor** en Spring

La última opción de **autowiring basado en XML** es usar **constructor**.

En los modos anteriores (`byType` y `byName`) el contenedor Spring realizaba la **inyección de dependencias mediante setters** (property-based injection).  
Ahora se demostrará cómo hacerlo con **inyección por constructor**.

---

### Pasos en Eclipse

1. **Agregar un constructor con parámetros**
    
    - En la clase `Employee` crear un constructor que reciba como parámetro el objeto de la dependencia, por ejemplo:
        
```java
package com.bharath.spring.springcoreadvanced.autowiring;

public class Employee {

	Employee(Address address) {
		this.address = address;
	}

	@Override
	public String toString() {
		return "Employee [address=" + address + "]";
	}

	public Address getAddress() {
		return address;
	}

	public void setAddress(Address address) {
		this.address = address;
	}

	private Address address;

}

```
        
2. **Limpiar beans duplicados**
    
    - En el archivo `config.xml`, eliminar el bean duplicado creado en la lección anterior (`address1`).
        
3. **Configurar el autowiring en el XML**
    
    - En la definición del bean `employee`, cambiar:
        
        `autowire="byName"`
        
        por:
        
        `autowire="constructor"`

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


	<bean class="com.bharath.spring.springcoreadvanced.autowiring.Address"
		name="address" p:hno="123" p:street="mira mesa" p:city="san diego" />
		
	<bean class="com.bharath.spring.springcoreadvanced.autowiring.Employee"
		name="employee" autowire="constructor"/>

</beans>
```

    - Esto indica al contenedor de Spring que debe realizar la inyección **usando el constructor**.
        
4. **Ejecutar la prueba**
    
    - En la clase `Test`, ejecutar como **Java Application**.
        
    - El contenedor de Spring:
        
        - Lee el archivo de configuración,
            
        - Detecta que el bean `employee` debe usar **constructor injection**,
            
        - Busca en la configuración un bean que coincida con el **tipo de parámetro** del constructor (`Address`),
            
        - Crea la instancia de `Address`,
            
        - Invoca el **constructor de `Employee`**, inyectando automáticamente la dependencia.
            

---

### Funcionamiento interno

- Spring identifica que el autowiring es por **constructor**,
    
- Analiza el **tipo de argumento** que requiere el constructor,
    
- Localiza un bean de ese **mismo tipo** en la configuración,
    
- Lo instancia e invoca el constructor de `Employee` para completar la inyección.
    

---

**Resumen completo del documento:**

La lección enseña la tercera modalidad de **autowiring en XML**: **`autowire="constructor"`**.

- A diferencia de `byType` y `byName`, que usan **setter injection**, aquí Spring realiza **inyección por constructor**.
    
- Se debe agregar un **constructor con parámetros** en la clase que reciba el bean dependiente.
    
- En el `config.xml`, se especifica `autowire="constructor"`.
    
- Spring entonces:
    
    1. Lee la configuración del bean,
        
    2. Verifica el tipo de parámetro requerido en el constructor,
        
    3. Busca un bean en el contexto que coincida con ese tipo,
        
    4. Crea la instancia y ejecuta el constructor para inyectar la dependencia.
        

De esta forma, el contenedor de Spring hace la **inyección de dependencias a través del constructor de manera automática**, sin necesidad de `<constructor-arg>` explícitos en el XML.