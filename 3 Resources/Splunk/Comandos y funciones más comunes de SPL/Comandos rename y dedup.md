
---

## Resumen: Comandos `rename` y `dedup` en SPL de Splunk

### 1. Comando `rename`

El comando `rename` permite **cambiar el nombre de los campos** en los resultados de una búsqueda, lo que es útil para compartir información o adaptarla a un idioma o nomenclatura específicos.

- **Sintaxis:** Se usa la cláusula **`AS`** (como) para indicar el nuevo nombre.
    
    - `| rename nombre_campo_original AS nuevo_nombre_campo`.
        
    - _Ejemplo:_ Renombrar un campo de `productId` a `código de producto`.

```Splunk
index="main" source="access_30days.log"
| table productId
| rename productId AS "Código de Producto"
```

- **Encadenamiento de Comandos (Pipes):** Es crucial recordar que, una vez que se ejecuta `rename`, el nombre original del campo **deja de existir** para los comandos subsiguientes.
    
    - Si un comando posterior (como `fields` o `table`) intenta referenciar el nombre original (`Productivity`), **fallará**.

```Splunk
index="main" source="access_30days.log"
| table productId action
| rename productId AS "Código de Producto" action AS "Acción"
| fields -productId
```

    - Para trabajar con el campo después de renombrarlo, se debe usar **el nuevo nombre** (`código de producto`).
        

```Splunk
index="main" source="access_30days.log"
| table productId action
| rename productId AS "Código de Producto" action AS "Acción"
| fields - "Código de Producto"
```
### 2. Comando `dedup`

El comando `dedup` se utiliza para **eliminar eventos duplicados** según los valores de uno o más campos especificados. Su objetivo es quedarse solo con los **valores únicos**.

- **Propósito:** Es muy útil para tareas de **limpieza de datos** o cuando se necesita saber cuáles son los **valores posibles/únicos** para una acción o entidad.
    
    - _Ejemplo:_ Si un mismo producto tiene la acción "add to cart" repetida muchas veces por diferentes usuarios, `dedup` permite quedarse solo con una instancia de esa acción para el producto, revelando las acciones **únicas** realizadas.
        
- **Sintaxis:** Se indica el comando seguido de los campos sobre los que se desean quitar los duplicados.
    
    - `| dedup campo1, campo2`.
        

```Splunk
index="main" source="access_30days.log"
| table productId action
| dedup productId, action
```
---