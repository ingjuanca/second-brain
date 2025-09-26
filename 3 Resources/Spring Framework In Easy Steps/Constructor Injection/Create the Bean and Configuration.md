
---

## Inyección de dependencias por **constructor** en Spring

Esta lección explica cómo **crear e inyectar beans** usando **constructores con parámetros**, una alternativa a la inyección por setters.

---

### 1️⃣ Crear el bean y la configuración inicial

- Spring permite crear objetos usando **constructores parametrizados**.
    
- En el XML se usa el elemento:
    
    `<constructor-arg>`
    
    para pasar los argumentos.
    
- Los argumentos pueden ser:
    
    - **Tipos primitivos** (`<value>`),
    - **Colecciones** (`<list>`, `<set>`, etc.),
    - **Tipos de referencia** (`<ref>`).
        
- Al igual que en la inyección por setters, se pueden usar:
    
    - **value** como **elemento** o como **atributo**,
        
    - **ref** como **elemento** o como **atributo**,
        
    - O el **C schema (C namespace)**, similar al **P schema** usado en property injection, para un XML más compacto.
        

---

### Paso a paso:

1. **Crear el paquete y las clases**
    
    - Paquete: `com.bharath.spring.springcore.constructorinjection`.
        
    - Reutilizar las clases `Employee` y `Address` de ejemplos anteriores.
        
2. **Modificar `Employee`**:
    
    - Definir un **constructor con parámetros**:
        
        `public Employee(int id, Address address) {     this.id = id;     this.address = address; }`
        
    - Se puede mantener o eliminar los setters, pero **no se usarán para la inyección**.

```java
package com.bharath.spring.springcore.constructorinjection;

public class Employee {

	private int id;
	private Address address;

	Employee(int id, Address address){
		this.id = id;
		this.address = address;
	}
	
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

```java
package com.bharath.spring.springcore.constructorinjection;

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

1. **Configurar el bean `Address`** en `config.xml` usando P schema (property injection):
    
```xml
<bean class="com.bharath.spring.springcore.constructorinjection.Address"
		name="address" p:hno="123" p:street="mira mesa" p:city="san diego" />
```
    
2. **Configurar el bean `Employee`** usando constructor injection:
    
```xml
<bean name="employee" class="com.bharath.spring.springcore.constructorinjection.Employee">     
	<constructor-arg value="123"/>     
	<constructor-arg ref="address"/> 
</bean>
```
    
    _El primer argumento es un valor primitivo (id), el segundo es un objeto (address)._
    
3.  Otra forma de configurar el bean *Employee* es usando el construtor C schema (C namespace)
```xml
<bean class="com.bharath.spring.springcore.constructorinjection.Employee"
		name="employee" c:id="123" c:address-ref="address"/>
```
---

### 2️⃣ Crear y ejecutar la prueba

- Clase `Test`:
    
    `ApplicationContext context =     new ClassPathXmlApplicationContext(         "com/bharath/spring/springcore/constructorinjection/config.xml"); Employee e = (Employee) context.getBean("employee"); System.out.println(e);`
    
- Al ejecutar:
    
    - Se imprime el `Employee` con:
        
        `House number: 123 Street: mira mesa City: san diego`
        
- Se demuestra la **inyección por constructor**, mientras que `Address` recibió sus datos mediante **property injection**.  
    _(Ejemplo de combinación de ambos tipos de inyección en un mismo XML)._
    

---

### 3️⃣ Variantes en la configuración XML

Spring permite varias formas de declarar los argumentos:

1. **Como elemento `<value>`** (ya visto arriba).
    
2. **Como atributo `value`** en `<constructor-arg>`:
    
```xml
    <constructor-arg value="123"/> 
    <constructor-arg ref="address"/>
```
    
3. **Usando C schema (C namespace)** para un XML más compacto:
    
    - Añadir el namespace `c` en `<beans>`:
        
        `xmlns:c="http://www.springframework.org/schema/c"`
        
    - Usar atributos con prefijo `c:`:
        
```xml
<bean name="employee" class="com.bharath.spring.springcore.constructorinjection.Employee" c:id="123" c:address-ref="address"/>
```
        
- Esto es similar al **p namespace**, pero para **constructor injection**.
        
- En todos los casos el resultado es el mismo: Spring inyecta las dependencias a través del **constructor parametrizado**.
    

---

**Resumen completo del documento:**

La **inyección por constructor** permite que Spring cree e inyecte beans llamando a un **constructor con parámetros**, en lugar de usar setters.

**Pasos principales:**

1. Crear un constructor en la clase principal (`Employee`) que reciba como parámetros los valores y dependencias.
    
2. Configurar el bean en el XML usando `<constructor-arg>` para pasar:
    
    - valores primitivos (`value`),
        
    - referencias a otros beans (`ref`).
        
3. Ejecutar la prueba para comprobar que Spring crea el objeto `Employee` con el `Address` inyectado.
    

**Formas de declarar los argumentos:**

- `value`/`ref` como elemento,
    
- `value`/`ref` como atributo,
    
- o con **C schema (c:id, c:address-ref)** para un XML más breve.
    

Este enfoque es flexible y permite **combinar property injection y constructor injection en un mismo archivo de configuración**.