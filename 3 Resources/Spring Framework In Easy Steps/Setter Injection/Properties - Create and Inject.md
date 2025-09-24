
---

### Inyección de **Properties** en Spring

Ahora que ya se aprendió a inyectar **List**, **Set** y **Map**, es sencillo inyectar un objeto **`Properties`** en una clase Java usando Spring.

---

## 1️⃣ Crear el POJO (Java Bean)

- Crear una clase llamada **`CountriesAndLanguages`** en el paquete:
    
    `com.bharath.springcore.properties`
    
- Agregar un único campo:
    
    `private Properties countryAndLangs;`
    
    (de `java.util.Properties`), que almacenará el **país** como clave y su **idioma nacional** como valor.
    
- Usar **Ctrl + 1** para:
    
    - Importar `java.util.Properties`.
        
    - Generar **getters y setters**.
        
- Generar también un **método `toString()`** para mostrar el contenido de `countryAndLangs`.
    
- Guardar los cambios.

```java
package com.bharath.spring.springcore.properties;

import java.util.Properties;

public class CountriesAndLanguages {

	private Properties countryAndLangs;

	public Properties getCountryAndLangs() {
		return countryAndLangs;
	}

	public void setCountryAndLangs(Properties countryAndLangs) {
		this.countryAndLangs = countryAndLangs;
	}

	@Override
	public String toString() {
		return "CountriesAndLanguages [countryAndLangs=" + countryAndLangs + "]";
	}

}
```

---

## 2️⃣ Crear el archivo de configuración XML

- Copiar el archivo de configuración utilizado en el ejemplo de `Map` junto con la clase de prueba y pegarlos en el paquete `properties`.
    
- Abrir el nuevo archivo de configuración y:
    
    - Cambiar el **nombre del bean** a:
        
        `countriesAndLangs`
        
    - Cambiar la **clase** a:
        
        `com.bharath.springcore.properties.CountriesAndLanguages`
        
    - Eliminar cualquier atributo `p:id`, ya que en este caso **no hay campo `id`**.
        
- Inyectar las propiedades:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">

	<bean name="countriesAndLangs"
		class="com.bharath.spring.springcore.properties.CountriesAndLanguages">
		<property name="countryAndLangs">
			<props>
				<prop key="USA">English</prop>
				<prop key="India">Hindi</prop>
			</props>
		</property>
	</bean>
</beans>
```

    - El elemento `<props>` permite definir varias propiedades.
        
    - Cada propiedad se declara con `<prop key="...">valor</prop>`.
        
- Formatear el archivo con **Ctrl + Shift + F**.
    

---

## 3️⃣ Ajustar la clase de prueba

- En la clase de prueba:
    
    - Cambiar la ruta del archivo de configuración a la carpeta `properties`.
        
    - Reemplazar cualquier referencia a `Customer` por:
        
        `CountriesAndLanguages`
        
    - Cambiar el nombre del bean a `"countriesAndLangs"`.
        
- Si al ejecutar aparece una excepción como:
    
    `No bean name 'customer' available`
    
    es porque todavía hay una referencia al bean anterior.  
    Corregir el nombre y volver a ejecutar.

```java
package com.bharath.spring.springcore.properties;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springcore/properties/config.xml");
		CountriesAndLanguages countriesAndLangs = (CountriesAndLanguages) context.getBean("countriesAndLangs");
		System.out.println(countriesAndLangs);
	}
}
```

---

### Resultado final

Al ejecutar la aplicación:

`CountriesAndLangs USA English India Hindi`

Spring:

- **Crea el bean `CountriesAndLanguages`**,
    
- **Inyecta el objeto `Properties`**, que contiene:
    
    - `USA = English`
        
    - `India = Hindi`.
        

---

**Resumen completo del documento:**

El documento explica cómo **inyectar un objeto `java.util.Properties` en Spring**:

1. **Crear la clase `CountriesAndLanguages`** con una propiedad de tipo `Properties` (`countryAndLangs`) y su método `toString()`.
    
2. **Configurar el bean en XML**:
    
    - Usar `<property>` con `<props>`.
        
    - Definir cada par clave–valor con `<prop key="...">valor</prop>`.
        
3. **Actualizar la clase de prueba** para:
    
    - Cargar el archivo de configuración en la carpeta `properties`.
        
    - Usar el bean `"countriesAndLangs"` en lugar de `"customer"`.
        

Al ejecutar, Spring inyecta correctamente las propiedades y muestra los países junto a sus idiomas nacionales.