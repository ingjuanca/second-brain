
---

## Métodos de ciclo de vida usando **anotaciones** en Spring

En esta lección se muestra la **tercera y última forma** de configurar los métodos de ciclo de vida (`init` y `destroy`) de un **bean de Spring**, esta vez usando **anotaciones** en lugar de interfaces o configuración XML.

---

### 1️⃣ Preparar el paquete y clases

- Crear un **nuevo paquete** llamado `annotations`.
    
- Copiar desde el paquete `interfaces`:
    
    - `Patient.java`,
        
    - `Test.java`,
        
    - `config.xml`.
        
- Abrir los archivos en el nuevo paquete.
    

---

### 2️⃣ Modificar la clase `Patient`

- **Eliminar la implementación** de las interfaces `InitializingBean` y `DisposableBean`.
    
- Quitar los métodos `afterPropertiesSet()` y `destroy()`.
    
- Mantener los métodos:
    
    `public void hi() { ... }` 
    `public void bye() { ... }`
    
    Estos serán ahora los métodos de inicialización y destrucción.
    
- Usar **anotaciones de `javax.annotation`**:

```java
package com.bharath.spring.springcore.lc.annotations;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

public class Patient {

	private int id;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		System.out.println("Inside the setter method");
		this.id = id;
	}

	@PostConstruct
	public void hi() {
		System.out.println("Inside Hi Method");
	}

	@PreDestroy
	public void bye() {
		System.out.println("Inside Bye Method");
	}

	@Override
	public String toString() {
		return "Patient [id=" + id + "]";
	}

}
```
 
_El nombre de los métodos puede ser cualquiera, solo deben ser `public void` sin parámetros._

---

### 3️⃣ Ajustar `config.xml`

- Cambiar el **paquete** de la clase `Patient` para que apunte a `annotations`.
    
- Inicialmente **no se agregan cambios para habilitar las anotaciones**.
    

_Al ejecutar en este punto_, se observará que:

- Spring **inyecta las dependencias** (se ejecutan los setters),
    
- **Pero no se llaman los métodos `hi` y `bye`**, porque **el soporte para anotaciones está deshabilitado por defecto**.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">
    
	<bean class="com.bharath.spring.springcore.lc.annotations.Patient" name="patient" p:id="123" />
		
</beans>
```

---

### 4️⃣ Habilitar soporte para las anotaciones de ciclo de vida

Existen **dos formas**:

#### Opción A: Habilitar solo `@PostConstruct` y `@PreDestroy`

1. En `config.xml`, declarar un bean:
    
    `<bean class="org.springframework.context.annotation.CommonAnnotationBeanPostProcessor"/>`
    
2. No es necesario darle nombre al bean.
    
3. Al ejecutar nuevamente:
    
    - Spring reconoce las anotaciones `@PostConstruct` y `@PreDestroy`.
        
    - Llama a `hi()` después de inyectar las dependencias.
        
    - Llama a `bye()` antes de destruir el bean.
        

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">
    
	<bean class="com.bharath.spring.springcore.lc.annotations.Patient" name="patient" p:id="123" />
	
	<bean class="org.springframework.context.annotation.CommonAnnotationBeanPostProcessor"/>	
</beans>
```
#### Opción B: Habilitar soporte para **todas las anotaciones de Spring**

1. Declarar el **namespace `context`** en la cabecera del archivo:
    
    `xmlns:context="http://www.springframework.org/schema/context"`
    
2. Dentro de `<beans>`, agregar:
    
    `<context:annotation-config/>`
    
3. Con esto:
    
    - Ya no se necesita el bean `CommonAnnotationBeanPostProcessor`.
        
    - Spring habilita el soporte para **todas las anotaciones**, no solo las de ciclo de vida.
        

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">
	<bean class="com.bharath.spring.springcore.lc.annotations.Patient"
		name="patient" p:id="123" />

	<context:annotation-config />

</beans>
```

---

### 5️⃣ Prueba y resultado

- Ejecutar la clase de prueba (`Test.java`) con:
    
    `context.registerShutdownHook();`
    
- Salida esperada:
    
    `Inside setter method Inside hi method Using the object... Inside bye method`
    
- Se confirma que:
    
    - `@PostConstruct` ejecuta el método `hi()` tras la inyección de dependencias.
        
    - `@PreDestroy` ejecuta el método `bye()` antes de destruir el bean.
        

```java
package com.bharath.spring.springcore.lc.annotations;

import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {

		AbstractApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcore/lc/annotations/config.xml");
		Patient patient = (Patient) context.getBean("patient");
		System.out.println(patient);
		context.registerShutdownHook();
		
	}

}

```

---

**Resumen completo del documento:**

Spring permite configurar los **métodos de ciclo de vida (init y destroy)** de un bean **con anotaciones**, evitando el uso de interfaces (`InitializingBean`, `DisposableBean`) o de atributos en XML.

**Procedimiento:**

1. En la clase del bean, marcar el método de inicialización con `@PostConstruct` y el de destrucción con `@PreDestroy`.
    
2. Habilitar el soporte de anotaciones:
    
    - O bien creando el bean `CommonAnnotationBeanPostProcessor` (solo para estas dos anotaciones),
        
    - O mejor, usando `<context:annotation-config/>` en el XML para habilitar **todas las anotaciones** de Spring.
        

Con esto, Spring:

- Ejecuta automáticamente el método anotado con `@PostConstruct` tras inyectar las dependencias,
    
- Y llama al método con `@PreDestroy` cuando el contenedor se cierra.
    

Esta es la forma **más recomendada y utilizada en aplicaciones reales** para manejar el ciclo de vida de los beans en Spring.
