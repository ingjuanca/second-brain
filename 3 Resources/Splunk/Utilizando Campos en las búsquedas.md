
---

## Resumen: Conceptos Clave sobre Fields (Campos) en Splunk

### 1. Definición y Propósito de los Fields

- Un **Field** (campo) es una estructura de nombre-valor.
    
- Los campos son cruciales para las búsquedas, ya que permiten **descubrir y extraer la información específica** que se necesita de los eventos guardados en Splunk.
    

### 2. Campos Fijos y Sugeridos

- Hay **tres campos fijos** que siempre aparecen y se buscan por defecto: host, source, y sourcetype**.
    
- **`sourcetype`** (tipo de fuente) es un campo muy útil para filtrar, ya que resume los posibles valores de la fuente de los _logs_ (ej. eventos de servidor Linux, _logs_ de servidor web, etc.).
    
- **Interesting Fields:** Son campos **sugeridos por Splunk** que aparecen en al menos el **20%** de los eventos indexados, haciéndolos bastante comunes.
    
- Se puede acceder a la lista **completa de campos** para ver el número de valores que pueden tomar y la cobertura (el porcentaje de logs en que aparece ese campo).
    

### 3. Interacción y Tipos de Datos

- **Añadir a la Búsqueda:** Al hacer clic en un valor de un campo (ej. `sourcetype=linux`), automáticamente se **añade ese campo y valor** a la consulta de búsqueda, refinando los resultados.
    
- **Tipos de Datos:** La interfaz de Splunk indica el tipo de dato del campo:
    
    - La **letra 'S'** indica que el campo es un **string** (cadena de texto).
        
    - El símbolo de **almohadilla ('#')** indica que el campo es un entero (número).
        

### 4. Uso de Operadores para Refinar la Búsqueda

Los campos permiten usar operadores de comparación para búsquedas más avanzadas:

| Operador                 | Aplicación                                                                  | Ejemplo de Uso |                                        |
| ------------------------ | --------------------------------------------------------------------------- | -------------- | -------------------------------------- |
| **`=`** (Igual)          | Búsqueda por valor exacto.                                                  |                | `action=purchased`                     |
| **`!=`** (Distinto de)   | Excluye un valor específico.                                                |                | `action!=purchased`                    |
| **`AND`**                | Conecta dos condiciones (es implícito si no se escribe nada).               |                | `sourcetype=web AND action!=purchased` |
| **`OR`**                 | Muestra eventos que cumplen una condición o la otra.                        |                |                                        |
| **`NOT`**                | Niega una condición (se usa antes de la condición).                         |                | `NOT action=purchased`                 |
| **`IN`**                 | Permite buscar **múltiples valores** para un solo campo de forma abreviada. |                | `status IN (200, 500, 503)`            |
| **`<`, `>`, `<=`, `>=`** | Útiles para campos de tipo entero (numérico).                               |                |                                        |

### 5. Consideraciones Clave (Case Sensitivity)

- **Nombre del Campo:** El nombre del _Field_ es **case sensitive** (distingue mayúsculas y minúsculas). Debe escribirse exactamente como aparece (ej. `action` en minúsculas)18.
    
- **Valor del Campo:** El valor asociado al nombre del _Field_ **no es case sensitive** (no distingue mayúsculas). Se puede escribir con mayúsculas o minúsculas (ej. `purchased` o Purchased).
    

### 6. Diferencia Crítica entre `!=` y `NOT`

- Es importante entender la diferencia entre `campo != valor` y NOT campo = valor:
    
    - **`action != purchased`** (Distinto de) solo devuelve eventos que **tienen el campo `action`** definido, pero con un valor diferente a "purchased".
        
    - **`NOT action=purchased`** (Negación) devuelve muchos más eventos, incluyendo **todos aquellos que ni siquiera tienen el campo `action`** definido dentro de su _log_.
        

---

¿Te gustaría que viéramos un ejemplo específico de cómo usar el operador `IN` o quizás ahondar más en el concepto de `sourcetype`?