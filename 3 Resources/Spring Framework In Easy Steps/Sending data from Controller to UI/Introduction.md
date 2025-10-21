
---

**Introducción**

Tomemos cualquier aplicación web —ya sea Gmail, Facebook, Instagram u otra—, todas se basan en **intercambiar información entre el usuario final y la aplicación**, es decir, entre la **interfaz de usuario (UI)** y la **aplicación backend**.

En el mundo de **Spring MVC**, existen **dos formas principales de intercambiar datos**:

1. **Del controlador hacia la UI.**
    
2. **De la UI hacia el controlador.**
    

En esta sección se explorarán ambos mecanismos, comenzando con **cómo enviar datos desde el controlador hacia la interfaz de usuario**.

---

### 🔸 **Enviar datos del controlador a la UI**

Para enviar datos desde el **controlador** hacia la **vista (UI)**, se utiliza el objeto **`ModelAndView`**.

El flujo es el siguiente:

1. Se crea un objeto de tipo `ModelAndView`.
    
2. Para incluir datos dentro de él, se usa el método:
    
    `addObject(String key, Object value)`
    
    - El **parámetro `key`** es un nombre de tipo `String` que servirá para identificar el dato.
        
    - El **parámetro `value`** puede ser **cualquier tipo de objeto Java** (primitivo, colección, POJO, etc.).
        
3. Una vez agregado el dato con `addObject`, este se almacena internamente en el **objeto de solicitud HTTP (`HttpServletRequest`)**.
    
4. Cuando la solicitud llega a la vista (por ejemplo, una página JSP), se puede acceder a los datos utilizando:
    
    `request.getAttribute("key")`
    
    Al pasar la clave (key), se obtiene el valor correspondiente.
    

---

### 🔸 **Tipos de datos que se pueden enviar**

Los valores que se agregan al `ModelAndView` pueden ser de cualquier tipo:

- **Tipos primitivos:** `int`, `boolean`, `double`, etc.
    
- **Objetos personalizados:** instancias de clases definidas por el usuario.
    
- **Colecciones:** listas (`List`), conjuntos (`Set`), mapas (`Map`), etc.
    
- **Cualquier otro tipo de objeto** compatible con Java.
    

Estos datos serán accesibles desde la vista, y se pueden mostrar directamente al usuario final.

---

### 🔹 **Resumen general**

|Elemento|Descripción|
|---|---|
|**Objetivo**|Intercambiar información entre el controlador y la vista (UI).|
|**Clase utilizada**|`ModelAndView`|
|**Método principal**|`addObject(String key, Object value)`|
|**Acceso en JSP**|`request.getAttribute("key")`|
|**Ubicación del dato**|Objeto `HttpServletRequest`|
|**Tipos de datos permitidos**|Primitivos, objetos, colecciones o cualquier tipo Java.|

---

### ✅ **Conclusión**

Toda aplicación web se basa en **intercambiar información** entre el usuario y el backend.  
En **Spring MVC**, esto se realiza fácilmente mediante el objeto **`ModelAndView`**, que permite:

- Enviar datos desde el **controlador** hacia la **interfaz de usuario**.
    
- Acceder a dichos datos desde las vistas (por ejemplo, JSP) usando **`request.getAttribute()`**.
    

> 💡 En resumen:  
> Todo lo que se agrega a un `ModelAndView` en el controlador estará disponible como un **atributo en la solicitud HTTP**, accesible en la UI para ser mostrado o procesado.