
---
### Inyección de un **Set** en Spring

En esta lección se explica cómo **crear una instancia de `Set`** e inyectarla en una clase usando **Spring**, siguiendo **tres pasos**:

---

## 1️⃣ Crear el POJO (Java Bean)

- Crear la clase **`CarDealer`** con:
    
    - `private String name;` – nombre del concesionario,
    - `private Set<String> models;` – conjunto de modelos de autos que vende.
     
- Generar **getters y setters** para ambos campos.
    
- Formatear el código (Ctrl + Shift + F).

```java
package com.bharath.spring.springcore.set;

import java.util.Set;

public class CarDealer {

	private String name;
	private Set<String> models;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public Set<String> getModels() {
		return models;
	}

	public void setModels(Set<String> models) {
		this.models = models;
	}

}
```

---

## 2️⃣ Crear el archivo de configuración XML

- Copiar el archivo de configuración usado para la lista (`listconfig.xml`) y renombrarlo a **`setconfig.xml`**.
    
- Editar el bean:
    
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">

	<bean name="cardealer" class="com.bharath.spring.springcore.set.CarDealer">
		<property name="name">
			<value>Hyderabad Dealer</value>
		</property>
		<property name="models">
			<set> 
				<value>Honda</value>
				<value>BMW</value>
				<value>Honda</value>
			</set>
		</property>
	</bean>

</beans>
    ```
    
- Diferencias principales frente a `<list>`:
    
    - `<set>` **no permite valores duplicados**.
        
    - La inyección se hace igual que en listas, pero garantizando elementos únicos.
        

---

## 3️⃣ Crear y ejecutar la clase de prueba

- Copiar la clase de prueba usada para el ejemplo de lista.
    
- Cambiar:
    
    - Nombre del bean a **`carDealer`**,
        
    - Ruta del archivo a **`setconfig.xml`**,
        
    - Tipo de objeto a **`CarDealer`**.

```java
package com.bharath.spring.springcore.set;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcore/set/setconfig.xml");
		CarDealer carDealer = (CarDealer) context.getBean("cardealer");
		System.out.println(carDealer.getName());
		System.out.println(carDealer.getModels().getClass().getName());

	}

}

```

- En el `main`, imprimir:
    
    `System.out.println(carDealer.getName()); System.out.println(carDealer.getModels());`
    
- Ejecutar la prueba:
    
    - Si se usa el nombre de archivo incorrecto, Spring lanzará un error indicando que no encuentra el archivo de configuración.
        
    - Al corregirlo, la salida mostrará:
        
        `Hyderabad Dealer [Honda, BMW, Hyundai]`
        

---

### Más detalles sobre la inyección de Set

- **Un solo valor:**  
    Si se quiere inyectar **solo un elemento**, se puede omitir la etiqueta `<set>` y colocar el `<value>` directamente dentro de `<property>`.
    
- **Duplicados:**  
    Si se declaran valores duplicados (por ejemplo, dos veces "Honda"), **Spring inyectará solo una instancia**, porque un `Set` en Java **no permite duplicados**.
    
- **Implementación por defecto:**
    
    - Para conocer el tipo de `Set` que Spring crea por defecto, se puede imprimir:
        
        `carDealer.getModels().getClass().getName()`
        
    - Spring usa **`LinkedHashSet`** como implementación predeterminada.
        

---

**Resumen completo del documento:**

La lección enseña a **inyectar colecciones de tipo Set en Spring**:

1. **Crear el bean `CarDealer`** con propiedades `name` y `models` (Set de Strings).
    
2. **Definir el bean en `set-config.xml`**, usando:
    
    - `<property name="name"><value>Hyderabad Dealer</value></property>`
        
    - `<property name="models"><set>...valores...</set></property>`
        
3. **Probar la inyección** con una clase de prueba que cargue el archivo XML y muestre las propiedades.
    

Spring:

- Crea el bean y **inyecta el Set automáticamente**,
    
- Ignora los valores duplicados,
    
- Usa por defecto un **LinkedHashSet**.
    

Así, Spring permite manejar de forma sencilla colecciones de tipo **Set**, garantizando elementos únicos y simplificando la configuración.