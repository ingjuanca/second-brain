
---

### 🟢 **1. Envío de tipos primitivos**

En esta parte se enseña cómo pasar datos simples desde el controlador a la interfaz usando `ModelAndView`.

#### **Pasos:**

1. En el controlador (`HelloController`), después de configurar la vista con `setViewName`, se agregan datos al modelo usando:
    
```java
    modelAndView.addObject("id", 123); 
    modelAndView.addObject("name", "Bharath"); 
    modelAndView.addObject("salary", 10000);
```
    
    - El primer parámetro es la **clave (String)**.
        
    - El segundo parámetro es el **valor (Object)**: puede ser cualquier tipo de dato (int, String, double, etc.).
        
2. Estos valores se almacenan en el **objeto de solicitud HTTP (`HttpServletRequest`)**, y pueden ser accedidos desde la vista JSP mediante:
    
    `request.getAttribute("id")`
    

```java
package com.bharath.spring.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class HelloController {

	@RequestMapping("/hello")
	public ModelAndView hello() {
		ModelAndView modelAndView = new ModelAndView();
		modelAndView.setViewName("hello");
		modelAndView.addObject("id", 123);
		modelAndView.addObject("name", "Bharath");
		modelAndView.addObject("salary", 10000);
		return modelAndView;
	}

}

```

---

### 🟡 **2. Mostrando datos en la vista (JSP)**

#### **Método 1: Usando scriptlets JSP**

En el archivo `hello.jsp`:

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>

<html>
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	<title>Hello</title>
</head>
<body>
	<%
		Integer id = (Integer) request.getAttribute("id");
		String name = (String) request.getAttribute("name");
		Integer salary = (Integer) request.getAttribute("salary");
		out.println("ID: " + id);
		out.println("Name: " + name);
		out.println("salary: " + salary);
	%>
</body>
</html>
```

- Se recuperan los valores desde el `request`.
    
- Se hace **casting** al tipo adecuado.
    
- Se muestran los resultados usando `out.println()`.
    

Resultado esperado en el navegador:

```bash
Id: 123 Name: Bharath Salary: 10000.0
```

---

#### **Método 2: Usando JSP Expression Language (EL)**

Otra forma más limpia es usar **JSP Expression Language (EL)**.  
Se escribe directamente en la página:

```jsp
<b>Id:</b> ${id} 
<br/> 
<b>Name:</b> ${name} 
<br/> 
<b>Salary:</b> ${salary}
```

- `${}` permite acceder a variables almacenadas en el `request`, `session` o `application`.
    
- JSP automáticamente busca la variable en el **ámbito (scope)** correspondiente y muestra su valor.
    

📌 _Importante:_  
Para que EL funcione correctamente, **debes eliminar la línea `<!DOCTYPE html>` generada automáticamente por Eclipse**, ya que puede impedir su evaluación.

### ✅ **En resumen**

|Método|Necesita `request.getAttribute()`|Conversión de tipo|Recomendado|
|---|---|---|---|
|**Scriptlet JSP**|✅ Sí|✅ Manual (casting necesario)|❌ Obsoleto|
|**Expression Language (EL)**|❌ No|❌ Automática|✅ Recomendado|

---

### 🟠 **3. Envío de objetos desde el controlador**

#### **Paso 1: Crear clase DTO**

Se crea una clase `Employee` dentro de un paquete `dto`:

```java
package com.bharath.spring.springmvc.dto;  
public class Employee {     
	private int id;     
	private String name;     
	private double salary;      
	// Getters y setters     
	// toString() 
}
```

#### **Paso 2: Crear un nuevo controlador**

```java
package com.bharath.spring.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import com.bharath.spring.springmvc.dto.Employee;

@Controller
public class ObjectController {

	@RequestMapping("/readObject")
	public ModelAndView sendObject() {

		ModelAndView modelAndView = new ModelAndView();
		modelAndView.setViewName("displayObject");

		Employee employee = new Employee();
		employee.setId(1234);
		employee.setName("John");
		employee.setSalary(8000);
		modelAndView.addObject("employee", employee);

		return modelAndView;
	}
}

```

---

#### **Paso 3: Crear la vista (`displayObject.jsp`)**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Object Details</title>
</head>
<body>
	<%=request.getAttribute("employee")%>
</body>
</html>
```

- Aquí, `employee` es el nombre asignado en `addObject()`.
    
- JSP mostrará la representación del objeto (`toString()`).
    

**Resultado esperado:**

`Employee [id=1234, name=John, salary=8000.0]`

---

### 🔵 **4. Envío de listas (colecciones) desde el controlador**

#### **Paso 1: Crear un nuevo controlador para listas**

```java
package com.bharath.spring.springmvc.controller;

import java.util.ArrayList;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import com.bharath.spring.springmvc.dto.Employee;

@Controller
public class ListController {

	@RequestMapping("/readList")
	public ModelAndView sendList() {

		ModelAndView modelAndView = new ModelAndView();
		modelAndView.setViewName("displayList");

		Employee employee = new Employee();
		employee.setId(1234);
		employee.setName("John");
		employee.setSalary(8000);

		Employee employee2 = new Employee();
		employee2.setId(2);
		employee2.setName("Bharath");
		employee2.setSalary(10000);

		Employee employee3 = new Employee();
		employee3.setId(3);
		employee3.setName("Bob");
		employee3.setSalary(7000);

		ArrayList<Employee> employees = new ArrayList<Employee>();
		employees.add(employee);
		employees.add(employee2);
		employees.add(employee3);

		modelAndView.addObject("employees", employees);

		return modelAndView;
	}
}

```

---

#### **Paso 2: Crear la vista JSP (`displayList.jsp`)**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"
	import="com.bharath.spring.springmvc.dto.Employee,java.util.List"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%
		List<Employee> employees = (List<Employee>) request.getAttribute("employees");
		for (Employee e : employees) {
			out.println(e.getId());
			out.println(e.getName());
		}
	%>
</body>
</html>
```

**Resultado en el navegador:**

`Id: 1 - Name: John Id: 2 - Name: Bharath Id: 3 - Name: Bob`

---

### 🧠 **Resumen general**

|Tipo de dato enviado|Método en `ModelAndView`|Ejemplo de clave|Vista JSP usa|Resultado|
|---|---|---|---|---|
|**Primitivo**|`addObject("id", 123)`|`"id"`|`${id}` o `request.getAttribute("id")`|Muestra valores simples|
|**Objeto**|`addObject("employee", employee)`|`"employee"`|`<%= request.getAttribute("employee") %>`|Muestra el `toString()` del objeto|
|**Lista de objetos**|`addObject("employees", list)`|`"employees"`|Bucle `for-each` en JSP|Muestra todos los objetos de la lista|

---

### ✅ **Conclusión final**

En **Spring MVC**, el intercambio de datos entre el **Controlador** y la **Interfaz (JSP)** se logra usando el objeto `ModelAndView`.  
El método `addObject()` permite enviar cualquier tipo de información:

- **Datos primitivos** (int, String, double, etc.)
    
- **Objetos** (como `Employee`)
    
- **Colecciones** (`List`, `Map`, etc.)
    

En las vistas JSP, los datos pueden mostrarse usando:

1. **Scriptlets JSP** (`request.getAttribute("clave")`)
    
2. **JSP Expression Language (EL)** (`${clave}`)
    

> 💡 En resumen:  
> Spring MVC facilita el flujo de datos **Controlador → Vista**, manteniendo el código limpio, flexible y orientado a objetos, sin necesidad de manipular directamente el `HttpServletRequest` ni escribir lógica de presentación dentro del backend.