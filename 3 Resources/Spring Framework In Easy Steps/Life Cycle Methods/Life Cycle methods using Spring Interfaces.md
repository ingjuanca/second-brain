
---

### Métodos de ciclo de vida usando **interfaces de Spring**

En esta lección se explica cómo configurar los **métodos de ciclo de vida** de un **bean de Spring** utilizando **interfaces de Spring**, en lugar de la configuración XML vista anteriormente.

---

## Paso 1: Preparar el paquete y clases

- Crear un **nuevo paquete** llamado `interfaces` (en lugar de `xmlconfig`).
    
- Copiar dentro de él las tres clases utilizadas previamente en la configuración por XML:
    
    - `Patient.java` (POJO),
    - `Test.java`,
    - y el archivo de configuración `config.xml`.
    
- El contenido de `Test.java` **no necesita cambios**.
    

---

## Paso 2: Implementar las interfaces en `Patient.java`

1. **Implementar la interfaz `InitializingBean`**
    
    - Importar desde:
        
        `org.springframework.beans.factory.InitializingBean`
        
    - Esta interfaz exige implementar el método:
        
        `public void afterPropertiesSet() throws Exception`
        
        _Este método actúa como el método `init`_.
        
    - Dentro del método, agregar:
        
        `System.out.println("Inside afterPropertiesSet");`
        
2. **Implementar la interfaz `DisposableBean`**
    
    - Importar desde:
        
        `org.springframework.beans.factory.DisposableBean`
        
    - Implementar el método:
        
        `public void destroy() throws Exception`
        
        _Este método actúa como el método `destroy`_.
        
    - Dentro del método, agregar:
        
        `System.out.println("Inside destroy");`
        

```java
package com.bharath.spring.springcore.lc.interfaces;

import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;

public class Patient implements InitializingBean, DisposableBean{

	private int id;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		System.out.println("Inside the setter method");
		this.id = id;
	}

	public void hi() {
		System.out.println("Inside Hi Method");
	}
	
	public void bye(){
		System.out.println("Inside Bye Method");
	}

	@Override
	public String toString() {
		return "Patient [id=" + id + "]";
	}

	@Override
	public void afterPropertiesSet() throws Exception {
		System.out.println("Inside afterPropertiesSet()");
		
	}

	@Override
	public void destroy() throws Exception {
		System.out.println("Inside destroy()");
	}

}
```
---

## Paso 3: Ajustes en el archivo `config.xml`

- Ya **no es necesario especificar** los atributos:
    
    `init-method="" destroy-method=""`
    
- Basta con definir el bean:
    
    `<bean name="patient" class="com.ejemplo.interfaces.Patient" p:id="123"/>`
    
- Spring detectará automáticamente que el bean implementa las interfaces `InitializingBean` y `DisposableBean` e invocará los métodos `afterPropertiesSet()` y `destroy()`.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">
	
	<bean class="com.bharath.spring.springcore.lc.interfaces.Patient"
		name="patient" p:id="123" />
		
</beans>
```

---

## Paso 4: Ejecutar la prueba

- En `Test.java` seguir usando:
    
    `context.registerShutdownHook();`
    
    para que el método `destroy()` se ejecute cuando el contenedor se cierre.
	
- Cambiar en el archivo de configuración el **paquete** de la clase `Patient` para que apunte a `interfaces` en lugar de `xmlconfig`.

```java
package com.bharath.spring.springcore.lc.interfaces;

import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {

		AbstractApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcore/lc/interfaces/config.xml");
		Patient patient = (Patient) context.getBean("patient");
		System.out.println(patient);
		context.registerShutdownHook();
	}
}

```

- Ejecutar como **Java Application**:
    
    1. Spring crea el objeto `Patient`,
        
    2. Llama a los **métodos setter** para inyectar dependencias,
        
    3. Invoca `afterPropertiesSet()` (equivalente a `init`),
        
    4. Se utiliza el bean,
        
    5. Antes de cerrar el contenedor, se llama a `destroy()`.
        

---

**Resumen completo del documento:**

La lección muestra cómo implementar los **métodos de ciclo de vida de un bean Spring** usando **interfaces propias de Spring**, en vez de definir `init-method` y `destroy-method` en XML.

**Pasos clave:**

1. Crear la clase `Patient` que implemente:
    
    - `InitializingBean` → define el método `afterPropertiesSet()` (método de inicialización).
        
    - `DisposableBean` → define el método `destroy()` (método de destrucción).
        
2. En el `config.xml` solo se declara el bean, **sin especificar atributos de init o destroy**.
    
3. En la prueba se usa `registerShutdownHook()` para que Spring invoque automáticamente `destroy()` al cerrar el contenedor.
    

Al ejecutar, Spring:

- Crea el bean,
    
- Inyecta las dependencias,
    
- Llama a `afterPropertiesSet()` tras inyectarlas,
    
- Y al finalizar, ejecuta `destroy()`.
    

De esta forma, los **ciclos de vida de los beans** se gestionan de manera automática mediante las **interfaces de Spring**, sin necesidad de configuración XML explícita para los métodos `init` y `destroy`.