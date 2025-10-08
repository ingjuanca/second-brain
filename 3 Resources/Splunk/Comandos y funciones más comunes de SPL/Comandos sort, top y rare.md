
---

## Resumen: Comandos `sort`, `top` y `rare` en SPL de Splunk

### 1. Comando `sort` (Ordenación)

El comando `sort` se utiliza para **ordenar los resultados de la búsqueda** basándose en los valores de uno o más campos.

- **Orden Predeterminado:** Por defecto, `sort` ordena el primer campo especificado de manera **ascendente** (valores más bajos primero).

```Splunk
index="main" source="access_30days.log"
| table clientip productId
| sort clientip product
```

- **Orden Descendente/Inverso:** Para ordenar de forma **inversa** (descendente), se debe anteponer un **guion (`-`)** delante del nombre del campo.
    
- **Ordenar Múltiples Campos:**
    
    - Si se usa el guion **sin espacio** antes de un campo (`-campo1 campo2`), solo se aplica el orden inverso al primer campo. El segundo campo permanece desordenado o en el orden predeterminado.

```Splunk
index="main" source="access_30days.log"
| table clientip productId
| sort -clientip product
```

	- Si se usa el guion **con un espacio** (`- campo1 campo2`), el orden inverso se aplica a **ambos campos**.

```Splunk
index="main" source="access_30days.log"
| table clientip productId
| sort - clientip product
```

- **Argumento `limit`:** Permite **acotar el número de resultados** que se muestran después de la ordenación.
    
    - _Ejemplo:_ `| sort limit=10 clientId` limitaría la salida a los 10 primeros resultados ordenados.
        

```Splunk
index="main" source="access_30days.log"
| table clientip productId
| sort clientip product limit=10
```

### 2. Comandos `top` y `rare` (Análisis de Frecuencia)

Los comandos `top` y `rare` son **opuestos** entre sí, ya que uno busca los valores más comunes y el otro, los menos comunes.

- **Comando `top`:** Muestra los **valores más repetidos** o comunes para un campo especificado.
    
    - _Ejemplo:_ `| top clientId` muestra las direcciones IP que más veces han accedido.

```Splunk
index="main" source="access_30days.log"
| table clientip productId
| top clientip
```

- **Comando `rare`:** Muestra los **valores menos repetidos** o raros para un campo especificado.
    
    - _Ejemplo:_ Muestra las direcciones IP que menos veces han accedido.
        

```Splunk
index="main" source="access_30days.log"
| table clientip productId
| rare clientip
```
### 3. Argumentos Comunes en `top` y `rare`

Ambos comandos comparten una serie de argumentos para personalizar los resultados.

- **`limit`:** Similar a `sort`, permite recortar los resultados, por ejemplo, mostrando solo el top 5 o las 5 IP más raras.

```Splunk
index="main" source="access_30days.log"
| table clientip productId
| top clientip limit 5
```

    
- **Campos de Conteo Automático:** Por defecto, `top` y `rare` incluyen automáticamente dos campos en los resultados:
    
    - **`count` (`countfield`):** El número de veces que aparece el valor.
        
    - **`percent` (`percentfield`):** El porcentaje de los eventos totales que representa ese valor.
        
- **Personalización de Campos:**
    
    - Se pueden **renombrar** los campos automáticos usando `countfield` y `percentfield`.

```Splunk
index="main" source="access_30days.log"
| table clientip productId
| top clientip countfield=cantidad percentfield=porcentaje
```

- Se puede **ocultar** la visualización de estos campos usando `showcount=false` y `showperc=false`.

```Splunk
index="main" source="access_30days.log"
| table clientip productId
| top clientip showcount=false
```

```Splunk
index="main" source="access_30days.log"
| table clientip productId
| top clientip showperc=false
```

- **`useother`:** Es útil para saber la magnitud de la información que se está "dejando fuera" al aplicar un `limit`.
    
    - Si se usa `useother=true` junto con un límite (ej. top 5), se añade una fila que resume la **cantidad total de apariciones** y el **porcentaje** que representa el resto de los valores no incluidos en el top.

```Splunk
index="main" source="access_30days.log"
| table clientip productId
| top clientip limit 5 useother=true
```

```Splunk
index="main" source="access_30days.log"
| table clientip productId
| rare clientip limit 5 useother=true
```