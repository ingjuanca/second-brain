
---

**El cambio de paradigma: de la asistencia a la delegación** Históricamente, los humanos eran quienes ejecutaban: el desarrollador escribía el código y el tester realizaba las pruebas. Las herramientas solo asistían al humano. En el modelo de **AI Pod**, esta dinámica se invierte: el sistema ejecuta el trabajo (genera código, borradores de casos de prueba y documentación), mientras que los humanos se desplazan a una capa de supervisión y gobernanza. Ya no se paga a humanos por teclear, sino al sistema por generar y a los humanos por gobernar.

En este sistema de producción, el conocimiento de Globant sobre cómo construir soluciones ya está codificado en el proceso. El enfoque cognitivo humano se centra ahora en:

- **Orquestación:** Preparar paquetes de contexto para que la IA tenga la información exacta antes de generar.
    
- **Validación:** Controlar las puertas de calidad y revisar los resultados.
    
- **Calibración:** Registrar ajustes para que la maquinaria mejore en cada sprint.
    

**Evolución de los roles en la línea de producción** Se debe visualizar el AI Pod como un entorno de manufactura avanzada donde el profesional está en la sala de control y no en la línea de ensamblaje:

- **Arquitecto de AI Pod (APA):** Diseña la solución, configura las "listas de reproducción" (playlists) y supervisa el proceso sin ejecutar el trabajo directamente.
    
- **Experto de Dominio (DE):** Actúa como el control de calidad crítico. Revisa resultados, corrige desviaciones y enseña a la IA a alcanzar los estándares de Globant.
    

**La importancia de la validación** Dado que la IA puede generar contenido masivamente, el cuello de botella ya no es la creación, sino la validación. El valor comercial se mide a través de **"tokens validados"**: las líneas de código o diagramas generados por la IA no tienen valor hasta que un experto humano los revisa, refina y aprueba.

**Anatomía y Tecnología del AI Pod** El sistema se divide en tres bloques principales:

1. **Streams (Flujos):** Unidades de capacidad estandarizadas (producto, desarrollo, QA) con mejores prácticas y controles integrados.
    
2. **Playlists (Listas de reproducción):** La orquestación de múltiples flujos para entregar un proyecto de principio a fin.
    
3. **Agentes:** Herramientas de IA especializadas que ejecutan las tareas. Son estrictamente internas y gestionadas por expertos para lograr una eficiencia del 20% al 30%.
    

Todo esto corre sobre un **Stack de Ejecución** que incluye la plataforma de IA de Globant (infraestructura segura), **Koda CLI** (motor de automatización) y **Stepwise** (orquestación de procedimientos estándar para humanos y máquinas).

---

### Ideas más Relevantes

- **De ejecución humana a ejecución del sistema:** La IA pasa de ser un asistente a ser el motor que ejecuta las tareas técnicas iniciales (código, pruebas, documentos).
    
- **El nuevo rol humano:** El valor del profesional se desplaza hacia el diseño de contextos, la edición de alta calidad y la gobernanza del sistema.
    
- **Tokens validados:** El éxito no se mide por cuánto contenido genera la IA, sino por cuántos de esos componentes son verificados y firmados por un experto humano.
    
- **Infraestructura estandarizada:** El uso de _Streams_ y _Playlists_ permite que el desarrollo de software funcione como una fábrica predecible y escalable.
    
- **Eficiencia operativa:** Los agentes de IA son herramientas internas diseñadas para absorber el trabajo repetitivo, permitiendo ahorros de tiempo y costos del 20-30%.
    
- **Curva de aprendizaje:** Delegar en lugar de escribir desde cero puede sentirse extraño al principio, pero para el tercer sprint se convierte en "memoria muscular" que permite entregas a una escala superior.