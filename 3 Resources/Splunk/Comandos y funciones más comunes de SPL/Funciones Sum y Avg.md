
---

## Resumen: Funciones `sum`, `avg`, `min` y `max` en el Comando `stats`

### 1. Funciones de Estado y Aplicación

- Las funciones **`sum`** y **`avg`** son **funciones de estado**.
    
- Son muy útiles para trabajar con información que involucra **valores numéricos** como **ventas** o **beneficios**.
    
- Estas funciones, al igual que `min` y `max`, se utilizan dentro del comando **`stats`**.
    

### 2. Función `sum` (Suma Total)

- La función **`sum`** calcula la **suma total** de los valores de un campo numérico que se le indique.
    
- **Ejemplo:** `| stats sum("Total Profit")` devuelve el **beneficio total** de todas las ventas realizadas.

```Splunk
index="sales" sourcetype="sales.csv"
| stats sum("Total Profit")
```

- **Desglose por Campo (`by`):** Se puede utilizar la cláusula **`by`** para **agrupar la suma** por un campo categórico (ej. `by "Item Type"`), lo que permite determinar, por ejemplo, el objeto más rentable.

```Splunk
index="sales" sourcetype="sales.csv"
| stats sum("Total Profit") by "Item Type"
```

- **Combinación con `count`:** Es posible incluir la función `count` junto con `sum` para obtener una **visión más completa** de las ventas (ej. _unidades vendidas_ y _beneficio total_ por tipo de artículo).
    
```Splunk
index="sales" sourcetype="sales.csv"
| stats count("Units Sold") sum("Total Profit") by "Item Type"
```

### 3. Funciones `avg`, `min` y `max` (Promedio y Extremos)

- **Función `avg` (Average):** Se utiliza para calcular el **valor medio** de un campo.
    
    - **Ejemplo:** Calcular el precio medio por unidad de todos los productos en venta.

```Splunk
index="sales" sourcetype="sales.csv"
| stats avg("Unit Price") as "Precio medio por producto"
```

- **Funciones `min` y `max`:** Permiten calcular el **valor mínimo** (`min`) y el **valor máximo** (`max`) de un campo numérico.
    
    - **Ejemplo:** `min("unit price")` y `max("unit price")` mostrarían el precio del producto más barato y el más caro.

```Splunk
index="sales" sourcetype="sales.csv"
| stats avg("Unit Price") as "Precio medio por producto", min("Unit Price"), max("Unit Price")
```

- **Legibilidad (`AS`):** Al igual que con otras funciones de `stats`, se recomienda usar la cláusula **`AS`** (como) para cambiar el nombre de los campos resultantes, haciéndolos más legibles (ej. _precio medio_, _precio mínimo_).
    

---