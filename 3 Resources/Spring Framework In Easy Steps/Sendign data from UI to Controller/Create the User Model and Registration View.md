
---

## 🧩 **Título: Caso práctico de registro de usuario (User Registration Use Case)**

---

### 🔹 **1. Descripción general del caso**

El objetivo de este caso es implementar un **flujo completo de registro de usuario**, desde que el usuario accede al formulario en la interfaz (UI), hasta que el controlador recibe los datos, los procesa y devuelve una vista de confirmación.

Flujo general:

1. El usuario accede a una URL (por ejemplo, `/registrationPage`).
    
2. El **controlador** devuelve la vista `userReg.jsp` (el formulario de registro).
    
3. El usuario llena el formulario y lo envía.
    
4. El formulario envía los datos al controlador.
    
5. El controlador crea un objeto `User` con los datos del formulario (gracias a `@ModelAttribute`).
    
6. Finalmente, el sistema muestra una nueva vista (`regResult.jsp`) con el resultado del registro.
    

---

## 🧱 **2. Creación del modelo y la vista de registro**

### 🧩 **a. Modelo: `User.java`**

1. Crear una clase llamada `User` dentro del paquete `dto`:
    
```java
package com.bharath.spring.springmvc.dto;

public class User {

	private int id;
	private String name;
	private String email;
	
	public int getId() {
		return id;
	}
	
	public void setId(int id) {
		this.id = id;
	}
	
	public String getName() {
		return name;
	}
	
	public void setName(String name) {
		this.name = name;
	}
	
	public String getEmail() {
		return email;
	}
	
	public void setEmail(String email) {
		this.email = email;
	}
		
	@Override
	public String toString() {
		return "User [id=" + id + ", name=" + name + ", email=" + email + "]";
	}
}

```
    
2. Los campos `id`, `name` y `email` representan los datos básicos del usuario.
    

> 💡 _Regla importante:_  
> Los nombres de los campos en el formulario deben coincidir exactamente con los nombres de las variables del modelo (`id`, `name`, `email`).

---

### 🧩 **b. Vista de registro: `userReg.jsp`**

1. Crear el archivo en `WEB-INF/views/` con el nombre `userReg.jsp`.
    
2. Contenido del formulario:
    
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<form action="registerUser" method="post">
	<pre>
	Id: <input type="text" name="id" />
	Name: <input type="text" name="name" />
	Email: <input type="text" name="email" />
	<input type="submit" name="register" />
	</pre>
</form>
</body>
</html>
```
    
3. El atributo `action="registerUser"` define la URL a la que se enviará el formulario.  
    El método es `POST`, ya que se están enviando datos.
    

---

## ⚙️ **3. Creación del controlador**

### 🟢 **a. Clase `UserController.java`**

```java
package com.bharath.spring.springmvc.controller;  
import org.springframework.stereotype.Controller; 
import org.springframework.web.bind.annotation.ModelAttribute; 
import org.springframework.web.bind.annotation.RequestMapping; 
import org.springframework.web.bind.annotation.RequestMethod; 
import org.springframework.web.servlet.ModelAndView;  
import com.bharath.spring.springmvc.dto.User;  

@Controller 
public class UserController {
```

---

### 🔹 **Primer método: mostrar el formulario**

```java
@RequestMapping("/registrationPage") 
public ModelAndView showRegistrationPage() {
     ModelAndView modelAndView = new ModelAndView();     
     modelAndView.setViewName("userReg");     
     return modelAndView; 
}
```

📌 Este método:

- Se ejecuta cuando el usuario accede a `/registrationPage`.
    
- Devuelve la vista `userReg.jsp` (el formulario).
    

---

### 🔹 **Segundo método: procesar los datos del formulario**

```java
@RequestMapping(value = "/registerUser", method = RequestMethod.POST) 
public ModelAndView registerUser(@ModelAttribute("user") User user) {     
	System.out.println(user);     
	ModelAndView modelAndView = new ModelAndView();     
	modelAndView.addObject("user", user);     
	modelAndView.setViewName("regResult");     
	return modelAndView; 
}
```

📘 **Explicación:**

- `@ModelAttribute("user")` le dice a Spring que cree un objeto `User` y asigne los valores del formulario.
    
- Los nombres de los campos (`id`, `name`, `email`) deben coincidir con los atributos de la clase `User`.
    
- Luego, se imprime el objeto en consola y se devuelve una vista llamada `regResult`.
    

```java
package com.bharath.spring.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.bharath.spring.springmvc.dto.User;

@Controller
public class UserController {

	@RequestMapping("/registrationPage") 
	public ModelAndView showRegistrationPage() {
	     ModelAndView modelAndView = new ModelAndView();     
	     modelAndView.setViewName("userReg");     
	     return modelAndView; 
	}
	
	@RequestMapping(value = "/registerUser", method = RequestMethod.POST) 
	public ModelAndView registerUser(@ModelAttribute("user") User user) {     
		System.out.println(user);     
		ModelAndView modelAndView = new ModelAndView();     
		modelAndView.addObject("user", user);     
		modelAndView.setViewName("regResult");     
		return modelAndView; 
	}
	
}

```
---

## 🧩 **4. Vista de respuesta: `regResult.jsp`**

Este archivo mostrará el mensaje de confirmación y los datos del usuario.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>User Registration Response</title>
</head>
<body>
	User Registered successfully.User Details are:
	<%=request.getAttribute("user")%>
</body>
</html>
```

Al renderizar, el método `toString()` del objeto `User` mostrará los datos del usuario registrados.

**Ejemplo de salida:**

`User Registered Successfully! User details are: User [id=1234, name=John, email=john@bob.com]`

---

## 🚀 **5. Flujo completo de la aplicación**

|Etapa|Descripción|Componente Spring|
|---|---|---|
|**1. Acceso inicial**|El usuario visita `/registrationPage`.|`showRegistrationPage()` devuelve `userReg.jsp`.|
|**2. Envío del formulario**|El usuario llena los datos y presiona "Register".|El formulario envía un POST a `/registerUser`.|
|**3. Creación del modelo**|Spring crea automáticamente un objeto `User` con los valores del formulario.|`@ModelAttribute`|
|**4. Procesamiento**|El controlador recibe el objeto `User` y lo imprime.|`registerUser()`|
|**5. Respuesta final**|Se devuelve la vista `regResult.jsp` con el objeto `user`.|`ModelAndView`|

---

## 🧠 **6. Resumen general**

|Elemento|Descripción|
|---|---|
|**Formulario (UI)**|En `userReg.jsp`, el usuario ingresa `id`, `name`, y `email`.|
|**Controlador**|Tiene dos métodos: uno para mostrar el formulario y otro para procesar los datos.|
|**Anotación clave**|`@ModelAttribute` — Mapea automáticamente los campos del formulario al objeto `User`.|
|**Modelo (User)**|Clase DTO con `id`, `name`, `email`, getters/setters y `toString()`.|
|**Vistas**|`userReg.jsp` (formulario) y `regResult.jsp` (resultado).|
|**Respuesta final**|Se muestra un mensaje de éxito con los datos del usuario.|

---

### ✅ **Conclusión**

Con este ejercicio se completa un **flujo MVC completo en Spring**:

- **UI → Controlador:** mediante un formulario HTML.
    
- **Controlador → UI:** mediante `ModelAndView`.
    
- Uso de `@ModelAttribute` para la **vinculación automática de datos**.
    
- Separación clara entre **modelo, vista y controlador**.
    

> 💡 En resumen:  
> Este caso práctico demuestra el funcionamiento completo de **Spring MVC**, permitiendo enviar datos del usuario al controlador, procesarlos en un modelo (`User`) y devolver una vista de respuesta (`regResult.jsp`) con los resultados del registro.