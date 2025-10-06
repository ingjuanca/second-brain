
---

## Resumen: Componentes y Estructura del Lenguaje de Búsqueda (SPL) de Splunk

### 1. Los Cinco Componentes Fundamentales de SPL

El Lenguaje de Procesamiento de Búsqueda (**SPL**) de Splunk se compone de cinco elementos clave:

| Componente               | Función                                                                                                 | Ejemplo (según el documento)                        |
| ------------------------ | ------------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| **Términos de Búsqueda** | Palabras clave, expresiones booleanas (`AND`, `OR`), y pares clave-valor (`campo=valor`).               | `index=main sourcetype=web`                         |
| **Comandos**             | Indican lo que se debe hacer con los resultados de la búsqueda.                                         | `stats` (para obtener estadísticas)                 |
| **Funciones**            | Explican a SPL cómo mostrar, procesar o evaluar los resultados.                                         | `sum()` (para sumar valores)                        |
| **Argumentos**           | Valores que una función necesita para operar (siempre que se usa una función, se necesitan argumentos). | El campo `precio` dentro de la función `sum()`      |
| **Cláusulas**            | Indican cómo agrupar o renombrar los resultados.                                                        | `AS` (para renombrar el resultado de una operación) |

### 2. Estructura y Encadenamiento de Comandos

- **Encadenamiento (Pipes):** Para realizar operaciones complejas, los comandos se separan con barras verticales (`|`), conocidas como **pipes**.
    
- **Flujo de Datos:** El resultado (eventos) del comando anterior se convierte en la **entrada** (_input_) para el siguiente comando. Esto crea un **efecto de embudo**, reduciendo progresivamente la información para llegar al resultado final.
    

### 3. Ejemplo Práctico Desglosado

El documento ilustra el encadenamiento de comandos para calcular el total de _bytes_ de compras que resultaron en error (códigos 500 y 503):

1. **Primer Comando (Filtro):** Se usa para filtrar los eventos: `index=main sourcetype=web action=purchase status IN (500, 503)`. Esto usa  **términos de búsqueda** y **operaciones binarias (`IN`)** para obtener un subconjunto de 124 eventos.
    
2. **Segundo Comando (Procesamiento):** Se aplica la operación `| stats sum(bytes) AS bytes_totales`. Aquí se usa el **comando `stats`**, la **función `sum()`** y la **cláusula `AS`** para calcular el total de _bytes_.
    
3. **Tercer Comando (Formato):** Se usa una operación para formatear el número entero resultante. Transforma el número en un  _string_, añade **separadores de coma** cada tres dígitos y **concatena** la palabra "bytes" para un formato más legible.
    

`index=main sourcetype=access_30day.log action=purchase AND status IN (500, 503) | stats sum(bytes) AS bytes_totales | fieldformat bytes_totales = tostring(bytes_totales, "commas") + "B"`
### 4. Configuración del Editor SPL

Splunk ayuda visualmente con el **resaltado de sintaxis**:

- **Comandos** se muestran en azul.
    
- **Funciones** se muestran en púrpura o rosa.
    
- **Cláusulas** se muestran en naranja.
    

Desde las **Preferencias** del usuario (en el editor SPL), se pueden ajustar varias opciones:

- **Nivel de Asistencia:** Controlar la ayuda que ofrece la barra de búsqueda.
    
- **Autoformato:** Colocar automáticamente **cada comando encadenado en una línea diferente** (separada por _pipes_), lo cual facilita la lectura de búsquedas largas.
    
- **Temas:** Cambiar el tema del editor, incluso a un **tema oscuro**.
    

---