
---

## 🧩 **Título: Uso de los atributos `required` y `defaultValue` en @RequestParam**

---

### 🔹 **Traducción completa al español**

En la lección anterior, ya aprendiste a **leer y mostrar parámetros de consulta (query parameters)** desde la URL usando `@RequestParam`.

En esta lección se explican **dos atributos adicionales** que se pueden utilizar dentro de la anotación `@RequestParam`:

- `required`
    
- `defaultValue`
    

---

### 🧱 **1. Escenario inicial: Falta de un parámetro obligatorio**

En el ejemplo anterior, el método del controlador recibía los parámetros `id`, `name` y `salary` desde la URL.

Si en el navegador se omite el parámetro `salary` y se envía una URL como:

`http://localhost:8080/springmvc/showData?id=123&name=John`

entonces el contenedor de Spring genera el siguiente error:

`Required double parameter 'salary' is not present`

y el navegador muestra un **HTTP 400 (Bad Request)**.

📘 **Motivo:**  
Por defecto, **Spring considera todos los parámetros marcados con `@RequestParam` como obligatorios**.

---

### 🟡 **2. Hacer opcional un parámetro con `required=false`**

Para evitar el error anterior y permitir que un parámetro sea opcional, se puede agregar el atributo `required` dentro de `@RequestParam`:

```java
@RequestParam(value = "sal", required = false) 
double salary
```

🔍 **Explicación:**

- `value` indica el nombre del parámetro en la URL.
    
- `required = false` le dice a Spring que el parámetro puede omitirse.
    
- Por defecto, `required = true`.
    

---

### ⚠️ **3. Nueva excepción: valores nulos en tipos primitivos**

Si intentas ejecutar el controlador sin el parámetro `salary`, notarás que aparece una nueva excepción diferente:

`Optional double parameter 'sal' is not present but cannot be translated into null`

📘 **Causa:**  
Aunque `salary` se declaró como opcional (`required = false`), **Spring intenta asignarle `null`**, pero los **tipos primitivos** (`int`, `double`, `boolean`, etc.) **no aceptan valores nulos**.

---

### 🟢 **4. Solución: Usar `defaultValue`**

Para resolverlo, Spring permite asignar un **valor por defecto** cuando el parámetro no se proporciona en la URL.  
Esto se logra con el atributo `defaultValue`.

```java
@RequestParam(value = "sal", required = false, defaultValue = "60") 
double salary
```

🔍 **Explicación:**

- Si el usuario no envía el parámetro `sal`, Spring asignará automáticamente el valor `"60"`.
    
- Este valor se convierte internamente al tipo correcto (`double` en este caso).
    
- También puedes usar cualquier otro valor, incluso `"0.0"` si deseas indicar salario no especificado.
    

---

### 🧪 **5. Verificación en el navegador**

Luego de agregar `defaultValue`, reinicia el servidor Tomcat y vuelve a acceder a la URL:

`http://localhost:8080/springmvc/showData?id=123&name=John`

Ahora:

- No se producirá ningún error.
    
- En la consola del servidor se verá:
    
    `Id: 123 Name: John Salary: 60.0`
    

El valor 60 corresponde al valor por defecto definido.

---

### 🔴 **6. Ejemplo de error por tipo incorrecto**

Si se envía un valor no numérico en un parámetro que espera un número, por ejemplo:

`http://localhost:8080/springmvc/showData?id=xyz&name=John&sal=70`

Spring intentará convertir `"xyz"` a un número entero y fallará, mostrando nuevamente un **error 400 (Bad Request)**.

---

## 🧠 **Resumen general**

| Atributo           | Función                                                     | Valor por defecto | Ejemplo                                        |
| ------------------ | ----------------------------------------------------------- | ----------------- | ---------------------------------------------- |
| **`required`**     | Indica si el parámetro es obligatorio.                      | `true`            | `@RequestParam(value="sal", required=false)`   |
| **`defaultValue`** | Asigna un valor predeterminado si el parámetro no se envía. | Ninguno           | `@RequestParam(value="sal", defaultValue="0")` |

---

### ⚙️ **Comportamiento por tipo de dato**

| Tipo de dato                        | ¿Acepta null? | ¿Requiere `defaultValue` si es opcional? |
| ----------------------------------- | ------------- | ---------------------------------------- |
| **Primitivo (int, double, etc.)**   | ❌ No          | ✅ Sí, para evitar errores de conversión  |
| **Wrapper (Integer, Double, etc.)** | ✅ Sí          | Opcional — puede ser `null` sin error    |

---

## ✅ **Conclusión final**

- Por defecto, los parámetros de `@RequestParam` son **obligatorios**.
    
- Si deseas que un parámetro sea opcional, usa **`required=false`**.
    
- Si el parámetro es de tipo **primitivo** y puede faltar, **debes usar `defaultValue`**, ya que los tipos primitivos **no aceptan `null`**.
    
- Spring convierte automáticamente los valores por defecto al tipo correspondiente.
    
- Si el usuario envía un valor incompatible (por ejemplo, letras en lugar de números), se producirá un **error 400**.
    

> 💡 En resumen:  
> Usa `required=false` para parámetros opcionales y **siempre combina `defaultValue` con tipos primitivos** para evitar errores de conversión.