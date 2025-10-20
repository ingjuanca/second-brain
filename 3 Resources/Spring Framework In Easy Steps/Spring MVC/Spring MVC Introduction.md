
---

**Spring MVC** se utiliza para diseñar **aplicaciones web dinámicas**.  
Internamente, emplea tres patrones de diseño importantes:

1. **Front Controller**
    
2. **Handler Mapper**
    
3. **View Resolver**
    

Estos componentes trabajan juntos para implementar el patrón **MVC (Model-View-Controller)**.

---

### 🔸 **Flujo de trabajo de Spring MVC**

Cuando un cliente envía una **solicitud HTTP**, el primer componente que la recibe es el **Dispatcher Servlet**, el cual es una **implementación del patrón Front Controller**.

- Este servlet se coloca **al frente de la aplicación** y maneja **todas las solicitudes entrantes**.
    
- Se configura dentro del archivo `web.xml` (descriptor de despliegue de la aplicación web).
    
- No es necesario escribir un servlet propio, ya que **Spring lo proporciona listo para usar**; solo se debe configurar.
    

---

### 🔸 **Paso 1: Dispatcher Servlet**

El **Dispatcher Servlet** intercepta cada solicitud que llega al servidor.  
Su función es **dirigir la solicitud al controlador correcto** con la ayuda del **Handler Mapper**.

---

### 🔸 **Paso 2: Handler Mapper**

El **Handler Mapper** (también proporcionado por Spring):

- Determina **qué clase controladora (Controller)** debe manejar la solicitud.
    
- Realiza esta asociación según el **patrón de URL** recibido.
    
- Devuelve la información al **Dispatcher Servlet** para que invoque el controlador adecuado.
    

---

### 🔸 **Paso 3: Controller**

El **Controller** es una clase **POJO** (Plain Old Java Object) creada por el desarrollador y marcada con la anotación:

`@Controller`

Dentro de esta clase:

- Se define un **método** que maneja la solicitud.
    
- Este método **crea y devuelve** un objeto de tipo **`ModelAndView`**.
    

**ModelAndView:**

- **Model** representa los datos que se mostrarán en la vista (puede ser opcional).
    
- **View** es el nombre lógico de la página que se debe mostrar al usuario (por ejemplo, “hello”).
    

El método devuelve este objeto `ModelAndView` al **Dispatcher Servlet**.

---

### 🔸 **Paso 4: View Resolver**

El **Dispatcher Servlet** recibe el **nombre de la vista** desde el controlador y lo pasa al **View Resolver**.

El **View Resolver**:

- Es otro componente esencial de Spring MVC.
    
- Se encarga de **localizar la vista real (archivo JSP, JSF, Thymeleaf, etc.)**.
    
- Toma el nombre de la vista y **le agrega un prefijo y un sufijo**, por ejemplo:
    
    `Prefijo: /WEB-INF/pages/ Nombre de la vista: hello Sufijo: .jsp`
    
    Resultado final → `/WEB-INF/pages/hello.jsp`
    

De esta forma, el controlador no está acoplado directamente a una vista física.  
Si en el futuro se cambia el motor de vista (por ejemplo, de JSP a JSF o Thymeleaf), **solo se actualiza la configuración**, sin modificar el controlador.

---

### 🔸 **Paso 5: Renderización de la vista**

Finalmente:

1. El **View Resolver** devuelve la vista completa al **Dispatcher Servlet**.
    
2. El **Dispatcher Servlet** pasa el **Model** (si existe) a la vista.
    
3. La vista utiliza esos datos y **renderiza la página** que se enviará al cliente.
    

---

### 🔸 **Resumen del flujo completo**

|Paso|Componente|Descripción|
|---|---|---|
|**1**|**Dispatcher Servlet**|Intercepta todas las solicitudes HTTP (Front Controller).|
|**2**|**Handler Mapper**|Determina qué controlador manejará la solicitud.|
|**3**|**Controller**|Procesa la solicitud, crea el modelo y define el nombre de la vista.|
|**4**|**View Resolver**|Agrega prefijo y sufijo al nombre de la vista para ubicar el archivo real.|
|**5**|**Vista (View)**|Genera la página final con los datos y la devuelve al cliente.|

---

### 🔸 **Ejemplo ilustrativo**

Supongamos que un controlador devuelve:

`return new ModelAndView("hello");`

El **View Resolver** podría tener esta configuración en el archivo de Spring:

```xml 
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<property name="prefix" value="/WEB-INF/pages/" />     
	<property name="suffix" value=".jsp" /> 
</bean>
```

Entonces, el **Dispatcher Servlet** mostrará finalmente:

`/WEB-INF/pages/hello.jsp`

Si más adelante se cambia la tecnología de vistas (por ejemplo, a JSF o Thymeleaf), solo se modifica el sufijo:

`<property name="suffix" value=".xhtml" />`

y la aplicación seguirá funcionando sin alterar el código Java.

---

### ✅ **Conclusión general**

**Spring MVC** implementa el patrón **Modelo-Vista-Controlador (MVC)** usando componentes reutilizables y configurables:

- El **Dispatcher Servlet** actúa como **Front Controller**, recibiendo y distribuyendo las solicitudes.
    
- El **Handler Mapper** elige el **controlador adecuado** según la URL.
    
- El **Controller** procesa la lógica del negocio y prepara los datos (modelo).
    
- El **View Resolver** decide **qué vista mostrar** y **dónde está ubicada**.
    
- Finalmente, el **modelo** se envía a la vista, y esta **genera la respuesta HTML** para el usuario.
    

---

### 🧠 **Resumen técnico final**

|Concepto|Función principal|Configuración/Anotación|
|---|---|---|
|**Dispatcher Servlet**|Controlador frontal que maneja todas las peticiones HTTP.|Configurado en `web.xml`.|
|**Handler Mapper**|Determina el controlador basado en el patrón de URL.|Automático por Spring.|
|**Controller**|Clase POJO que maneja la lógica y devuelve `ModelAndView`.|`@Controller`|
|**ModelAndView**|Contiene los datos (`Model`) y el nombre de la vista (`View`).|Retornado por el controlador.|
|**View Resolver**|Ubica la vista real combinando prefijo y sufijo.|Definido en configuración XML.|
|**View**|Página que muestra el resultado al usuario final.|Ejemplo: JSP, Thymeleaf, JSF.|

---

### 💡 **En resumen:**

Spring MVC organiza el flujo de una aplicación web de la siguiente forma:

> **Cliente → Dispatcher Servlet → Handler Mapper → Controller → View Resolver → Vista (HTML/JSP/JSF) → Cliente**

Gracias a esta arquitectura:

- Todo está **desacoplado y modular**.
    
- Cambiar vistas o controladores no afecta el resto del sistema.
    
- El mantenimiento y la extensibilidad son mucho más sencillos.
    

👉 En pocas palabras, **Spring MVC convierte la compleja estructura de una aplicación web en un flujo ordenado, limpio y fácilmente configurable.**