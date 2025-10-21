
---

## Resumen Completo de Pruebas de Rendimiento

Las **Pruebas de Rendimiento** (o _perf testing_) son un tipo de prueba de software **no funcional** que garantiza que las aplicaciones de software funcionarán bien bajo una carga de trabajo esperada. Su objetivo principal no es encontrar _bugs_, sino **eliminar cuellos de botella de rendimiento**.

El enfoque principal es verificar la **velocidad**, la **escalabilidad** y la **estabilidad** del programa de _software_.

- **Velocidad:** Determina si la aplicación responde rápidamente.
    
- **Escalabilidad:** Determina la carga máxima de usuarios que puede manejar la aplicación.
    
- **Estabilidad:** Determina si la aplicación es estable bajo cargas variables.
    

Se realizan pruebas de rendimiento para proporcionar a los interesados información sobre la aplicación con respecto a su velocidad, estabilidad y escalabilidad, y para descubrir qué necesita mejorar antes de que el producto salga al mercado. Una aplicación lanzada con un rendimiento deficiente puede sufrir una mala reputación y no alcanzar los objetivos de ventas.

---

## Tipos de Pruebas de Rendimiento

Existen varios tipos principales de pruebas de rendimiento:

| **Tipo de Prueba**                               | **Objetivo**                                                                                                    | **Descripción**                                                                                                                    |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **Pruebas de Carga** (_Load Testing_)            | Identificar cuellos de botella antes de la puesta en marcha.                                                    | Verifica la capacidad de la aplicación para funcionar bajo una **carga de usuario anticipada**.                                    |
| **Pruebas de Estrés** (_Stress Testing_)         | Identificar el punto de ruptura de una aplicación.                                                              | Implica probar la aplicación bajo **cargas de trabajo extremas** para ver cómo maneja el tráfico alto o el procesamiento de datos. |
| **Pruebas de Resistencia** (_Endurance Testing_) | Asegurar que el _software_ maneje la carga esperada durante un **largo período de tiempo**.                     | Hecho para buscar problemas que solo se manifiestan con el tiempo (por ejemplo, fugas de memoria).                                 |
| **Pruebas de Picos** (_Spike Testing_)           | Probar la reacción del _software_ a **incrementos repentinos y grandes** en la carga generada por los usuarios. | Evalúa la capacidad de recuperación del sistema después de un pico.                                                                |
| **Pruebas de Volumen** (_Volume Testing_)        | Comprobar el rendimiento de la aplicación de _software_ bajo **volúmenes de bases de datos variables**.         | Se inserta una gran cantidad de datos en la base de datos y se monitorea el comportamiento general del sistema.                    |

---

## Problemas de Rendimiento Comunes

La mayoría de los problemas de rendimiento giran en torno a la velocidad, el tiempo de respuesta, el tiempo de carga y la escalabilidad deficiente.

|**Problema Común**|**Descripción**|
|---|---|
|**Tiempo de Carga Prolongado** (_Long Load Time_)|El tiempo inicial que tarda la aplicación en iniciarse. Debe mantenerse al mínimo, idealmente por debajo de unos pocos segundos.|
|**Tiempo de Respuesta Deficiente** (_Poor Response Time_)|El tiempo que transcurre desde que un usuario introduce datos hasta que la aplicación emite una respuesta. Si es demasiado largo, el usuario pierde interés.|
|**Escalabilidad Deficiente** (_Poor Scalability_)|El producto de _software_ no puede manejar el número esperado de usuarios o un rango suficientemente amplio de usuarios.|
|**Cuellos de Botella** (_Bottlenecking_)|Abstracciones en un sistema que degradan el rendimiento general. Puede ser causado por errores de codificación o problemas de _hardware_ que provocan una disminución del rendimiento bajo ciertas cargas.|

### Cuellos de Botella Comunes

Los cuellos de botella comunes que se encuentran en el rendimiento incluyen:

- Utilización de la **CPU**
    
- Utilización de la **Memoria**
    
- Utilización de la **Red**
    
- Limitaciones del **Sistema Operativo**
    
- Uso del **Disco**
    

---

## Proceso de Pruebas de Rendimiento

El proceso genérico para realizar pruebas de rendimiento incluye los siguientes pasos:

1. **Identificar el Entorno de Prueba:** Conocer el entorno físico, de producción y las herramientas disponibles. Entender la configuración del _hardware_, _software_ y red.
    
2. **Planificar y Diseñar la Prueba de Rendimiento:** Determinar cómo variará el uso entre los usuarios, identificar escenarios clave, simular una variedad de usuarios, planificar los datos de prueba y delinear las métricas a recopilar.
    
3. **Identificar Criterios de Aceptación de Rendimiento:** Establecer objetivos y restricciones para el rendimiento, los tiempos de respuesta y la asignación de recursos. Esto también puede implicar comparar con una aplicación similar.
    
4. **Configurar el Entorno de Prueba:** Preparar el entorno antes de la ejecución, incluyendo herramientas y otros recursos.
    
5. **Implementar el Diseño de la Prueba:** Crear las pruebas de rendimiento según el diseño de la prueba, que se basa en los requisitos de rendimiento.
    
6. **Ejecutar la Prueba:** Ejecutar los casos de prueba de rendimiento y monitorear la prueba.
    
7. **Analizar e Informar:** Analizar y compartir los resultados de las pruebas.
    
8. **Ajustar y Volver a Probar (_Fine Tune and Test Again_):** Volver a probar para ver si hay mejoras o disminuciones en el rendimiento. El proceso se detiene cuando el cuello de botella es causado por la CPU, lo que puede requerir aumentar la potencia de la CPU.
    

---

## Métricas de Pruebas de Rendimiento

Estas son los parámetros comunes a monitorear después de realizar las pruebas:

|**Métrica**|**Descripción**|
|---|---|
|**Uso del Procesador** (_Processor Usage_)|Tiempo que el procesador dedica a ejecutar hilos no inactivos.|
|**Uso de Memoria** (_Memory Use_)|Cantidad de memoria física disponible para los procesos.|
|**Tiempo de Disco** (_Disk Time_)|Tiempo que el disco está ocupado ejecutando una solicitud de lectura o escritura.|
|**Ancho de Banda** (_Bandwidth_)|Bits por segundo utilizados por una interfaz de red.|
|**Bytes Privados** (_Private Bytes_)|Bytes asignados por un proceso que no se pueden compartir con otros. Se utiliza para medir pérdidas y usos de memoria.|
|**Memoria Comprometida** (_Committed Memory_)|Cantidad de memoria virtual utilizada.|
|**Tiempo de Respuesta** (_Response Time_)|Tiempo desde que un usuario ingresa una solicitud hasta que se recibe el primer carácter de la respuesta.|
|**Rendimiento** (_Throughput_)|Tasa a la que una computadora o red recibe solicitudes por segundo.|
|**Agrupación de Conexiones** (_Connection Pooling_)|Número de solicitudes de usuario satisfechas por conexiones agrupadas.|
|**Máximo de Sesiones Activas** (_Maximum Active Sessions_)|El número máximo de sesiones que pueden estar activas a la vez.|
|**Ratios de Aciertos** (_Hit Ratios_)|Número de sentencias SQL manejadas por datos en caché en lugar de operaciones costosas.|
|**Aciertos por Segundo** (_Hits per Second_)|Número de aciertos en el servidor web durante cada segundo de una prueba de carga.|
|**Conteo de Hilos** (_Thread Count_)|Medición de la salud de una aplicación por el número de hilos en ejecución y activos.|

---

## Ejemplos de Casos de Prueba y Herramientas

### Ejemplos de Casos de Prueba

- Verificar que el tiempo de respuesta **no sea superior a cuatro segundos** cuando 1000 usuarios acceden simultáneamente (Pruebas de Carga).
    
- Verificar el tiempo de respuesta en un rango aceptable cuando la **conectividad de red es lenta**.
    
- Verificar el **número máximo de usuarios** que la aplicación puede manejar antes de fallar (Pruebas de Estrés).
    
- Verificar el tiempo de ejecución de la base de datos cuando se leen/escriben 500 registros simultáneamente.
    
- Verificar el uso de CPU y memoria del servidor bajo **condiciones de carga máxima** (Pruebas de Carga).
    
- Verificar el tiempo de respuesta bajo carga baja, normal, moderada y pesada (Pruebas de Picos).
    

### Herramientas de Pruebas de Rendimiento

Las herramientas populares disponibles en el mercado incluyen:

- **JMeter** (muy popular)
    
- **Micro Focus LoadRunner** (muy popular)
    
- **NeoLoad**
    
- **LoadNinja**
    

### Conclusión

Las pruebas de rendimiento son necesarias en la ingeniería de _software_ antes de comercializar cualquier producto, ya que **garantizan la satisfacción del cliente** y protegen la inversión contra el fallo del producto. Los costos de las pruebas de rendimiento generalmente se compensan con una **mayor satisfacción, lealtad y retención del cliente**.