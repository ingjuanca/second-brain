
---

## Uso de la anotación **@Value** en Spring

En esta lección se explica cómo usar **@Value** para inyectar valores en beans de Spring, cuando se trabaja con anotaciones en lugar de configuración XML.

---

### 1️⃣ Inyección de **tipos primitivos**

- Si no se configuran valores en XML o anotaciones, los atributos toman **valores por defecto**.
    
- Con `@Value`, se pueden asignar directamente valores a atributos primitivos:
    

```java
@Value("Java Architect and Instructor")
private String title;  
@Value("Bharath Thippireddy") private String name;
```

- La anotación debe importarse desde:
    
    `org.springframework.beans.factory.annotation.Value`
    
- Los valores proporcionados con `@Value` **tienen prioridad** sobre los valores asignados directamente en el código (por ejemplo, en inicializaciones).
    

Ejemplo:  
Si en la clase se asigna `id = 15` y `name = "John"`, pero en las anotaciones se usa `10` y `"Bharath"`, los valores finales serán los de `@Value`.

```java
package com.bharath.spring.springcoreadvanced.stereotype.annotations;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class Profile {

	@Value("Java Architect and Instructor")
	private String title;
	@Value("Vivekananda Consulting")
	private String company;

	@Override
	public String toString() {
		return "Profile [title=" + title + ", company=" + company + "]";
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getCompany() {
		return company;
	}

	public void setCompany(String company) {
		this.company = company;
	}

}

```
---

### 2️⃣ Inyección de **colecciones**

Para inyectar listas, sets o mapas con `@Value`, se siguen **dos pasos**:

#### Paso 1: Definir la colección en XML con `util:schema`

`<util:list id="topics" list-class="java.util.LinkedList">     <value>Java Web Services</value>     <value>Core Java</value>     <value>XSLT</value> </util:list>`

- Se debe importar el **namespace `util`** en el XML.
    
- Se da un **id** a la colección, en este caso `topics`.
    

#### Paso 2: Referenciar la colección en el POJO con `@Value`

`@Value("#{topics}") private List<String> topics;`

- Aquí se usa **Spring Expression Language (SpEL)**.
    
- La sintaxis es:
    
    - `#{idDelBean}`
        
    - El `id` corresponde al definido en el XML (`topics`).
        
- Al ejecutar, Spring inyecta la colección definida en el archivo XML dentro del bean `Instructor`.
    

---

### 3️⃣ Inyección de **objetos (dependencias)**

- Para inyectar objetos de otras clases, se utiliza **@Autowired**.
    
- `@Value` es principalmente para primitivos y colecciones.
    

---

## Resumen completo del documento

El documento explica cómo usar la anotación **@Value** en Spring para inyectar valores cuando se trabaja con configuración basada en anotaciones:

1. **Tipos primitivos:**
    
    - Se usan valores literales dentro de `@Value("...")`.
        
    - Ejemplo: `@Value("10") private int id;`.
        
    - Estos valores **anulan** cualquier asignación hecha directamente en el código.
        
2. **Colecciones (List, Set, Map):**
    
    - Se definen en el archivo XML mediante `util:schema` (`util:list`, `util:set`, `util:map`).
        
    - Se da un `id` a la colección.
        
    - En la clase se referencia con `@Value("#{idColeccion}")` usando **SpEL**.
        
    - Spring inyecta la colección definida en el XML en el bean correspondiente.
        
3. **Objetos (clases dependientes):**
    
    - No se usa `@Value`, sino `@Autowired`.
        

En conclusión, **@Value** permite asignar valores a atributos primitivos y colecciones en beans de Spring sin necesidad de usar XML directamente, simplificando la configuración cuando se emplean anotaciones.

---

¿Quieres que te prepare un **ejemplo consolidado** con los tres casos (primitivos, colección y objeto) para que tengas un modelo práctico en una sola clase?