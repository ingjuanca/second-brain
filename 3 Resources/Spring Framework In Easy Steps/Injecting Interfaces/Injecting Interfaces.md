
---

## Inyección de dependencias a través de **interfaces** en Spring

Este documento explica cómo realizar **inyección de dependencias mediante interfaces** en Spring, aplicando el principio de **programar hacia interfaces y no hacia implementaciones** para lograr un diseño flexible y desacoplado.

---

### 🔹 1. Concepto general

En aplicaciones reales, las clases suelen depender de otras para cumplir una función.  
En lugar de acoplar una clase directamente con otra, se utiliza una **interfaz** para definir el comportamiento, y Spring se encarga de **inyectar la implementación** adecuada.

**Ejemplo general del flujo:**

- `OrderBO` → contiene la **lógica de negocio** relacionada con pedidos.
    
- `OrderDAO` → maneja el **acceso a datos** (JDBC, Hibernate, etc.).
    
- `OrderBOImpl` implementa `OrderBO`.
    
- `OrderDAOImpl` implementa `OrderDAO`.
    

El objeto `OrderBOImpl` **depende** de `OrderDAO`, pero a través de su interfaz, no de una clase específica.  
Esto permite reemplazar la implementación del DAO sin modificar el código, solo cambiando la configuración de Spring.

---

### 🔹 2. Creación de interfaces y clases

#### `OrderBO` (interfaz)

```java
package com.bharath.spring.springcoreadvanced.injecting.interfaces;

public interface OrderBO {	
	void placeOrder();
}
```

#### `OrderBOImpl` (implementación)

```java
package com.bharath.spring.springcoreadvanced.injecting.interfaces;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Component;

public class OrderBOImpl implements OrderBO {

	private OrderDAO orderDAO;

	@Override
	public void placeOrder() {
		System.out.println("Inside Order BO");
		orderDAO.createOrder();
	}

	public OrderDAO getDao() {
		return orderDAO;
	}

	public void setDao(OrderDAO dao) {
		this.dao = orderDAO;
	}
}
```

#### `OrderDAO` (interfaz)

```java
public interface OrderDAO {
    void createOrder();
}
```

#### `OrderDAOImpl` (implementación)

```java
package com.bharath.spring.springcoreadvanced.injecting.interfaces;

import org.springframework.stereotype.Component;

public class OrderDAOImpl implements OrderDAO {

	@Override
	public void createOrder() {
		System.out.println("Inside Order DAO createOrder()");
	}

}
```

---

### 🔹 3. Configuración en el archivo **XML**

En `config.xml` se definen los beans e inyecciones:

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

    <!-- Dependencia -->
    <bean id="dao" class="com.bharath.springcoreadvanced.injecting.interfaces.OrderDAOImpl"/>

    <!-- Bean principal que depende del DAO -->
    <bean id="bo" class="com.bharath.springcoreadvanced.injecting.interfaces.OrderBOImpl">
        <property name="orderDAO" ref="dao"/>
    </bean>

</beans>

```

**Explicación:**

- Primero se crea el bean del DAO (`OrderDAOImpl`).
    
- Luego se crea el bean del BO (`OrderBOImpl`).
    
- En `OrderBOImpl`, Spring **inyecta** el bean `dao` en la propiedad `orderDAO`.
    

Este proceso demuestra la **inyección por propiedad (setter injection)**, donde el contenedor Spring llama al setter correspondiente e inyecta el objeto DAO.

---

### 🔹 4. Clase de prueba

```java
package com.bharath.spring.springcoreadvanced.injecting.interfaces;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcoreadvanced/injecting/interfaces/config.xml");
		OrderBO bo = (OrderBO) context.getBean("bo");
		bo.placeOrder();

	}

}
```

**Salida esperada:**

```console
Inside OrderBO placeOrder 
Inside OrderDAO createOrder
```

**Flujo:**

1. Spring crea `OrderDAOImpl` y lo inyecta en `OrderBOImpl`.
    
2. Al ejecutar `placeOrder()`, se imprime el mensaje de `OrderBOImpl`.
    
3. Luego se invoca `createOrder()` desde `OrderDAOImpl`, imprimiendo su mensaje.
    
4. Todo esto ocurre sin que `OrderBOImpl` conozca la implementación concreta del DAO.
    

---

### 🔹 Ventajas del enfoque basado en interfaces

- **Desacoplamiento total:** las clases dependen de interfaces, no de implementaciones.
    
- **Polimorfismo dinámico:** Spring puede inyectar distintas implementaciones de una interfaz.
    
- **Flexibilidad:** se puede cambiar la implementación con solo modificar el XML, sin alterar el código Java.
    
- **Mantenibilidad:** el sistema es más fácil de ampliar o probar.
    
- **Reutilización:** una misma interfaz puede tener múltiples implementaciones según el contexto (JDBC, Hibernate, JPA, etc.).
    

---

### 🔹 Ejemplo de cambio de implementación (Loose Coupling)

Supongamos que se crea una segunda implementación del DAO:

```java
public class OrderDAOImpl2 implements OrderDAO {     
	public void createOrder() {         
		System.out.println("Inside OrderDAO2 createOrder");     
	} 
}
```

Luego se actualiza la configuración:

```xml
<bean id="dao2" class="com.bharath.springcoreadvanced.injecting.interfaces.OrderDAOImpl2"/>  

<bean id="bo" class="com.bharath.springcoreadvanced.injecting.interfaces.OrderBOImpl">
    <property name="orderDAO" ref="dao2"/> 
</bean>
```

**Sin cambiar el código Java**, Spring inyectará la nueva clase `OrderDAOImpl2`.  
Al ejecutar el test, la salida cambiará automáticamente:

```console
Inside OrderBO placeOrder 
Inside OrderDAO2 createOrder
```

Esto demuestra la **inyección dinámica de dependencias** y el **polimorfismo en tiempo de ejecución** que Spring facilita.

---

### 🔹 Resumen general

| Concepto                       | Descripción                                                     |
| ------------------------------ | --------------------------------------------------------------- |
| **OrderBO / OrderDAO**         | Interfaces que definen el contrato.                             |
| **OrderBOImpl / OrderDAOImpl** | Clases que implementan las interfaces.                          |
| **Inyección**                  | Se realiza por propiedad (`<property>`).                        |
| **Polimorfismo**               | Spring elige la implementación adecuada en tiempo de ejecución. |
| **Ventaja clave**              | Permite sustituir implementaciones sin tocar el código fuente.  |

---

### ✅ **Conclusión**

Este documento enseña cómo aplicar el principio de **"programar hacia interfaces"** usando **inyección de dependencias en Spring**.  
Con este enfoque:

- Las clases están **débilmente acopladas**.
    
- El sistema es **fácil de mantener, probar y escalar**.
    
- Se aprovecha el **polimorfismo** de Java junto con la configuración dinámica de Spring.
    

En definitiva, Spring permite que una aplicación cambie su comportamiento simplemente **modificando la configuración XML o de anotaciones**, sin alterar ni recompilar el código fuente.  
Este patrón es la base de todas las aplicaciones empresariales modernas basadas en Spring. ✅