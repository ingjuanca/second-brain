
---
## 🧩 **Título: Introducción a los parámetros de solicitud (Request Parameters) en Spring MVC**

---

### 🔹 **1. Segunda forma de enviar datos desde la UI al Controlador**

Hasta ahora, aprendiste que los datos se pueden enviar desde la UI al Controlador usando **formularios HTML** y la anotación `@ModelAttribute`.

Esta lección explica la **segunda forma** de hacerlo: usando **parámetros de solicitud (query parameters)**, también conocidos como **parámetros de URL**.

---

### 🧱 **2. ¿Qué son los parámetros de solicitud?**

Los parámetros de solicitud se envían **adjuntando datos al final de la URL**, después de un signo de **interrogación (?)**, en formato **clave=valor**.  
Cada par se separa con un **ampersand (&)**.

📘 **Ejemplo:**

`http://localhost:8080/springmvc/showData?id=123&name=John&sal=72.5`

Aquí:

- `id` es la primera clave, con valor `123`
    
- `name` es la segunda, con valor `John`
    
- `sal` es la tercera, con valor `72.5`
    

---

### 🟢 **3. Cómo acceder a esos datos en el Controlador**

Spring MVC ofrece la anotación **`@RequestParam`** para recuperar automáticamente los valores enviados en la URL.

Ejemplo:

`@RequestParam("id") int id`

Spring internamente realiza los siguientes pasos:

1. Llama a `request.getParameter("id")` para obtener el valor de la URL.
    
2. Convierte automáticamente el valor (que siempre llega como texto) al tipo de dato correspondiente, usando los métodos de parseo apropiados (`Integer.parseInt()`, `Double.parseDouble()`, etc.).
    
3. Asigna ese valor convertido al parámetro del método en el Controlador.
    

Así, el desarrollador **no necesita manipular el objeto `HttpServletRequest`** ni hacer conversiones manuales.

---

### ⚠️ **4. Manejo de errores y opciones adicionales**

#### a) **Error por tipo de dato incorrecto**

Si el tipo en la URL no coincide con el tipo declarado, Spring lanzará un **error 400 (Bad Request)**.

📘 Ejemplo:

```java
@RequestParam("id") 
int id
```

Si el usuario envía `?id=abc`, el valor `"abc"` no puede convertirse a número y Spring devolverá un error 400.

---

#### b) **Error por falta de parámetro**

Por defecto, los parámetros marcados con `@RequestParam` son **obligatorios**.  
Si el parámetro no viene en la URL, también se lanzará un error 400.

📘 Ejemplo:

```java
@RequestParam("id") 
int id
```

Si la URL no tiene `id`, Spring devolverá error 400.

Para evitarlo, se puede hacer opcional el parámetro:

```java
@RequestParam(value = "id", required = false)
```

---

#### c) **Valores por defecto**

Puedes definir un valor por defecto en caso de que no se proporcione ningún valor en la URL.

```java
@RequestParam(value = "sal", defaultValue = "0.0") 
double salary
```

Así, si no se especifica `sal`, Spring usará `0.0`.

---

#### d) **El orden no importa**

Spring **no depende del orden** de los parámetros en la URL.  
Lo importante es que las **claves coincidan** con las usadas en `@RequestParam`.

Por ejemplo, ambas URLs son válidas:

`...?id=1&name=John&sal=70.5 ...?sal=70.5&id=1&name=John`

---

## 🧩 **5. Ejemplo práctico con `@RequestParam`**

### **a. Crear el controlador**

```java
@Controller 
public class RequestParamsController {      
	@RequestMapping("/showData")     
	public ModelAndView showData(
		@RequestParam("id") int id, 
		@RequestParam("name") String name, 
		@RequestParam("sal") double salary
		) {          
		
		System.out.println("Id: " + id);         
		System.out.println("Name: " + name);         
		System.out.println("Salary: " + salary);          
		ModelAndView modelAndView = new ModelAndView();         
		modelAndView.setViewName("userReg"); // o cualquier vista         
		return modelAndView;     } 
		}
```

📘 **Explicación:**

- `@RequestMapping("/showData")` define la URL de acceso.
    
- Cada parámetro del método se marca con `@RequestParam("clave")`.
    
- Spring extrae automáticamente los valores de la URL, los convierte al tipo correcto y los pasa al método.
    
- Los valores se imprimen en consola (o podrían enviarse a una vista).
    

---

### **b. Cómo invocar desde el navegador**

URL de ejemplo:

`http://localhost:8080/springmvc/showData?id=123&name=John&sal=72.5`

Resultado en la **consola de Tomcat**:

`Id: 123 Name: John Salary: 72.5`

Spring convierte automáticamente los parámetros a los tipos apropiados (`int`, `String`, `double`) sin requerir código adicional.

---

## 🧠 **6. Resumen general**

|Elemento|Descripción|
|---|---|
|**Método de envío**|Parámetros adjuntos a la URL (`?clave=valor&clave2=valor2`).|
|**Anotación clave**|`@RequestParam("clave")`.|
|**Conversión automática**|Spring convierte `String` → tipo Java (`int`, `double`, etc.).|
|**Parámetros requeridos**|Por defecto `true`. Se puede cambiar con `required = false`.|
|**Valor por defecto**|Usar `defaultValue = "..."`.|
|**Orden de parámetros**|No importa, lo que importa es el nombre de la clave.|
|**Error común**|400 Bad Request cuando falta un parámetro o hay tipo inválido.|

---

## ✅ **Conclusión final**

El uso de `@RequestParam` en Spring MVC permite **recibir datos directamente desde la URL** sin necesidad de formularios.

Spring:

- Lee los valores de la URL (`request.getParameter()` internamente).
    
- Los convierte automáticamente al tipo de dato correcto.
    
- Los inyecta en los parámetros del método del controlador.
    

> 💡 En resumen:  
> `@RequestParam` es una forma sencilla, rápida y limpia de obtener datos de la URL, ideal para pruebas, filtros, búsquedas o acciones que no requieren formularios HTML.