
---

### Introducción

Hola, soy Megha. Hoy voy a hablar sobre los **Modelos de Lenguaje Extensos** (LLMs, por sus siglas en inglés). En este curso, aprenderás a definirlos, describir sus casos de uso, explicar el ajuste de instrucciones (_prompt tuning_) y conocer las herramientas de desarrollo de IA generativa de Google.

### ¿Qué son los LLMs?

- Los LLMs son un subconjunto del **aprendizaje profundo** (_deep learning_).
    
- Se cruzan con la **IA generativa**, la cual es un tipo de IA capaz de producir contenido nuevo como texto, imágenes, audio y datos sintéticos.
    
- Se definen como modelos de lenguaje de propósito general, de gran tamaño, que pueden ser pre-entrenados y luego ajustados (_fine-tuned_) para propósitos específicos.
    

### Características Principales: "Grande" y "Propósito General"

1. **Grande:** Se refiere tanto al tamaño enorme del conjunto de datos de entrenamiento (a escala de petabytes) como a la cantidad de **parámetros** (el conocimiento y memoria que la máquina aprende durante el entrenamiento).
    
2. **Propósito General:** Son capaces de resolver problemas comunes de lenguaje debido a la base común del lenguaje humano y a que solo ciertas organizaciones tienen los recursos para entrenarlos inicialmente.
    

### Entrenamiento y Ajuste

- **Pre-entrenamiento:** El modelo se entrena con un conjunto de datos masivo para tareas generales (como un perro que aprende comandos básicos: "siéntate", "ven").
    
- **Ajuste fino (_Fine-tuning_):** El modelo se adapta a tareas específicas (como un perro de servicio para la policía o guía) usando conjuntos de datos más pequeños de campos como finanzas o salud.
    

### Beneficios y Casos de Uso

- Un solo modelo puede realizar múltiples tareas: traducción, completar oraciones, clasificación de texto y responder preguntas.
    
- Requieren pocos datos de entrenamiento específicos para alcanzar un buen rendimiento, permitiendo escenarios de **pocos ejemplos** (_few-shot_) o incluso **cero ejemplos** (_zero-shot_).
    
- Se basan casi exclusivamente en modelos **Transformer**, que consisten en un codificador (_encoder_) y un decodificador (_decoder_).
    

### Desarrollo de LLMs vs. ML Tradicional

- **LLM:** No requiere ser un experto ni tener miles de ejemplos de entrenamiento; el enfoque está en el **diseño de prompts** (crear entradas claras e informativas).
    
- **ML Tradicional:** Requiere conocimientos especializados, hardware, tiempo de cómputo y muchos ejemplos de entrenamiento.
    

### Diseño y Ajuste de Prompts

- **Diseño de Prompts:** Crear una entrada adaptada a una tarea específica.
    
- **Ingeniería de Prompts:** Un concepto más especializado para mejorar el rendimiento mediante conocimientos del dominio o ejemplos de salida deseada.
    
- **Razonamiento de cadena de pensamiento (_Chain-of-thought_):** Los modelos obtienen mejores respuestas cuando primero explican el razonamiento antes de dar el resultado final.
    

### Tipos de LLMs

1. **Modelos de lenguaje genéricos:** Predicen la siguiente palabra basándose en los datos de entrenamiento (estilo "auto-completar").
    
2. **Modelos ajustados por instrucciones (_Instruction-tuned_):** Entrenados para responder a instrucciones específicas (ej. "resume este texto").
    
3. **Modelos ajustados por diálogo (_Dialog-tuned_):** Entrenados para mantener conversaciones y responder preguntas de chat.
    

### Herramientas de Google Cloud

- **Vertex AI Studio:** Para explorar y personalizar modelos generativos rápidamente.
    
- **Vertex AI Agent Builder:** Permite crear agentes de chat y búsqueda con poco o nada de código.
    
- **Gemini:** Un modelo de IA **multimodal** capaz de analizar texto, imágenes, audio y código de programación.
    
- **Model Garden:** Una biblioteca de modelos continuamente actualizada.
    

---

## Ideas más Relevantes

- **Evolución del Desarrollo:** Hemos pasado de la programación tradicional (reglas rígidas) a las redes neuronales y ahora a la IA generativa, donde el usuario genera contenido simplemente preguntando.
    
- **Eficiencia en el Ajuste:** El ajuste fino total es costoso, por lo que existen métodos de **ajuste eficiente en parámetros (PETM)** que añaden capas pequeñas sin alterar el modelo base.
    
- **Eliminación de la barrera de entrada:** El desarrollo con LLMs reduce drásticamente la necesidad de conocimientos técnicos profundos y datos masivos en comparación con el aprendizaje automático tradicional.
    
- **Multimodalidad:** La nueva frontera (ejemplificada por Gemini) es la capacidad de un solo modelo para entender e integrar diversos tipos de datos simultáneamente, no solo texto.