
---

### Inyección de colecciones en Spring

Hasta ahora se ha trabajado con la inyección de **tipos primitivos**.  
En esta sección se aprende a **inyectar colecciones**, el **segundo tipo de dependencias** que soporta Spring.

Spring permite inyectar **cuatro tipos de colecciones**:

1. **List** – Representada en XML con el elemento `<list>`.  
    *Spring crea automáticamente una lista e inserta los valores especificados con `<value>`.  
    *Para agregar un valor `null` se usa el elemento `<null>`.
    
2. **Set** – Representada con `<set>`.  
    *Funciona igual que `<list>`, pero **no permite duplicados**.
    
3. **Map** – Representada con `<map>` y elementos `<entry>` para **pares clave–valor**.  
    *Las claves y valores pueden definirse como **atributos** o como **elementos hijos** de `<entry>`.
    
4. **Properties** – Representadas con `<props>`.  
    *Cada propiedad se define con `<prop key="...">valor</prop>`.
    

---

### Ejemplo práctico: Inyección de una List

#### 1️⃣ Crear el bean (Hospital)

- Crear una clase `Hospital` en Eclipse con:

    - Una propiedad `name` (String) para el nombre del hospital.
    - Una propiedad `departments` (List) para los departamentos.
     - Métodos **getter y setter** para ambas propiedades.

```java
package com.bharath.spring.springcore.list;

import java.util.List;

public class Hospital {

	private String name;
	private List<String> departments;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public List<String> getDepartments() {
		return departments;
	}

	public void setDepartments(List<String> departments) {
		this.departments = departments;
	}

}
```

---

#### 2️⃣ Configurar la lista en XML

- Crear un nuevo archivo de configuración `listconfig.xml` (separado de `config.xml` para mayor claridad).
    
- Declarar un bean:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">

	<bean name="hospital" class="com.bharath.spring.springcore.list.Hospital">
		<property name="name">
			<value>Global Hospital</value>
		</property>
		<property name="departments">
			<list> 
				<value>Front Office</value>
				<value>In Patient</value>
				<value>Out Patient</value>
			</list>
		</property>
	</bean>

</beans>
```

- Aquí:
    
    - La propiedad `name` se inyecta como un valor primitivo (String).
    - La propiedad `departments` se inyecta como una **lista de Strings** usando `<list>`.


---

#### 3️⃣ Crear y ejecutar la prueba

- Crear una clase de prueba en Eclipse con un **método `main`**:
    
    1. Cargar el archivo `listconfig.xml` usando:
        
        `ApplicationContext context = new ClassPathXmlApplicationContext("com/bharath/spring/springcore/list/listconfig.xml");`
        
    2. Obtener el bean:
        
        `Hospital hospital = (Hospital) context.getBean("hospital");`
        
    3. Imprimir los valores:
        
        `System.out.println(hospital.getName()); System.out.println(hospital.getDepartments());`

```java
package com.bharath.spring.springcore.list;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcore/list/listconfig.xml");
		Hospital hospital = (Hospital) context.getBean("hospital");
		System.out.println(hospital.getName());
		System.out.println(hospital.getDepartments());
		System.out.println(hospital.getDepartments().getClass());
	}
}

```

---

### Ejecución y resultado

- Al ejecutar la prueba:
    
    - El contenedor Spring **carga la configuración**,
        
    - **Crea el bean Hospital**,
        
    - **Inyecta el nombre** `"Global Hospitals"`,
        
    - **Crea e inyecta la lista** de departamentos: `"Front Office"`, `"Inpatient"`, `"Outpatient"`.
        
- Spring, por defecto, crea un **ArrayList** si no se especifica el tipo de lista.  
    Se puede verificar con:
    
    `hospital.getDepartments().getClass()`
    
    Más adelante se puede personalizar para usar, por ejemplo, una `LinkedList`.
    

---

**Resumen completo del documento:**

Spring permite inyectar **colecciones** (List, Set, Map, Properties) además de valores primitivos.  
El proceso para inyectar una **List** en un bean es:

1. **Crear el bean** (`Hospital`) con propiedades para el nombre y la lista de departamentos.
    
2. **Configurar el bean en XML**, usando:
    
    - `<property name="name"><value>...</value></property>` para inyectar el nombre.
        
    - `<property name="departments"><list>...</list></property>` para inyectar una lista de Strings.
        
3. **Crear una clase de prueba**, cargar el archivo XML con `ClassPathXmlApplicationContext`, obtener el bean y mostrar sus propiedades.
    

Al ejecutarse, Spring **crea automáticamente un ArrayList** e inyecta los valores definidos, demostrando la capacidad del framework para manejar colecciones de manera sencilla.