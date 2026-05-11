
---

No, no se trata de _esos_ transformers. Pero pueden hacer cosas muy geniales. Por ejemplo, un ordenador creó este chiste: "¿Por qué el plátano cruzó la carretera? ¡Porque estaba harto de que lo machacaran!". Literalmente le pedí que me contara un chiste y eso fue lo que se le ocurrió.

Específicamente, utilicé **GPT-3**, o un modelo de **transformador generativo preentrenado**. El "3" significa que es la tercera generación. GPT-3 es un modelo de lenguaje autorregresivo que produce texto con apariencia de haber sido escrito por un humano. Puede escribir poesía, redactar correos electrónicos y, evidentemente, inventar sus propios chistes. Aunque el chiste del plátano no es exactamente divertido, encaja en el patrón típico de un chiste con introducción y remate, y tiene cierto sentido.

### ¿Qué es un Transformer?

Un Transformer es algo que transforma una secuencia en otra. La traducción de idiomas es un gran ejemplo: tomar una frase en inglés y traducirla al francés.

Los Transformers constan de dos partes:

1. **Codificador (_Encoder_):** Trabaja sobre la secuencia de entrada. Genera codificaciones que definen qué partes de la secuencia de entrada son relevantes entre sí y las pasa a la siguiente capa.
    
2. **Decodificador (_Decoder_):** Opera sobre la secuencia de salida objetivo. Utiliza el contexto derivado de las codificaciones para generar la secuencia de salida.
    

La traducción no es una simple búsqueda de palabras equivalentes, ya que el orden de las palabras y los giros idiomáticos complican las cosas. Los Transformers funcionan mediante el **aprendizaje de secuencia a secuencia**, donde el modelo toma una secuencia de tokens (palabras) y predice la siguiente palabra en la secuencia de salida.

### Características y Ventajas

- **Aprendizaje semisupervisado:** Se preentrenan de forma no supervisada con grandes conjuntos de datos sin etiquetar y luego se ajustan (_fine-tuning_) mediante entrenamiento supervisado para mejorar su rendimiento.
    
- **Mecanismo de Atención:** A diferencia de las Redes Neuronales Recurrentes (RNN), los Transformers no procesan los datos necesariamente en orden. El mecanismo de atención proporciona contexto sobre los elementos de la secuencia, identificando qué palabras aportan significado a otras, sin importar su posición.
    
- **Paralelismo:** El mecanismo de atención permite ejecutar múltiples secuencias en paralelo. Esto acelera enormemente los tiempos de entrenamiento en comparación con algoritmos que deben ejecutarse de forma secuencial.
    

### Aplicaciones de los Transformers

Más allá de la traducción, se aplican en:

- **Resúmenes de documentos:** Generar un par de frases que resuman los puntos principales de un artículo largo.
    
- **Creación de contenido:** Escribir publicaciones completas para blogs.
    
- **Otros campos:** Aprender a jugar al ajedrez o realizar procesamiento de imágenes con capacidades que rivalizan con las redes neuronales convolucionales.
    

Los Transformers son modelos de aprendizaje profundo potentes que mejoran constantemente gracias a que su mecanismo de atención puede paralelizarse.

---

## Ideas más Relevantes

- **Definición de Transformer:** Es un modelo de aprendizaje profundo diseñado para transformar una secuencia de entrada en una secuencia de salida mediante un sistema de **codificador y decodificador**.
    
- **Mecanismo de Atención:** Es la innovación clave que permite al modelo identificar el **contexto y la relevancia** entre palabras de una secuencia sin necesidad de procesarlas en orden lineal.
    
- **Eficiencia en el entrenamiento:** Gracias a la capacidad de procesar datos en **paralelo**, los Transformers reducen drásticamente los tiempos de entrenamiento en comparación con modelos anteriores como las RNN.
    
- **Versatilidad Multimodal:** Aunque son famosos por el lenguaje (traducción, redacción, resúmenes), su arquitectura permite aplicaciones en **ajedrez e imágenes**.
    
- **Aprendizaje Híbrido:** Utilizan un enfoque **semisupervisado**, combinando un preentrenamiento masivo con datos no etiquetados y un ajuste fino supervisado para tareas específicas.