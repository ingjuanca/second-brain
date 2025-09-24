
---

### Inyección de un **Map** en Spring

En esta lección se muestra cómo **crear e inyectar un mapa (Map)** en un bean de Spring.  
El ejemplo consiste en una **clase Customer** que guarda:

- **ID del cliente** (`int`), y
    
- Un **Map<Integer, String> products**, donde:
    
    - La **clave (key)** es el ID del producto,
     - El **valor (value)** es el nombre del producto.
     

---

## 1️⃣ Crear el POJO (Java Bean)

- Crear la clase `Customer` con:
    
    `private int id; 
    private Map<Integer, String> products;`
    
- Generar **getters y setters** para ambos campos.
    
- Crear un **método toString()** que muestre el `id` y el `products` (para imprimir el objeto completo sin usar getters).
    
- Formatear el código (Ctrl + Shift + F).

```java
package com.bharath.spring.springcore.map;

import java.util.Map;

public class Customer {
	private int id;
	private Map<Integer, String> products;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public Map<Integer, String> getProducts() {
		return products;
	}

	public void setProducts(Map<Integer, String> products) {
		this.products = products;
	}

	@Override
	public String toString() {
		return "Customer [id=" + id + ", products=" + products + "]";
	}
}

```

---

## 2️⃣ Configurar el archivo XML de Spring

- Copiar un archivo de configuración existente (`config.xml`) y colocarlo en el paquete `map`, renombrándolo si se desea.
    
- Definir el bean:
    
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">

	<bean name="customer" class="com.bharath.spring.springcore.map.Customer"
		p:id="20">
		<property name="products">
			<map>
				<!-- 4 maneras de definir las entradas del mapa -->
	            <!-- 1. key y value como atributos -->
				<entry key="100" value="IPhone" />
								
				<!-- 2. key como atributo, value como elemento -->
				<entry key="200">
					<value>IPad</value>
				</entry>
				
				<!-- 3. value como atributo, key como elemento -->
				<entry value="Macbook Pro">
					<key>
						<value>300</value>
					</key>
				</entry>
				
				<!-- 4. key y value como elementos -->
				<entry>
					<key>
						<value>400</value>
					</key>
					<value>Macbook AIR</value>
				</entry>
			</map>
		</property>
	</bean>
</beans>
```
    

En este ejemplo se muestran **cuatro variaciones** para definir las entradas de un `<map>`:

1. **key y value como atributos**,
    
2. **key como atributo y value como elemento**,
    
3. **value como atributo y key como elemento**,
    
4. **key y value como elementos**.
    

Cualquiera de las combinaciones es válida; se pueden mezclar según preferencia.

---

## 3️⃣ Crear y ejecutar la clase de prueba

- Copiar la clase de prueba usada en ejemplos anteriores (List/Set).
    
- Cambiar:
    
    - El bean a `"customer"`,
        
    - La ruta del archivo de configuración al nuevo XML en la carpeta `map`.
        
- En el `main`, obtener el bean:
    
```java
package com.bharath.spring.springcore.map;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcore/map/config.xml");
		Customer customer = (Customer) context.getBean("customer");
		System.out.println(customer);
	}
}

```
    
- Gracias al `toString()` implementado, se imprimirá directamente el ID y el contenido del mapa.
    

---

### Resultado

Al ejecutar la prueba, la salida muestra:

`ID: 20 {100=iPhone, 200=iPad, 300=MacBook Pro, 400=MacBook Air}`

Spring:

- **Crea el bean `Customer`**,
    
- **Inyecta el ID** y el **Map de productos**, con las claves y valores definidos.
    

---

**Resumen completo del documento:**

El ejercicio demuestra cómo **inyectar un `Map` en un bean de Spring**:

1. **Definir el bean `Customer`** con propiedades `id` y `products` (Map<Integer, String>) y un método `toString()`.
    
2. **Configurar el bean en XML**:
    
    - Usar `<property name="products"><map>...</map></property>`.
        
    - Spring permite **4 maneras** de definir las entradas de un `<map>`:
        
        - **key y value como atributos**,
            
        - **key atributo / value elemento**,
            
        - **value atributo / key elemento**,
            
        - **key y value como elementos**.
            
3. **Probar en una clase main**, obtener el bean e imprimir el objeto.
    

Spring gestiona automáticamente la creación e inyección del **Map**, demostrando su flexibilidad para manejar colecciones de tipo clave-valor de diferentes formas en la configuración XML.