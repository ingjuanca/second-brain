
---

## Introducción a la **Auto-conexión (Auto-Wiring)** en Spring

Cuando un **objeto A depende de un objeto B** para realizar su trabajo:

- **B es la dependencia**,
- **A es el objeto dependiente**.
    

Hasta ahora se había aprendido a **inyectar dependencias** con:

- **inyección por setter**, o
- **inyección por constructor**,
    

usando la etiqueta `<ref>` en el `config.xml`.  
Este proceso de enlazar los objetos se llama **“wiring”** (cableado o conexión de beans).

- Si el programador hace el wiring, se llama **bean wiring manual**.
    
- Si lo hace el **contenedor de Spring**, se llama **auto-wiring**.
    

En autowiring **ya no se escribe `<ref>` ni `<property>` ni `<constructor-arg>`**:  
el **contenedor Spring hace el enlace de forma automática**.

---

### Formas de autowiring en XML

De forma predeterminada **no hay autowiring** (`autowire="no"`).  
Se puede habilitar:

1. **byType**: por tipo de dato (usa setter injection).
    
2. **byName**: por nombre del bean (usa setter injection).
    
3. **constructor**: por constructor (usa constructor injection).
    
4. **auto-detect**: solo en Spring 2.0.x y anteriores (obsoleto desde Spring 3.0).
    

También se puede hacer **autowiring mediante anotaciones**:

- `@Autowired`
    
- `@Qualifier`
    

---

## Preparación del proyecto Maven

Para el ejemplo se crea **un nuevo proyecto Maven**:

1. **File → New → Maven Project**, aceptar las opciones por defecto.
    
2. **Group Id**: `com.bharath.spring`.
    
3. **Artifact Id**: `springcoreadvanced`.
    
4. Editar `pom.xml`:
    
    - Copiar de un proyecto anterior las secciones de `<properties>`, dependencias (`spring-core`, `spring-context`) y configuración del plugin `maven-compiler-plugin`.
        
    - Configurar **Java 1.8** (por defecto Maven usa 1.5).
```xml
<properties>
        <springframework.version>4.3.6.RELEASE</springframework.version>
    </properties>
 
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${springframework.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${springframework.version}</version>
        </dependency>
    </dependencies>
    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.2</version>
                    <configuration>
                        <source>1.8</source>
                        <target>1.8</target>
                    </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
```
5. Eliminar el paquete de pruebas `junit` si no se usa.
    
6. **Maven → Update Project** para descargar dependencias y limpiar errores.
    
7. Mover el paquete `propertyplaceholder` al nuevo proyecto, pues es un tema avanzado.
    

---

## Auto-Wiring por **tipo** (byType)

1. Crear un sub-paquete `autowiring`.
    
2. Copiar desde el proyecto anterior las clases:
    
    - `Address`

```java
package com.bharath.spring.springcoreadvanced.autowiring;

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

    - `Employee`

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
    - `Test`

```java
package com.bharath.spring.springcoreadvanced.autowiring;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcoreadvanced/autowiring/config.xml");
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


	<bean class="com.bharath.spring.springcoreadvanced.autowiring.Address"
		name="address" p:hno="123" p:street="mira mesa" p:city="san diego" />
		
	<bean class="com.bharath.spring.springcoreadvanced.autowiring.Employee"
		name="employee" autowire="byType"/>

</beans>
```

3. En la clase `Employee`:
    
    - Eliminar constructor e id innecesarios.
        
    - Mantener solo la dependencia `Address`.
        
4. En `config.xml`:
    
    - Definir el bean `address` usando p-schema (setter injection tradicional).
        
    - Para el bean `employee`:
    
```xml
<bean id="employee" class="...Employee" autowire="byType"/>
```
        
        **No se usan `<constructor-arg>` ni `<property>`**.
        
5. Ajustar paquetes en el XML y en la clase `Test` para el nuevo proyecto.
    
6. Ejecutar `Test`:
    
    - Spring crea el bean `address`.
        
    - Al crear `employee`, **Spring detecta que `Employee` depende de `Address`**.
        
    - Busca en la configuración un bean con **clase `Address`**.
        
    - Si no está instanciado, lo crea.
        
    - Inyecta esa instancia en `Employee` usando **setter injection**.
        

---

### Funcionamiento interno

Cuando el contenedor Spring arranca:

- Revisa las **referencias de objeto** (dependencias) de cada bean.
    
- Como se configuró `autowire="byType"`, busca en el XML un bean cuya **clase coincida con el tipo requerido**.
    
- Lo instancia si es necesario y lo **inyecta automáticamente**.
    

---

## Resumen completo

La **autoconexión (auto-wiring)** en Spring permite que el contenedor **resuelva e inyecte dependencias de manera automática**, sin `<ref>`, `<property>` ni `<constructor-arg>`.

Formas de autowiring:

- **byType**: localiza el bean por su **tipo de clase** (usa setter injection).
    
- **byName**: localiza el bean por el **id/nombre** (usa setter injection).
    
- **constructor**: utiliza **inyección por constructor**.
    
- **auto-detect**: obsoleto desde Spring 3.0.
    
- También se puede hacer con anotaciones `@Autowired` y `@Qualifier`.
    

En el ejemplo:

- Se crea un proyecto Maven con dependencias de Spring.
    
- Se prepara un bean `Employee` que depende de `Address`.
    
- Con `autowire="byType"`, Spring identifica automáticamente el bean `Address` y lo inyecta en `Employee` usando **setter injection**, sin necesidad de configurarlo manualmente.
    

Así, la autoconexión **simplifica la configuración** cuando hay muchas dependencias en una aplicación Spring.