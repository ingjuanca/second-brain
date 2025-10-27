
---

## 🧩 **Título: Creación de un Proyecto Maven con Spring AOP**

---

### 🔹 **1. Creación del Proyecto Maven AOP**

1. Abre **Eclipse** → `File → New → Maven Project`.
    
2. Usa el **arquetipo QuickStart** y define:
    
    - **Group ID:** `com.bharath.spring`
        
    - **Artifact ID:** `springAOP`
        
3. Finaliza la creación del proyecto.
    

Luego abre el archivo `pom.xml` y actualiza las dependencias:

- Abre el `pom.xml` de _Spring Core_ y copia desde la sección `<properties>` hasta `<dependencies>`.
    
- Pega ese bloque en tu `springAOP/pom.xml`.
    
- Elimina la dependencia de **JUnit** (no se usará) y borra la carpeta de pruebas `src/test/java`.
    

Agrega estas dependencias adicionales:

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
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-aop</artifactId>
		<version>${springframework.version}</version>
	</dependency>
	<dependency>
		<groupId>org.aspectj</groupId>
		<artifactId>aspectjrt</artifactId>
		<version>1.6.11</version>
	</dependency>
	<dependency>
		<groupId>org.aspectj</groupId>
		<artifactId>aspectjweaver</artifactId>
		<version>1.6.11</version>
	</dependency>
</dependencies>
```

Luego ejecuta:

`Right click → Maven → Update Project → OK`

✅ En las dependencias de Maven ya deberías ver:  
`spring-context`, `spring-aop`, `aspectjrt`, `aspectjweaver`.

---

### 🔹 **2. Crear las clases principales (POJOs)**

Crea la estructura básica de la aplicación.

1. Elimina la clase `App.java`.
    
2. En `src/main/java/com/bharath/spring/springAOP`, crea:
    
    - **Interfaz:** `ProductService`
        
    - **Clase:** `ProductServiceImpl` que la implemente.
        

📘 **Código:**

```java
package com.bharath.spring.springaop;

public interface ProductService {

	int multiply(int num1, int num2);
}
```

```java
package com.bharath.spring.springaop;

public class ProductServiceImpl implements ProductService {
	@Override
	public int multiply(int num1, int num2) {
		return num1 * num2;
	}
}

```

---

### 🔹 **3. Crear el Aspecto de Logging**

1. Crea un nuevo paquete: `com.bharath.spring.springAOP.aspects`.
    
2. Dentro de él, crea una clase llamada `LoggingAspect`.
    

📘 **Código:**

```java
@Aspect 
public class LoggingAspect { }
```

La anotación `@Aspect` (de `org.aspectj.lang.annotation.Aspect`) indica que esta clase define un **aspecto**, es decir, lógica que se aplicará a otros métodos de forma transversal (en este caso, logging).

---

### 🔹 **4. Crear los Advices**

Los _advices_ son los métodos que se ejecutan **antes o después** del método de negocio.

Crea dentro de `LoggingAspect`:

```java
@Before("") 
public void logBefore(JoinPoint joinPoint) {     
	System.out.println("Before calling the method"); 
} 
 
@After("") 
public void logAfter(JoinPoint joinPoint) {     
	System.out.println("After the method invocation"); 
}
```

- `@Before` → se ejecuta antes del método objetivo.
    
- `@After` → se ejecuta después del método.
    
- `JoinPoint` permite acceder a información sobre el método ejecutado (nombre, parámetros, etc.).
    

Por ahora, las expresiones de los pointcuts (`""`) están vacías. Se definirán a continuación.

---

### 🔹 **5. Crear las expresiones Pointcut**

Un **pointcut** define _dónde_ se aplica un advice.

Actualiza el código anterior agregando la expresión:

```java
package com.bharath.spring.springaop.aspects;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

@Aspect
public class LogginAspect {

	@Before("execution(* com.bharath.spring.springaop.ProductServiceImpl.multiply(..))")
	public void logBefore(JoinPoint joinPoint) {
		System.out.println("Before Calling the method");
	}

	@After("execution(* com.bharath.spring.springaop.ProductServiceImpl.multiply(..))")
	public void logAfter(JoinPoint joinPoint) {
		System.out.println("After the method invocation");
	}

}

```

📘 **Explicación:**

- `execution` → indica que la expresión se aplica a un método.
    
- `*` → cualquier tipo de retorno.
    
- `(..)` → cualquier número o tipo de parámetros.
    
- La expresión completa se refiere al método `multiply` dentro de `ProductServiceImpl`.
    

---

### 🔹 **6. Crear el archivo de configuración de Spring**

Crea un archivo `config.xml` dentro del paquete `com.bharath.spring.springAOP.test`.

📘 **Estructura:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p" xmlns:c="http://www.springframework.org/schema/c"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop.xsd">
    
	<aop:aspectj-autoproxy />
	
</beans>
```

📘 **Explicación:**

- `xmlns:aop` agrega el espacio de nombres de AOP.
    
- `<aop:aspectj-autoproxy/>` habilita la detección automática de aspectos definidos con anotaciones (`@Aspect`).
    

---

### 🔹 **7. Configurar los Beans**

Agrega los beans de servicio y del aspecto dentro del `config.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p" xmlns:c="http://www.springframework.org/schema/c"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop.xsd">
    
	<aop:aspectj-autoproxy />
	
	<bean name="productService" class="com.bharath.spring.springaop.ProductServiceImpl" />
	
	<bean name="logginAspect" class="com.bharath.spring.springaop.aspects.LogginAspect" />
	
</beans>
```

📘 **Spring**, al iniciar, creará los objetos de estas clases y aplicará el _weaving_ (entrelazado de AOP) automáticamente.

---

### 🔹 **8. Crear la clase de prueba**

Crea una clase `Test` en `com.bharath.spring.springAOP.test`.

📘 **Código:**

```java
package com.bharath.spring.springaop.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.bharath.spring.springaop.ProductService;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext("com/bharath/spring/springaop/test/config.xml");
		ProductService productService = (ProductService) context.getBean("productService");
		System.out.println(productService.multiply(4, 5));

	}
}

```

---

### 🔹 **9. Ejecución y flujo del programa**

Cuando se ejecuta la clase `Test`:

1. Spring carga el contexto desde `config.xml`.
    
2. Crea los beans: `ProductServiceImpl` y `LoggingAspect`.
    
3. Genera dinámicamente un **proxy** de `ProductServiceImpl`.
    
4. Cuando se llama al método `multiply(4, 5)`:
    
    - Se ejecuta el `@Before` advice → imprime **"Before calling the method"**.
        
    - Se ejecuta el método real → calcula 4 × 5 = 20.
        
    - Se ejecuta el `@After` advice → imprime **"After the method invocation"**.
        
5. Finalmente, se muestra el resultado: **Result: 20**
    

📋 **Salida esperada en consola:**

`Before calling the method After the method invocation Result: 20`

---

## 🧾 **Resumen general**

|Etapa|Descripción|
|---|---|
|**1. Crear proyecto Maven**|Se configura `pom.xml` con dependencias de Spring AOP y AspectJ.|
|**2. Crear POJOs**|`ProductService` y `ProductServiceImpl` con método `multiply`.|
|**3. Crear Aspecto**|Clase `LoggingAspect` marcada con `@Aspect`.|
|**4. Crear Advices**|Métodos `@Before` y `@After` que imprimen mensajes en consola.|
|**5. Definir Pointcuts**|Usan `execution(* package.Class.method(..))` para identificar el método `multiply`.|
|**6. Configurar Spring**|Archivo `config.xml` con `<aop:aspectj-autoproxy/>` habilitado.|
|**7. Definir Beans**|Beans de `ProductServiceImpl` y `LoggingAspect` en XML.|
|**8. Crear Test**|Clase `Test` que obtiene el bean y ejecuta el método.|
|**9. Ejecución**|AOP aplica los advices antes y después del método real.|

---

## ✅ **Conclusión final**

Este proyecto demuestra cómo **Spring AOP** integra **AspectJ** para aplicar lógica transversal (como logging) sin modificar el código fuente del negocio.

**Ventajas clave:**

- Separa la lógica transversal del código principal.
    
- Reutilizable y fácil de mantener.
    
- Se activa de forma automática gracias a `@Aspect` y `<aop:aspectj-autoproxy/>`.
    

> 💡 En resumen:  
> Con **Spring AOP y AspectJ**, puedes agregar comportamientos adicionales a tus métodos (como logs, seguridad o manejo de transacciones) de forma **modular, declarativa y no invasiva**.