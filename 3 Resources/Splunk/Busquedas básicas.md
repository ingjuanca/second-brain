
---

## Resumen de Ideas Clave sobre Búsquedas y la Interfaz de Splunk

### La Interfaz de Splunk

- **Barra Superior:** Contiene opciones fijas relacionadas con la **administración** de la plataforma, los **settings**, los _jobs_, y las **alertas**3.
    
- **Aplicaciones (Apps):** En la parte superior izquierda se indica la aplicación seleccionada4. Las aplicaciones son **extensiones** que permiten una visualización de datos de una manera concreta, a menudo con **Dashboards** para facilitar su comprensión5. Es posible **gestionar o encontrar más aplicaciones** desde allí.
    
    - _Ejemplo:_ Una aplicación de **Tenable** (escáner de vulnerabilidades) permite una visualización sencilla e intuitiva de los datos de vulnerabilidades.
        
- La clase se centra en la aplicación de **Búsqueda y _Reporting_**.
### Realizando Búsquedas

- **Barra de Búsqueda:** Es donde se ingresa el término de búsqueda.
    
    - Splunk proporciona una ayuda **sugiriendo términos** mientras se escribe.
        
- **Rango de Tiempo:** Hay un desplegable para seleccionar el rango de tiempo de la búsqueda. Lo recomendado es **restringir este rango** (ejemplo: "últimos siete días") porque usar "all time" (_desde el principio de los tiempos_) es muy ineficiente y consume muchos recursos.
    
### Opciones y Resultados de la Búsqueda

- **Resultados y Rango:** Al hacer la búsqueda, Splunk muestra el **número de resultados** encontrados y el **rango de tiempo** en el que se realizó la búsqueda.
    
- **Sampling (Muestreo):** Permite reducir el número de eventos mostrados para búsquedas muy grandes.
    
- **_Jobs_** **de Búsqueda:** Cada búsqueda genera un _job_.
    
    - Por defecto, los resultados de un _job_ duran **10 minutos** si no se comparten.
        
    - Se puede hacer **pausa** o **detener (_stop_)** un _job.
        
- **Compartir Búsquedas:** Es muy útil para el trabajo en equipo18. Al compartir, se genera una URL para que un compañero pueda acceder a los resultados durante **7 días** sin tener que volver a ejecutar la búsqueda.
    
- **Descargar y Guardar:** Es posible **imprimir** o **descargar** la información en formatos como CSV e ISO NTC20. Las búsquedas se pueden **salvar** como alertas, paneles de _Dashboard_, o _reports_.
    
- **Modos de Búsqueda:** El modo por defecto es **Smart**.
    
    - **Fast (_Rápido_):** No intenta descubrir los campos (_fields_) y busca específicamente lo que se pide.
        
    - **Verbose (_Detallado_):** Muestra todos los eventos y realiza un descubrimiento de **todos los campos**.
        
    - **Smart (_Inteligente_):** Ajusta la búsqueda para que sea la más optimizada según lo que se está tratando de encontrar.
        

### Visualización de Eventos e Interacción

- **Histograma de Eventos:** Muestra cómo evolucionan los eventos a lo largo del tiempo, por ejemplo, a nivel de hora.
    
    - Hacer clic en un rango específico del histograma permite hacer un **zoom in** y ajustar la búsqueda a ese periodo **sin necesidad de volver a ejecutarla**.
        
    - Hacer un **zoom out** (volver al rango original) **sí requiere** volver a ejecutar la consulta.
        
- **Pestañas Adicionales:** Existen pestañas para identificar **patrones**, ver **estadísticas** (para _pivots_), y **visualización** (gráficos).
    
- **Campos (_Fields_):** Al hacer clic en el desplegable de los eventos, se muestran los campos seleccionados por defecto (ej. _host_, _source_).
    
    - Al hacer clic en un campo (ej. `SSH`) se puede **añadir ese campo a la búsqueda**.
        

### Uso de SPL (Search Processing Language)

- Splunk permite usar **operaciones binarias** en las búsquedas.
    
- **Operador AND:** Si no se escribe ningún operador entre los términos, se asume que es un **AND**.
    
    - _Ejemplo:_ `fail SSH` (eventos que contienen `fail` **Y** `SSH`).
        
- **Operadores OR y NOT:** Se pueden usar explícitamente.
    
    - _Ejemplo OR:_ `fail OR SSH` (eventos que contienen `fail` **O** `SSH`).
        
    - _Ejemplo NOT:_ `fail NOT password` (eventos con `fail` pero **NO** con `password`).
        
- Se pueden usar **paréntesis** para agrupar y organizar las búsquedas complejas.
    

