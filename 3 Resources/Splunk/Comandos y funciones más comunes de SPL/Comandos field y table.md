
---

## Resumen: Comandos `fields` y `table` para Gestión de Visualización en Splunk

### 1. El Comando `fields`

El comando `fields` se utiliza para **seleccionar qué campos (columnas) se mostrarán** en los resultados de la búsqueda, permitiendo al usuario centrarse solo en la información relevante.

- **Inclusión (Selección):** Se utiliza para seleccionar explícitamente los campos que se quieren ver. Por ejemplo, al buscar solo **`| fields Ticker, Average`**, Splunk elimina el resto de los campos y solo muestra esos dos.

```Splunk
index="stocks"
| fields Ticker Average
```

- **Exclusión (Eliminación):** Se utiliza al anteponer un signo de **menos (`-`)** al nombre del campo (ej., **`| fields -Ticker, -Average`**). Esto quita los campos listados de los resultados y muestra todos los demás.

```Splunk
index="stocks"
| fields -Ticker -Average
```

#### 🔑 **Mejor Práctica de Optimización:**

- **Siempre es mejor usar la inclusión** (seleccionar campos) que la exclusión (quitar campos) para optimizar las búsquedas.
    
- La **inclusión** es más eficiente porque Splunk sabe de antemano qué debe buscar.
    
- La **exclusión** es menos eficiente porque Splunk primero recorre todo el índice para obtener todos los resultados y _luego_ quita los campos especificados.
    
#### ⚠️ **Campos Internos (`_`):**

- Existen **campos internos** que Splunk utiliza para su propio funcionamiento y que inician con un guion bajo (`_`).
    
- Ejemplos de estos campos son `_time` (el tiempo que Splunk agrega al evento) y **`_raw`** (la información del evento en crudo).
    
- Estos campos internos también se pueden **excluir** con el comando `fields` (ej., **`| fields -_raw`**) para limpiar la presentación de los resultados.
    

```Splunk
index="stocks"
| fields - _raw
```

### 2. El Comando `table`

El comando `table` es muy similar a `fields`, ya que también selecciona un conjunto de campos para mostrar, pero su principal diferencia es la **manera en que formatea los resultados**.

- **Formato de Tabla:** `table` obliga a que los resultados se muestren en un formato de tabla. Los nombres de los campos que se buscan se convierten en las **cabeceras** de esa tabla.
    
- **Orden de Campos:** El orden en el que se listan los campos en el comando `table` **determina el orden de las columnas** en la tabla resultante. Si se quiere cambiar la posición de una columna, basta con modificar el orden de los campos en la búsqueda.
    

### 3. Contexto de Datos (Ejemplo)

El documento utiliza un índice de ejemplo llamado **`docs`** que contiene información sobre **predicciones de analistas sobre valores de la bolsa** (nombre de la acción, valor actual, predicciones de máximos/mínimos, etc.).
