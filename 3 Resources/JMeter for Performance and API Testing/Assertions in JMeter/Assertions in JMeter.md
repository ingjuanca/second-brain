
---

El documento se enfoca en las **Aserciones** de JMeter, que son puntos de verificación aplicados a la respuesta del servidor para validar que la prueba fue exitosa más allá de una simple conexión.

### 1. Concepto de Aserción

- Una aserción es un tipo de **punto de verificación**.
    
- Se aplica sobre la **respuesta** del servidor.
    
- El propósito es **validar un patrón** en la respuesta.
    

### 2. Tipos de Aserciones Más Utilizadas

Los tipos de aserciones más comunes en JMeter incluyen:

- **Response Assertion** (Aserción de Respuesta)
    
- **Duration Assertion** (Aserción de Duración)
    
- **Size Assertion** (Aserción de Tamaño)
    
- **HTML Assertion** (Aserción HTML)
    
- **XML Assertion** (Aserción XML)
    
- **XML Schema Assertion** (Aserción de Esquema XML)
    
- **XPath Assertion** (Aserción XPath)
    
- **JSON Assertion** (Aserción JSON)
    

### 3. Response Assertion (Aserción de Respuesta)

Se utiliza para validar un patrón en diversas partes de la respuesta.

| **Campo a Validar**                    | **Validación                                                                 | (Ejemplos)**                                                                          |
| -------------------------------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
|                                        | **Response Message** (Mensaje de Respuesta)                                  | Verificar si el mensaje contiene o no la palabra "OK".                                |
|                                        | **Response Code** (Código de Respuesta)                                      | Verificar si el código de respuesta es exactamente igual a `200` (éxito).             |
|                                        | **Response Headers** (Encabezados de Respuesta)                              | Verificar si los encabezados contienen una cadena específica (ej: `HTTP/1.1 200 OK`). |
| **Text Response** (Texto de Respuesta) | Verificar si un texto específico está presente en el cuerpo de la respuesta. |                                                                                       |

### 4. Duration Assertion (Aserción de Duración)

- **Propósito:** Validar que la solicitud del _Sampler_ se procese dentro de una **cantidad de tiempo específica**.
    
- **Métrica:** La duración se especifica en **milisegundos** (ms).
    
- **Fallo:** Si el tiempo de respuesta excede el valor especificado, la prueba falla.
    

### 5. Size Assertion (Aserción de Tamaño)

- **Propósito:** Validar que el **tamaño de la respuesta** coincide con un valor especificado en **bytes**.
    
- **Campos a Verificar:** Se puede validar el tamaño de la **respuesta completa**, solo los **encabezados**, solo el **cuerpo**, el **código** o el **mensaje** de respuesta.
    
- **Operadores:** Se utilizan operadores como "menor que" (`<`), "mayor que" (`>`), o "igual a" (`=`).
    

### 6. Aserciones de Formato

| **Aserción** | **Propósito**            | **Verificación**                                                             |                                                                                                          |
| ------------ | ------------------------ | ---------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
|              | **HTML Assertion**       | Verifica la **sintaxis HTML** de la respuesta.                               | Puede configurar umbrales de **errores** y **advertencias** aceptables (ej: 5 errores, 10 advertencias). |
|              | **XML Assertion**        | Valida que la respuesta tenga una **sintaxis XML válida**.                   | Falla si el contenido no está bien formado (ej: "la entidad no fue declarada").                          |
|              | **XML Schema Assertion** | Valida la respuesta contra un **Esquema XML** especificado (un archivo XSD). | Compara el esquema XML de la respuesta con un esquema XML de referencia proporcionado.                   |
|              | **XPath Assertion**      | Valida la respuesta usando una **Expresión XPath**.                          | Verifica si un **nodo o elemento** específico (ej: un logo) está presente en la respuesta.               |
|              | **JSON Assertion**       | Valida la respuesta cuando está en formato **JSON**.                         | Verifica si un **nombre de nodo JSON** específico está presente en la respuesta.                         |

### 7. Configuración de un Test Plan Básico (Ejemplo)

Para probar aserciones, se sigue una configuración estándar:

1. **Thread Group (Usuarios):** Se añade y se configuran los usuarios (_Threads_), el tiempo de aceleración (_Ramp-up_) y el contador de ciclos (_Loop Count_).
    
2. **Sampler (Solicitud):** Se añade un _Sampler_ (ej: **HTTP Request**) y se especifica el servidor (`opensource-demo.orangehrmlive.com`) y el protocolo.
    
3. **Listener (Reporte):** Se añaden _Listeners_ para ver los resultados. Es crucial el **Assertion Results** (Resultados de Aserción), que muestra los estados de las verificaciones.
    
4. **Assertion:** Se añaden las aserciones, generalmente haciendo clic derecho en el _Thread Group_ o _Test Plan_.
    
---
