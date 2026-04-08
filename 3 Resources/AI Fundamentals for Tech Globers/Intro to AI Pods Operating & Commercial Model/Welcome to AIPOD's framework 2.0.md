
---

Bienvenido al marco de trabajo 2.0 de AIPOD. Este es nuestro sistema de producción para escalar la entrega mediante capacidades basadas en código y una supervisión rigurosa. Veamos cómo se conectan las piezas:

Todo comienza a la izquierda con nuestra gente. El **Arquitecto del AI Pod** diseña la maquinaria de entrega gestionando el repositorio de conocimiento. Este repositorio contiene nuestras configuraciones YAML y, fundamentalmente, nuestras "habilidades" (_skills_): implementaciones estándar con control de versiones que codifican exactamente cómo Globant produce artefactos. Junto al arquitecto, el **Experto de Dominio** actúa como nuestro guardián de calidad, activando el trabajo y validando todos los resultados.

Una vez activado, el trabajo entra en el **Plano de Ejecución**. Aquí, el orquestador _Stepwise_ toma el control, aplicando estrictamente las fases de investigación, planificación e implementación definidas en cada capacidad o "Stream". Un _Stream_ es una unidad de capacidad específica de un dominio que combina el juicio humano con la ejecución de la IA. _Stepwise_ interpreta la capacidad y ordena al entorno de ejecución de agentes realizar las tareas. Aunque nuestro motor preferido es Koda, esta capa es "enchufable" y puede ejecutar otros agentes de línea de comandos como Claude Code, Gemini CLI o CodeEx.

Los agentes utilizan las habilidades proporcionadas para ejecutar el trabajo de forma segura y confiable, interactuando con los sistemas del cliente. Crucialmente, nuestros agentes no adivinan. Se basan en el **Paquete de Contexto**, que se inyecta directamente desde repositorios del cliente como Jira, Git y Confluence, asegurando que cada artefacto se alinee con sus estándares. El sistema sincroniza automáticamente el trabajo validado de vuelta a estos repositorios (ramas de Git, páginas de Confluence o tickets de Jira) sin interrupciones.

A medida que se crean los artefactos, el trabajo pasa al **Plano de Supervisión**. Los expertos de dominio revisan los resultados; si un artefacto falla, lo rechazan y lo registran en el historial de calibración (_calibration backlog_). El arquitecto usa este historial para ajustar el repositorio de conocimiento, asegurando que el sistema aprenda y el error no se repita. Finalmente, las llamadas a los modelos de lenguaje (LLM) se realizan de forma segura a través de _Globant Enterprise AI_, que actúa como frontera de confianza, capturando telemetría y alimentando tableros de gobernanza.

La IA ejecuta, los humanos validan y el sistema aprende. Así entregamos capacidad predecible.

![[Pasted image 20260408151156.png]]

---

## Ideas más Relevantes

- **Sistema de Producción Basado en Roles:** El modelo se apoya en dos figuras humanas clave: el **Arquitecto**, que diseña y calibra el sistema, y el **Experto de Dominio**, que activa y valida el trabajo.
    
- **Ejecución vs. Supervisión:** Existe una división clara entre el **Plano de Ejecución** (donde la IA realiza las tareas técnicas) y el **Plano de Supervisión** (donde los humanos garantizan la calidad y el cumplimiento).
    
- **Eliminación de la Improvisación:** Los agentes no "adivinan"; operan bajo reglas estrictas denominadas "habilidades" (_skills_) y se alimentan de contexto real del cliente proveniente de herramientas como Jira o Git.
    
- **Aprendizaje Continuo del Sistema:** Los errores detectados por los humanos no solo se corrigen, sino que se registran para ajustar el repositorio de conocimiento, evitando que la IA repita los mismos fallos en el futuro.
    
- **Gobernanza y Seguridad:** Todas las interacciones con la IA pasan por una plataforma empresarial (_Globant Enterprise AI_) que garantiza la seguridad de los datos, la observabilidad y el control operativo.
    
- **Interoperabilidad:** El sistema permite utilizar diferentes motores de IA (Koda, Claude, Gemini) según las necesidades del proyecto, manteniendo un estándar de ejecución común.