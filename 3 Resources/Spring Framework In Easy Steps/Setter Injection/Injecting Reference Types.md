
---

### Inyección de **tipos de referencia** (objetos) en Spring

El **tercer tipo de inyección de dependencias**, y uno de los más usados en aplicaciones reales, es la **inyección de objetos** o **tipos de referencia**.

Se utiliza cuando existe una **relación “HAS-A”** (tiene-un) entre dos clases:  
_Una clase necesita una instancia de otra clase para su funcionamiento._

---

## Ejemplo: Clases `Student` y `Scores`

- `Student` depende de la clase `Scores`, que almacena:
    
    - `maths` (double)
        
    - `physics` (double)
        
    - `chemistry` (double)

### Paso 1: Crear los POJOs

1. Crear la clase **`Scores`** con los campos `maths`, `physics` y `chemistry`.
    
    - Generar **getters y setters**.
        
    - Formatear el código (Ctrl + Shift + F).

```java
package com.bharath.spring.springcore.reftypes;

public class Scores {
	private Double maths;
	private Double physics;
	private Double chemistry;

	public Double getMaths() {
		return maths;
	}

	public void setMaths(Double maths) {
		this.maths = maths;
	}

	public Double getPhysics() {
		return physics;
	}

	public void setPhysics(Double physics) {
		this.physics = physics;
	}

	public Double getChemistry() {
		return chemistry;
	}

	public void setChemistry(Double chemistry) {
		this.chemistry = chemistry;
	}
	
	@Override
	public String toString() {
		return "Scores [maths=" + maths + ", physics=" + physics + ", chemistry=" + chemistry + "]";
	}

}
```

2. Crear la clase **`Student`** con una propiedad:
    
    `private Scores scores;`
    
    - Generar **getters y setters** para `scores`.
        
    - Implementar un **método `toString()`** en ambas clases para mostrar la información.

```java
package com.bharath.spring.springcore.reftypes;

public class Student {
	private Scores scores;

	public Scores getScores() {
		return scores;
	}

	public void setScores(Scores scores) {
		this.scores = scores;
	}

	@Override
	public String toString() {
		return "Student [scores=" + scores + "]";
	}
}

```

---

## Paso 2: Configuración de Spring (XML)

Para inyectar un objeto dentro de otro se puede usar:

1. **Setter Injection con `<ref>`**:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">

	<bean name="scores" class="com.ejemplo.Scores">
	    <property name="maths" value="80"/>
	    <property name="physics" value="90"/>
	    <property name="chemistry" value="78"/>
	</bean>
	
	<bean name="student" class="com.ejemplo.Student">
	    <property name="scores">
	        <ref bean="scores"/>
	    </property>
	</bean>

</beans>
```


2. **Setter Injection por atributo**:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">

	<bean name="scores" class="com.ejemplo.Scores">
	    <property name="maths" value="80"/>
	    <property name="physics" value="90"/>
	    <property name="chemistry" value="78"/>
	</bean>
	
	<bean name="student" class="com.ejemplo.Student">
	    <property name="scores" ref="scores"/>
	</bean>

</beans>
```

    Aquí `student` recibe la referencia al bean `scores`.
    
    El elemento `<ref>` también puede usarse como atributo:
    
    `<property name="scores" ref="scores"/>`

3. **P Schema (namespace p)**:
    
    - Permite inyectar valores de forma más compacta:
        

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">
	
	<bean name="scores" class="com.ejemplo.Scores" p:maths="80" p:physics="90" p:chemistry="78"/>
	
	<bean name="student" class="com.ejemplo.Student" p:scores-ref="scores"/>
	      
</beans>
```

---

## Paso 3: Clase de prueba

- Crear un `main` que:
    
    1. Cargue el archivo de configuración con:
        
        `ApplicationContext context =    new ClassPathXmlApplicationContext("reftypes/config.xml");`
        
    2. Obtenga el bean:
        
        `Student student = (Student) context.getBean("student"); System.out.println(student);`
        
- Gracias al `toString()` en `Student` y `Scores`, se mostrarán los valores de las notas.

```java
package com.bharath.spring.springcore.reftypes;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcore/reftypes/config.xml");
		Student student = (Student) context.getBean("student");
		System.out.println(student);

	}
}

```

---

### Resultado

Al ejecutar la prueba, Spring:

- Crea el bean `Scores` con los valores de **maths = 80**, **physics = 90**, **chemistry = 78**.
    
- Crea el bean `Student` e **inyecta el objeto `Scores`** en su propiedad `scores`.
    
- La salida muestra el estudiante junto con sus calificaciones.
    

---

**Resumen completo del documento:**

La **inyección de tipos de referencia** es el proceso de **inyectar un objeto como dependencia de otro objeto**.  
En el ejemplo:

1. Se crean las clases **`Student`** y **`Scores`**, donde `Student` depende de `Scores`.
    
2. Se configuran los beans en **Spring XML** para:
    
    - Crear primero el bean `scores` con valores para las notas.
        
    - Crear el bean `student` e inyectarle la referencia al bean `scores` usando `<ref>`.
        
3. Se ejecuta una clase de prueba que recupera el bean `student` y muestra sus valores.
    

Spring permite tres formas de inyectar la referencia:

- **ref como elemento**,
    
- **ref como atributo**,
    
- **p schema (p:namespace)**.
    

Este es uno de los métodos de inyección más utilizados en aplicaciones reales, porque modela la **relación entre objetos** de manera clara y automática.