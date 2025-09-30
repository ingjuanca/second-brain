
---

## Colecciones independientes (Standalone Collections) en Spring

En esta lección se aprende a trabajar con **colecciones independientes** en Spring, que permiten definir listas, mapas o sets fuera de un bean y reutilizarlos en múltiples beans.

---

### Problema con las colecciones locales

Cuando se inyecta una colección directamente dentro de un bean:

- La colección queda **local al bean** y **no puede reutilizarse** en otros beans.
    
- Además, por defecto Spring usa:
    
    - `ArrayList` para listas,
        
    - `LinkedHashSet` para sets,
        
    - etc.  
        No hay forma de cambiar el tipo de colección a otra implementación como `LinkedList`.
        

---

### Solución: colecciones independientes con **util schema**

- Se usa el **namespace `util`** de Spring.
    
- Pasos:
    
    1. Importar el namespace `util` en `config.xml`.

```xml
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
```

    2. Definir la colección con sintaxis `util:collectionName`.
        
        - Ejemplo:
            
```xml
<util:list list-class="java.util.LinkedList" id="productNames">
	<value>Mac Book</value>
	<value>Iphone</value>
</util:list>

<bean class="com.bharath.spring.springcoreadvanced.standalone.collections.ProductsList"
	name="productsList">
	<property name="productNames">
		<ref bean="productNames" />
	</property>
</bean>
```
            
    3. Esta colección ahora es **independiente de cualquier bean** y puede inyectarse en uno o varios beans.
        

---

### Ejemplo práctico: `ProductsList`

1. Crear clase `ProductsList`:
    
```java
package com.bharath.spring.springcoreadvanced.standalone.collections;

import java.util.List;

public class ProductsList {

	private List<String> productNames;

	public List<String> getProductNames() {
		return productNames;
	}

	public void setProductNames(List<String> productNames) {
		this.productNames = productNames;
	}
	
	@Override
	public String toString() {
		return "ProductsList [productNames=" + productNames + "]";
	}
}
```
    
2. Definir en `config.xml`:
    
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

	<util:list list-class="java.util.LinkedList" id="productNames">
		<value>Mac Book</value>
		<value>Iphone</value>
	</util:list>

	<bean
		class="com.bharath.spring.springcoreadvanced.standalone.collections.ProductsList"
		name="productsList">
		<property name="productNames">
			<ref bean="productNames" />
		</property>
	</bean>

</beans>
```
    
3. Crear clase de prueba (`Test`), obtener el bean `productsList` y mostrar el contenido.
    
```java
package com.bharath.spring.springcoreadvanced.standalone.collections;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcoreadvanced/standalone/collections/config.xml");
		ProductsList pl = (ProductsList) context.getBean("productsList");
		System.out.println(pl);

	}

}

```
---

### Resultado

- Al ejecutar, se inyecta correctamente la `LinkedList` con los valores definidos.
    
- Tras implementar `toString()`, se imprime la lista de productos.
    
- La colección es **reutilizable**: puede inyectarse en **cualquier número de beans**.
    

---

**Resumen completo del documento:**

Las **colecciones independientes en Spring** permiten:

- Definir listas, mapas o sets **fuera de los beans**,
    
- Especificar la implementación concreta (`LinkedList`, `HashMap`, etc.),
    
- Reutilizarlas en múltiples beans.
    

Pasos:

1. **Agregar el namespace `util`** en `config.xml`.
    
2. **Definir la colección con `util:list`, `util:map`, `util:set`**, indicando la clase de implementación.
    
3. **Inyectar la colección en cualquier bean** mediante `<ref>`.
    

En el ejemplo:

- Se crea una `LinkedList` de nombres de productos en `config.xml`.
    
- Se inyecta en el bean `ProductsList`.
    
- Al ejecutar, se muestra correctamente la lista.
    

Esto resuelve dos limitaciones de las colecciones locales:

- La falta de **reutilización**,
    
- La imposibilidad de **cambiar el tipo de colección** por defecto.