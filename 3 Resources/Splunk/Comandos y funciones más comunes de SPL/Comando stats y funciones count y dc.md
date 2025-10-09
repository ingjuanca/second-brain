
---

## Resumen: Comando `stats`, `count` y `dc` en Splunk SPL

### 1. Comandos de Transformación (Transforming Commands)

- Los **comandos de transformación** permiten ordenar los resultados de búsqueda en un **formato de tabla** con fines **estadísticos**.
    
- El objetivo final es poder **transformar esos resultados en visualizaciones** (gráficos, etc.).
    
- El comando **`stats`** es un ejemplo de comando de transformación, al igual que `top` y `rare`.
    

### 2. El Comando `stats` y sus Funciones

- El comando **`stats`** se utiliza para **obtener estadísticas** sobre la información que se está analizando.
    
- Se apoya en una serie de **funciones** como `count`, `dc`, `sum`, `avg`, `list`, y `value`.
    

### 3. La Función `count`

La función `count` se utiliza para **contar el número de eventos** que cumplen un criterio.

- **Conteo por Campo (`by`):** Permite contar la ocurrencia de eventos **agrupados por el valor de un campo**.
    
    - _Ejemplo:_ `| stats count by region` cuenta el número de ventas en cada región.

```Splunk
index="sales" sourcetype="sales.csv"
| stats count by region
```

- **Conteo de un Campo Específico:** Se puede usar `count` para determinar **cuántas peticiones tuvieron un campo específico configurado o existente** (ej. contar cuántas peticiones tienen el campo `action`).

```Splunk
index="main" sourcetype="access_30days.log"
| stats count(action) AS "ActionEvents" 
```

- **Conteo Total de Eventos:** Si se usa `| stats count AS "TotalEvents"` sin especificar un campo `by`, el resultado será el **número total de eventos** en el rango de búsqueda.

```Splunk
index="main" sourcetype="access_30days.log"
| stats count(action) as "ActionEvents", count as "Total Events"
```

- **Renombrar Resultados (`AS`):** Se utiliza la cláusula **`AS`** (similar al comando `rename`) para darle un nombre descriptivo a la columna de conteo (ej. `AS "ventas totales por región"`).
    
```Splunk
index="sales" sourcetype="sales.csv"
| stats count as "ventas totales por región" by region
```


### 4. La Función `dc` (Distinct Count)

La función **`dc`** (Distinct Count) se utiliza para obtener el **número de eventos únicos** que coinciden con los criterios de búsqueda.

- **Propósito:** Muestra el **número de valores únicos** de un campo entre todos los eventos.
    
    - _Ejemplo:_ `| stats dc(Country)` indica el número único de países a los que se les ha vendido, sin duplicar.

```Splunk
index="sales" sourcetype="sales.csv"
| stats dc(country) as "Numero de paises"
```

- **Agrupación (`BY`):** Al igual que `count`, se puede usar la cláusula **`by`** para agrupar los conteos distintos según otro campo.
    
    - _Ejemplo:_ `| stats dc(country) by "Order Priority"` muestra la cantidad de países únicos a los que se les ha vendido para cada nivel de prioridad de orden.
        
```Splunk
index="sales" sourcetype="sales.csv"
| stats dc(country) as "Numero de paises" by "Order Priority"
```

---