
---

## Resumen: Consejos para la Optimización de Búsquedas en Splunk

La optimización de búsquedas es crucial, especialmente en entornos reales de producción con

**multitud de índices y terabytes** de información, donde estos consejos marcan la diferencia en los **tiempos de búsqueda**.

### 1. Limitar el Rango de Tiempo (Time Range)

- **Nunca** hagas búsquedas con el rango de tiempo **"All Time"** (_desde siempre_); esto consume muchísimos recursos y tarda demasiado en dar resultados.
    
- **Limita el rango** de tiempo al máximo posible (ej. **7 días**, **24 horas**), eligiendo un rango que se ajuste a la información que buscas.
    

### 2. Uso Prioritario de Campos Fijos

- Para concretar la búsqueda, utiliza los **cuatro campos clave** que actúan como filtros primarios:
    
    - **`index`**: El índice específico donde se encuentran los datos (ej. `index=main`).
        
    - **`sourcetype`**: El tipo de fuente de los _logs_ (ej. `sourcetype=access_combined_wcookie` para eventos de servidor web).
        
    - **`source`**: El fichero del que viene la información.
        
    - **`host`**: Dónde se han generado esos eventos.
        
- **Sé lo más específico posible**. Usar estos campos ayuda a **reducir drásticamente la cantidad de eventos** a procesar (por ejemplo, bajando de 42,000 a 23,000 eventos al especificar el `sourcetype`).
    

### 3. Preferir Búsquedas Inclusivas y Concretas

- **Mejor `campo=valor` que solo palabras clave:** Si puedes buscar por un campo específico, hazlo. Por ejemplo, en lugar de solo `failed`, si conoces el campo, usa la combinación de palabras clave dentro de ese campo (ej. `password failed`).
    
- **Siempre preferir el operador `AND` (inclusión) en lugar de `NOT` (exclusión)**.
    
    - La recomendación es usar **búsquedas inclusivas** (ej. `method=GET`) en lugar de hacer la misma búsqueda por descarte (ej. `NOT method=POST`).
        
    - Usar **`NOT`** es menos eficiente y se recomienda evitarlo siempre que sea posible para el valor de un campo.
        

### 4. Personalización Avanzada del Rango de Tiempo

- Además de los _presets_ (preconfigurados), puedes usar la opción **"Custom"** para definir rangos de fechas específicos.
    
- Para un control avanzado, puedes modificar directamente los parámetros
    
    **`earliest`** y **`latest`** en la búsqueda:
    
    - **`earliest`**: El evento más temprano (hace, por ejemplo, 7 días).
        
    - **`latest`**: El evento más tardío (ahora mismo)17.
        
    - Se utiliza una
        
        **nomenclatura** específica para el tiempo (ej. **`d`** para días, **`m`** para minutos, **`s`** para segundos, **`mon`** para meses)18.
        

### 5. Monitoreo en Tiempo Real (_Real-Time_)

- La opción
    
    **"Real-Time"** se utiliza si tienes una fuente de eventos que está cambiando constantemente19.
    
- Al aplicar un rango (ej. eventos producidos
    
    **hace 7 días**) y activar el **"Real-Time"**, Splunk va **refrescando y añadiendo nuevos eventos** automáticamente a los resultados de la búsqueda20202020.
    

---

Estas son las buenas prácticas fundamentales para optimizar tus consultas. ¿Te gustaría practicar cómo aplicar estos filtros de `index` y `sourcetype` en una búsqueda de ejemplo?