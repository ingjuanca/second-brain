
---

Hola a todos, muchas gracias por su interés en este curso.

Este curso está dirigido especialmente a estudiantes que ya están familiarizados con Java y quieren aprender más sobre _virtual threads_ (hilos virtuales).

### Por qué aprender _virtual threads_

Si eres desarrollador Java, sabes que cada instrucción que escribimos se ejecuta en un hilo.  
Un hilo es la unidad de planificación o de concurrencia.  
Tradicionalmente los desarrolladores usamos múltiples hilos para manejar solicitudes concurrentes.

Por ejemplo, en una aplicación web con Spring Boot y Tomcat, este viene por defecto con 200 hilos para atender hasta 200 solicitudes concurrentes.  
Cada solicitud se asigna a un hilo para ser procesada.  
Si cada solicitud tarda 1 segundo, la aplicación puede procesar 200 solicitudes por segundo: este es el **throughput** (rendimiento) de la aplicación.  
Si queremos aumentar el rendimiento, deberíamos aumentar el número de hilos.  
Pero no podemos incrementarlos indefinidamente (por ejemplo, hasta `Integer.MAX_VALUE`) porque son hilos del sistema operativo (OS threads), que son costosos y el propio sistema operativo impone límites.  
Esto se demostrará más adelante en el curso.

Los _Java virtual threads_ son hilos **ligeros**, no son hilos del sistema operativo, y podemos crear muchísimos para hacer la aplicación más escalable.  
Podemos seguir escribiendo código en el estilo tradicional _un hilo por petición_, pero creando grandes cantidades de hilos virtuales.  
Eso sí, hay un detalle importante (un “catch”) que se explicará en la primera sección.

El objetivo final del curso es aprender a desarrollar sistemas escalables utilizando _Java virtual threads_.

### Contenido del curso

1. **Introducción profunda a los virtual threads:** cómo funcionan internamente, cuándo y dónde usarlos, cuándo no, sus limitaciones y conceptos clave.
    
2. **Uso con Executor Service:** cómo ejecutar tareas de forma más eficiente combinando _executor service_ y _virtual threads_.
    
3. **Integración con `CompletableFuture`:** cómo aprovechar _CompletableFuture_ con _virtual threads_ para operaciones asíncronas.  
    _(Nota: `CompletableFuture` no es programación reactiva)._
    
4. **Nueva API: Structured Concurrency:** Java está introduciendo una API en vista previa llamada _Structured Concurrency_. Se explicará brevemente su dirección futura.
    
5. **Aplicación práctica:** desarrollo de una aplicación simple usando _virtual threads_ con Spring Boot Web.
    
6. **Pruebas de rendimiento con JMeter:** se medirán los resultados para evaluar si los _virtual threads_ realmente mejoran la escalabilidad.
    
7. **Comparación con programación reactiva:** se aclara la duda común: los _virtual threads_ y _structured concurrency_ no reemplazan la programación reactiva, que se basa en otros principios. Se incluirá una clase extra al final para profundizar.
    

---

**Resumen completo**

El documento presenta la introducción y el temario de un curso especializado en _Java virtual threads_.  
Explica que en Java, cada instrucción se ejecuta en un hilo, y que tradicionalmente se utilizan hilos del sistema operativo para manejar solicitudes concurrentes, como en aplicaciones Spring Boot con Tomcat.  
Aunque aumentar el número de hilos podría mejorar el rendimiento, existe un límite debido al alto costo de los hilos del sistema operativo.

Los _virtual threads_ de Java surgen como una solución: son hilos extremadamente ligeros, permiten crear una gran cantidad y mantener el estilo clásico de _un hilo por petición_, mejorando la escalabilidad. Sin embargo, hay consideraciones y limitaciones que se estudiarán en profundidad.

El curso abarca: fundamentos y funcionamiento interno de los _virtual threads_, su uso con `ExecutorService` y `CompletableFuture`, una introducción a la nueva API de _Structured Concurrency_, el desarrollo de una aplicación práctica con Spring Boot y la realización de pruebas de rendimiento con JMeter. Finalmente, se aclara que los _virtual threads_ no reemplazan la programación reactiva; esta sigue teniendo un papel importante y distinto.