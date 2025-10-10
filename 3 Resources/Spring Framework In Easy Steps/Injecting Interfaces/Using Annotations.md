
---

## Inyección de dependencias usando **anotaciones y autowiring** en Spring

Este documento explica cómo reemplazar la configuración XML tradicional por **anotaciones** para realizar **inyección de dependencias a través de interfaces**, usando **@Component**, **@Autowired** y **@Qualifier**.

Se muestra también cómo manejar errores comunes al tener múltiples implementaciones de una misma interfaz.

---

### 🔹 1. Cambio de configuración XML a anotaciones

Hasta ahora, la inyección de dependencias entre interfaces (`OrderBO` y `OrderDAO`) se había hecho mediante **XML**.  
A partir de esta lección, se hará usando **anotaciones**, lo cual simplifica la configuración y elimina la necesidad de declarar `<bean>` manualmente.

---

### 🔹 2. Marcar clases con `@Component`

Primero, se deben **marcar las clases** para que Spring pueda detectarlas y crear instancias automáticamente.

```java
@Component
public class OrderBOImpl implements OrderBO { ... }

@Component
public class OrderDAOImpl implements OrderDAO { ... }


```

- La anotación `@Component` le indica a Spring que debe registrar la clase como **bean del contenedor**.
    
- Esto reemplaza la configuración XML de `<bean>`.
    

---

### 🔹 3. Configurar el **escaneo de componentes**

En el archivo de configuración `config.xml`, se elimina la lista de beans y se agrega:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p" xmlns:c="http://www.springframework.org/schema/c"
	xmlns:util="http://www.springframework.org/schema/util"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd
     http://www.springframework.org/schema/util
    http://www.springframework.org/schema/util/spring-util.xsd">
    
	<context:component-scan
		base-package="com.bharath.spring.springcoreadvanced.injecting.interfaces" />

</beans>
```

- Esto indica a Spring que debe **escanear el paquete base** y todos sus subpaquetes en busca de clases anotadas con `@Component`.
    
- El contenedor creará automáticamente los objetos detectados.
    

---

### 🔹 4. Uso de `@Autowired` para la inyección automática

En la clase `OrderBOImpl`, se agrega la anotación `@Autowired` para inyectar el DAO:

```java
@Component 
public class OrderBOImpl implements OrderBO {      

	@Autowired     
	private OrderDAO orderDAO;      
	
	@Override     
	public void placeOrder() {         
		System.out.println("Inside OrderBO placeOrder");        
		orderDAO.createOrder();     
	} 
}
```

- `@Autowired` le dice a Spring que **busque automáticamente una implementación de `OrderDAO`** y la inyecte.
    
- Si solo hay **una** implementación, Spring lo hace sin problema.
    

---
### 🔹 5. Inyección exitosa con una sola implementación

Con solo una clase que implemente la interfaz `OrderDAO`, el proceso funciona correctamente:

```bash 
Inside OrderBO placeOrder 
Inside OrderDAO createOrder
```

Spring detecta automáticamente qué clase implementar y realiza la inyección sin conflictos.

---

### 🔹 6. Error: `NoUniqueBeanDefinitionException`

Cuando se agrega **una segunda clase** que implementa la misma interfaz (`OrderDAO`), Spring lanza:

`UnsatisfiedDependencyException: No unique bean of type 'OrderDAO' available`

Esto sucede porque:

- Ahora existen **dos implementaciones** (`OrderDAOImpl1` y `OrderDAOImpl2`).
    
- Spring **no sabe cuál** debe inyectar automáticamente.
    

---

### 🔹 7. Solución con `@Qualifier`

Para resolver el conflicto, se usa `@Qualifier`, que especifica **qué bean exacto** debe inyectarse.

1. Se le asigna un nombre a cada implementación con `@Component("nombre")`:
    
```java
package com.bharath.spring.springcoreadvanced.injecting.interfaces;

import org.springframework.stereotype.Component;

@Component("dao")
public class OrderDAOImpl implements OrderDAO {

	@Override
	public void createOrder() {
		System.out.println("Inside Order DAO createOrder()");

	}

}

```

```java
package com.bharath.spring.springcoreadvanced.injecting.interfaces;

import org.springframework.stereotype.Component;

@Component("dao2")
public class OrderDAOImpl2 implements OrderDAO {

	@Override
	public void createOrder() {
		System.out.println("Inside OrderDAOImpl2 createOrder");

	}

}

```

2. En la clase que depende de ellas (`OrderBOImpl`), se indica cuál se usará:
    
```java
package com.bharath.spring.springcoreadvanced.injecting.interfaces;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Component;

@Component("bo")
public class OrderBOImpl implements OrderBO {

	@Autowired
	@Qualifier("dao1")
	private OrderDAO dao;

	@Override
	public void placeOrder() {
		System.out.println("Inside Order BO");
		dao.createOrder();
	}

	public OrderDAO getDao() {
		return dao;
	}

	public void setDao(OrderDAO dao) {
		this.dao = dao;
	}

}

```
    
3. Spring inyectará específicamente la implementación `dao1`.
    

---

### 🔹 8. Verificación del funcionamiento

Al ejecutar la prueba con `@Qualifier("dao1")`:

```bash
Inside OrderBO placeOrder 
Inside OrderDAO createOrder
```

Y al cambiar a `@Qualifier("dao2")`:

```bash
Inside OrderBO placeOrder 
Inside OrderDAO2 createOrder
```

Esto demuestra que **Spring puede cambiar la implementación inyectada sin modificar código**, solo ajustando el valor del `@Qualifier`.

---

### 🔹 9. Resumen general

| Anotación                | Función                                                                               |
| ------------------------ | ------------------------------------------------------------------------------------- |
| **@Component**           | Registra una clase como bean de Spring (reemplaza `<bean>` en XML).                   |
| **@Autowired**           | Permite la inyección automática de dependencias por tipo.                             |
| **@Qualifier("nombre")** | Indica qué bean específico debe inyectarse cuando hay múltiples candidatos.           |
| **component-scan**       | Escanea el paquete indicado para registrar automáticamente todas las clases anotadas. |

---

### ✅ **Conclusión**

Esta lección demuestra cómo hacer **inyección de dependencias a través de interfaces usando anotaciones**:

1. Se marcan las clases con `@Component`.
    
2. Se configura el escaneo de componentes en el XML.
    
3. Se usa `@Autowired` para inyectar dependencias automáticamente.
    
4. Si hay múltiples implementaciones, se emplea `@Qualifier` para especificar cuál usar.
    

De este modo, Spring:

- Elimina la necesidad de definir beans manualmente en XML.
    
- Facilita la **inyección automática y flexible** de dependencias.
    
- Permite cambiar implementaciones **sin alterar el código Java**, manteniendo un diseño **desacoplado y mantenible**. ✅