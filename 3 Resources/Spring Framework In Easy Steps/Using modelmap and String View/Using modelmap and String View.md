
---

#### **Introducción**

Hasta ahora, hemos estado usando el objeto **`ModelAndView`** como tipo de retorno en los métodos del controlador.  
Sin embargo, este enfoque está **fuertemente acoplado**, ya que incluso cuando no usamos el modelo o los datos, **igualmente debemos crear un objeto `ModelAndView`** solo para devolver una vista.

Por esa razón, en versiones más recientes de Spring se introdujeron dos alternativas más simples:

- **`ModelMap`** → para manejar los datos del modelo.
    
- **`String`** → para retornar el nombre de la vista.
    

---

### 🧱 **Cuándo usar cada uno**

- Si **no necesitas enviar datos** desde el controlador, simplemente devuelve un **String** con el nombre de la vista.
    
- Si **necesitas enviar datos al modelo**, entonces usa un **`ModelMap`** como parámetro en el método del controlador.
    

El `ModelMap` funciona como un mapa clave–valor (`Map<String, Object>`).  
Se usa el método `addAttribute()` para agregar datos al modelo:

```java
model.addAttribute("clave", valor);
```

---

### 🧩 **Caso de ejemplo: Migración en el caso de registro de usuario**

En el caso práctico de registro de usuario (`UserController`), el método `showRegistrationPage()` solo devuelve una vista (`userReg.jsp`) sin usar datos.  
Anteriormente se hacía así:

```java
ModelAndView modelAndView = new ModelAndView();
modelAndView.setViewName("userReg");
return modelAndView;
```

Ahora se puede simplificar así:

```java
@RequestMapping("/registrationPage")
public String showRegistrationPage() {
    return "userReg";
}
```

✅ **Más limpio y simple**, sin necesidad de crear `ModelAndView`.

---

### 🟢 **Segundo método: registrar usuario**

El segundo método recibe los datos del formulario (`User`) y antes también usaba `ModelAndView`.  
El código antiguo era:

```java
@RequestMapping(value = "/registerUser", method = RequestMethod.POST)
public ModelAndView registerUser(@ModelAttribute("user") User user) {
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.addObject("user", user);
    modelAndView.setViewName("regResult");
    return modelAndView;
}
```

El código actualizado usa **`ModelMap`** y devuelve solo el **nombre de la vista (String)**:

```java
@RequestMapping(value = "/registerUser", method = RequestMethod.POST)
public String registerUser(@ModelAttribute("user") User user, ModelMap model) {
    model.addAttribute("user", user);
    return "regResult";
}
```

📘 **Ventajas:**

- Se eliminan objetos innecesarios (`ModelAndView`).
    
- El código es **más limpio, corto y legible**.
    
- Spring se encarga internamente de procesar la vista con el `ViewResolver`.
    

---

### 🧪 **Verificación**

Al ejecutar la aplicación:

1. Se muestra la página de registro correctamente (vista `userReg`).
    
2. Al enviar el formulario, los datos llegan al controlador.
    
3. El método `registerUser()` recibe el objeto `User`, lo agrega al modelo y devuelve la vista `regResult`.
    
4. Todo funciona igual que antes, pero con un código más claro.
    

---

## 🧠 **Resumen general de Spring MVC**

### 🔸 **Arquitectura de Spring MVC**

Spring MVC es uno de los frameworks web más usados en el ecosistema Java EE.  
Implementa el patrón **MVC (Modelo–Vista–Controlador)** mediante tres componentes clave:

|Componente|Función principal|
|---|---|
|**FrontController (DispatcherServlet)**|Intercepta todas las solicitudes entrantes.|
|**HandlerMapper**|Determina qué controlador manejará la solicitud.|
|**ViewResolver**|Localiza y genera la vista adecuada.|

---

### 🔸 **Flujo interno de una solicitud**

1. El cliente envía una solicitud HTTP.
    
2. La solicitud llega al **DispatcherServlet** (configurado en `web.xml`).
    
3. Este pasa la solicitud al **HandlerMapper**.
    
4. El HandlerMapper encuentra el **Controlador** correcto basado en la URL y lo invoca.
    
5. El **Controlador** procesa la solicitud, realiza la lógica necesaria y devuelve una **vista** junto con un **modelo (datos)**.
    
6. El **DispatcherServlet** pasa el resultado al **ViewResolver**.
    
7. El **ViewResolver** agrega el prefijo y sufijo configurado (por ejemplo `/WEB-INF/views/` + `nombreVista` + `.jsp`).
    
8. Finalmente, Spring genera la vista y envía la respuesta al navegador.
    

---

### 🔸 **Intercambio de datos entre Controlador y Vista**

- **De Controlador → Vista:**  
    Usando `ModelAndView`, o en versiones modernas, `ModelMap` + `String`.
    
- **De Vista → Controlador:**  
    Usando `@ModelAttribute`.  
    Spring convierte automáticamente los campos del formulario en un objeto Java, **si los nombres coinciden** con los atributos de la clase.
    

---

### 🔸 **Evolución del uso del modelo**

|Versión antigua|Versión moderna|
|---|---|
|`ModelAndView` (mezcla de modelo y vista)|`ModelMap` + `String` (separación clara)|
|`modelAndView.addObject("key", value)`|`model.addAttribute("key", value)`|
|`modelAndView.setViewName("view")`|`return "view"`|

---

## ✅ **Conclusión final**

- **`ModelAndView`** fue la forma tradicional de manejar datos y vistas, pero estaba acoplado.
    
- Las versiones modernas de **Spring MVC** recomiendan:
    
    - Usar **`ModelMap`** para enviar datos.
        
    - Retornar un **`String`** como nombre de vista.
        
- Esto **reduce el código, mejora la legibilidad y simplifica el mantenimiento**.
    
- El flujo MVC sigue igual: el **DispatcherServlet** coordina entre el controlador, el modelo y la vista.
    

> 💡 En resumen:  
> Spring MVC ha evolucionado hacia un diseño más limpio, en el que el **modelo (`ModelMap`)** y la **vista (`String`)** están separados, manteniendo la misma funcionalidad que antes, pero con una arquitectura **más flexible, ligera y desacoplada**.