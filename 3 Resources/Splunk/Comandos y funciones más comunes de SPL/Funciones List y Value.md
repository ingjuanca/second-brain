
---

## Resumen: Funciones `list` y `values` en el Comando `stats`

### 1. El Propósito del Listado de Valores

Tanto **`list`** como **`values`** son funciones del comando `stats` que se utilizan para generar un listado de los posibles valores que toma un campo, típicamente agrupados por otro campo.

### 2. Función `list` (Listado Completo)

- **¿Qué hace?** La función `list` genera un listado de **todos los valores** que ha tomado un campo en un conjunto de eventos, **incluyendo duplicados**.
    
- **Uso (Sintaxis):** Se utiliza con el comando `stats` y se agrupa usando la cláusula `by`.
    
    - _Ejemplo:_ Si queremos ver qué tipos de objetos se han vendido por país, se usa: `| stats list("Item Type") by country`.

```Splunk
index=sales
| stats list("Item Type") by country
```

- **Utilidad:** Muestra el listado completo, tal como aparece en los eventos, lo cual puede ser útil en algunos casos.
    

---

### 3. Función `values` (Listado Único)

- **¿Qué hace?** La función `values` es similar a `list` pero suprime los duplicados. Genera un listado de los **valores únicos** (sin repeticiones) de un campo.
    
- **Uso:** Es ideal cuando se necesita conocer la **variedad** de valores posibles que un campo ha tomado, sin contar las repeticiones.
    
    - _Ejemplo:_ Si usamos `| stats values("Item Type") by "Country"`, el listado de objetos vendidos en cada país mostrará cada tipo de objeto **solo una vez**.

```Splunk
index=sales
| stats values("Item Type") by "country"
```

- **Utilidad Práctica:** Se utiliza comúnmente para estudios de mercado o para extraer información concisa sobre qué productos o categorías interesan más en cada grupo (país, región, etc.).
    

---

### 4. Buenas Prácticas Compartidas

- **Renombrar Campos:** Para que el informe sea más legible, se recomienda utilizar la cláusula `AS` para cambiar el nombre del campo resultante (ej. "tipos de objetos vendidos").
    
- **Contexto:** Ambas funciones se aplican a conjuntos de datos como índices de ventas, pero pueden aplicarse a cualquier _set_ de datos.
    

---