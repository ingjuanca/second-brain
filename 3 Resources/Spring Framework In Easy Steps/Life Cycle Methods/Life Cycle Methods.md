- [ ] 
---

### Métodos de ciclo de vida en Spring

Spring proporciona **dos métodos de ciclo de vida** que se pueden configurar para cada **bean**:

- **Método de inicialización (init)**
- **Método de destrucción (destroy)**

Los nombres de estos métodos pueden ser cualquiera, pero **la firma debe ser**:

```java
public void nombreMetodo()
```

(sin parámetros ni valor de retorno).

---

#### Cuándo se usan

- En el **método init** se coloca el código de **inicialización**, por ejemplo:
    
    - Cargar archivos de configuración,
    - Conectarse a una base de datos,
    - Establecer una conexión con un servicio web o servidor.
    
- En el **método destroy** se incluye el código de **limpieza**:
    
    - Cerrar conexiones,
    - Liberar recursos,
    - Eliminar objetos temporales.
      

---

### Flujo del ciclo de vida de un bean

1. El **contenedor Spring** crea una **instancia del bean**.
    
2. **Inyecta las dependencias** (llama a todos los métodos setter necesarios).
    
3. **Invoca el método init**.  
    _Aquí se ejecuta el código de inicialización._
    
4. La aplicación **usa el bean**.
    
5. Antes de destruir el contenedor o el bean:
    
    - Spring **invoca el método destroy** para ejecutar el código de limpieza.
        
6. Finalmente, el **bean es destruido**.
    

> Es similar a los métodos `init()` y `destroy()` de un **servlet**.

---

### Configurar métodos de ciclo de vida en Spring

Existen **tres formas** de configurar los métodos init y destroy:

1. **XML configuration** (archivo de configuración XML).
    
2. **Interfaces de Spring** (implementando interfaces específicas en la clase del bean).
    
3. **Anotaciones**.
    

La lección muestra la **configuración con XML**.

---

#### Ejemplo práctico con XML

1. **Crear el bean**:
    
    - Clase `Patient` con un único campo `id`.
        
    - Crear getters y setters.
        
    - Añadir dos métodos:
        
        `public void hi() { System.out.println("inside hi method"); }` 
        `public void bye() { System.out.println("inside bye method"); }`
        
        (estos actuarán como `init` y `destroy`).

```java
package com.bharath.spring.springcore.lc.xmlconfig;

public class Patient {

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
}

```

1. **Archivo de configuración** `config.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">
	
	<bean class="com.bharath.spring.springcore.lc.xmlconfig.Patient" name="patient" p:id="123" init-method="hi" destroy-method="bye"/>
	
</beans>
```

_Se usa `init-method` para indicar el método de inicialización y `destroy-method` para el de destrucción._

2. **Clase de prueba**:

```java
package com.bharath.spring.springcore.lc.xmlconfig;

import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
	public static void main(String[] args) {
		AbstractApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcore/lc/xmlconfig/config.xml");
		Patient patient = (Patient) context.getBean("patient");
		System.out.println(patient);
		context.registerShutdownHook();
	}
}

```

_Es importante usar `AbstractApplicationContext` para poder llamar a_ `registerShutdownHook()`.

---

### Importante:

- Si no se llama a `registerShutdownHook()`, el **método destroy** **no se ejecutará**.
    
- `registerShutdownHook()` indica a Spring que ejecute el método de destrucción cuando el contenedor se cierre.
    

---

### Comprobación en la prueba

- Al ejecutar:
    
    - Primero se observa la impresión del setter del `id`.
        
    - Luego el mensaje del método `hi` (init).
        
    - Finalmente, tras cerrar el contenedor, aparece el mensaje del método `bye` (destroy).
        

---

**Resumen completo del documento:**

Spring permite **definir métodos de inicialización y destrucción** en cada bean para ejecutar código especial:

- **Init**: se ejecuta **después** de instanciar el bean y de inyectar sus dependencias.
    
- **Destroy**: se ejecuta **justo antes** de que el bean sea destruido.
    

**Configuración en tres pasos:**

1. Crear el bean con métodos `init` y `destroy` (firma `public void nombre()`).
    
2. En el XML, declarar el bean con los atributos `init-method` y `destroy-method`.
    
3. En la clase de prueba, cargar el contexto y llamar a `registerShutdownHook()` para que Spring invoque el método `destroy`.
    

Este mecanismo es comparable a los métodos `init()` y `destroy()` de los servlets, y permite manejar correctamente recursos y conexiones durante el ciclo de vida de la aplicación.