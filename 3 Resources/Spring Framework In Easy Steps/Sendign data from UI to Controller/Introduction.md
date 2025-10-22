
---

En esta lección aprenderás cómo enviar datos desde la **interfaz de usuario (UI)** hacia el **controlador**.

Esto se puede hacer de **dos maneras**:

1. **Usando un formulario HTML.**
    
2. **Usando parámetros en la URL (query parameters).**
    

Primero, se analizará el método con **formularios HTML**.

---

### 🟢 **Cómo funciona el proceso con formularios HTML**

Cuando enviamos un formulario desde el navegador, el **contenedor de Spring** realiza automáticamente **cuatro pasos**:

1. **Lee los datos enviados** en la solicitud utilizando internamente los métodos `request.getParameter()`.
    
2. **Convierte los valores recibidos** (que son texto) en sus tipos de datos adecuados:
    
    - Por ejemplo:
        
        - `Integer.parseInt()` para números enteros
            
        - `Double.parseDouble()` para decimales
            
        - Y otros métodos `parse` según el tipo correspondiente.
            
3. **Crea un objeto de la clase modelo (por ejemplo, `User`)** que hemos definido previamente.
    
    - El contenedor crea una instancia de esa clase.
        
    - Luego, **asigna automáticamente** a cada atributo del objeto los valores enviados en el formulario.
        
4. Finalmente, el contenedor **entrega ese objeto al controlador**, listo para ser utilizado en el método correspondiente.
    

---

### 🧱 **Reglas importantes para que esto funcione**

Para que Spring pueda crear correctamente el objeto con los datos del formulario, se deben seguir dos reglas fundamentales:

1. **Los nombres de los campos en el formulario HTML** deben coincidir **exactamente** con los nombres de las variables en la clase modelo (por ejemplo, `name`, `email`, `age`).
    
2. **La cantidad de campos** en el formulario debe coincidir con **la cantidad de atributos** en la clase modelo.
    

📌 Si los nombres y la cantidad de campos coinciden, el contenedor podrá:

- Leer los datos del formulario.
    
- Crear el objeto.
    
- Asignar automáticamente los valores.
    
- Pasarlo como parámetro al método del controlador.
    

---

### 🧩 **Cómo acceder al objeto en el controlador**

Dentro del **Controlador**, para recibir el objeto con los datos del formulario, se usa la anotación **`@ModelAttribute`** en los parámetros del método.

Ejemplo:

```java
@Controller 
public class UserController {      
	@RequestMapping("/register")     
	public String registerUser(@ModelAttribute User user) {         
		System.out.println(user);         
	return "success";     
	} 
}
```

👉 Aquí:

- Spring crea automáticamente un objeto `User` con los datos del formulario.
    
- Lo inyecta como argumento del método usando `@ModelAttribute`.
    
- El programador puede usar el objeto directamente (sin usar `request.getParameter()`).
    

---

### 🧠 **Resumen general**

|Concepto|Descripción|
|---|---|
|**Dirección del flujo de datos**|De la UI (formulario) → al Controlador.|
|**Métodos disponibles**|1️⃣ Formulario HTML, 2️⃣ Parámetros de consulta (Query Params).|
|**Conversión de datos**|Spring convierte automáticamente los valores enviados en sus tipos Java (`int`, `double`, etc.).|
|**Objeto modelo**|Clase Java (por ejemplo, `User`) con campos que coinciden con los nombres de los inputs del formulario.|
|**Anotación clave**|`@ModelAttribute` — vincula automáticamente los datos del formulario con el objeto modelo.|
|**Ventaja principal**|El controlador trabaja directamente con objetos, sin usar `request.getParameter()`.|

---

### ✅ **Conclusión final**

En **Spring MVC**, existen dos formas de enviar datos desde la UI al Controlador:

1. **Mediante un formulario HTML.**
    
2. **Mediante parámetros en la URL.**
    

Cuando se usa un formulario:

- El contenedor Spring lee automáticamente los datos enviados.
    
- Los convierte al tipo adecuado.
    
- Crea un objeto del modelo (como `User`).
    
- Llena sus campos con los valores del formulario.
    
- Entrega el objeto directamente al controlador mediante la anotación `@ModelAttribute`.
    

> 💡 En resumen:  
> **Spring MVC automatiza completamente el proceso de captura, conversión y asignación de datos del formulario**, permitiendo que el desarrollador trabaje directamente con objetos Java, sin escribir código manual como `request.getParameter()`.