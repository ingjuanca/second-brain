
---

Ahora, para concluir esta sección, también quiero mencionar que no solo puedes ejecutar esos modelos de código abierto localmente. En cambio, también puedes ejecutar, alojar, podrías decir, esos modelos en servidores remotos que posees o alquilas. Y hay dos opciones principales para hacer eso. Podrías, por supuesto, simplemente poseer un servidor en tus instalaciones, así que en tu propio centro de datos o (risas) tal vez en tu habitación. O podrías alquilar un servidor privado virtual. Podrías usar un servicio como AWS, EC2 o Hetzner para hacerlo.

Y la idea aquí es que realmente posees o alquilas y configuras ese servidor, así que tienes acceso completo a él. Tú gestionas el software que se ejecuta en ese servidor, e instalas una herramienta como Ollama, LM Studio, LLaMA.cpp, para luego ejecutar un modelo en ese servidor y exponerlo a través de una API a otras aplicaciones en ese servidor, o tal vez también a otras aplicaciones que se ejecutan en otros servidores o en tu máquina.

Por lo tanto, también necesitas configurar la red para hacer eso. Y adjunto, también encontrarás un artículo sobre cómo podrías alojar modelos en una máquina remota y cómo podrías exponerlos a la World Wide Web, por ejemplo, para que puedas acceder a esos modelos alojados desde cualquier parte del mundo, por ejemplo, desde dentro de tus propias aplicaciones. Pero, por supuesto, hacer eso es totalmente opcional y requiere cierta experiencia técnica. La ventaja de esta opción es que solo pagas por el servidor, así que sabes de antemano cuánto vas a pagar por un mes determinado.

La alternativa es usar un servicio administrado, algo como Amazon Bedrock, por ejemplo, que es un servicio ofrecido por AWS que facilita bastante la selección y ejecución de modelos, modelos de código abierto o modelos proporcionados por Amazon, en la nube, así que en servidores gestionados por AWS. Y no tienes que gestionar ni configurar esos servidores por tu cuenta al usar Bedrock. Ahora, si deseas aprender más sobre AWS o Amazon Bedrock, te recomiendo que sigas mi curso de certificación dedicado. Y adjunto, también encontrarás un artículo que te ayudará a comenzar a usar Bedrock para ejecutar modelos de código abierto con él. La ventaja, como se mencionó, es que realmente no necesitas pasar por ninguna configuración. No necesitas instalar ningún software, y por lo tanto, no se requiere experiencia técnica, pero pagas por el uso.

Ahora, eso puede ser una ventaja si no tienes mucho uso. Podría ser más barato que pagar por un servidor completo por adelantado. Pero, por supuesto, si lo usas mucho, pagar por un servidor podría ser más barato (risas) que pagar por el uso. Así que simplemente depende de tu caso de uso específico.

También cabe señalar que no es el enfoque de este curso. Aquí estamos hablando de usar la IA de manera eficiente, no sobre la gestión de servidores. Por eso te guié a esos otros recursos, como el artículo adjunto, si deseas aprender más sobre eso. Pero aún así, vale la pena conocer estas opciones que tienes y que puedes ejecutar modelos localmente o de forma remota en servidores o con servicios administrados como Bedrock.

---

**Resumen:**

El documento describe las opciones para alojar modelos de lenguaje de código abierto, ya sea localmente en servidores propios o alquilados, o utilizando servicios administrados como Amazon Bedrock. Se mencionan dos enfoques: ejecutar modelos en servidores locales, lo que requiere experiencia técnica y configuración, o utilizar servicios en la nube que simplifican el proceso al no requerir instalación de software. Hugging Face se destaca como un recurso para encontrar modelos de código abierto. La elección entre estas opciones depende del caso de uso específico del usuario, considerando factores como el costo y la necesidad de gestión técnica.

### Notas

| # Configuring Ollama AI LLM on an EC2 instance in AWS | https://medium.com/@sreskills/configuring-ollama-ai-llm-on-an-ec2-instance-in-aws-12cff0f5d83b |
| ----------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| **# Using Open LLMs On-Demand via Bedrock**           | https://maximilian-schwarzmueller.com/articles/using-open-models-on-demand-via-bedrock/        |

