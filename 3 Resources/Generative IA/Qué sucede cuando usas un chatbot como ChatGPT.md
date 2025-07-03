
---
### **¿Qué sucede cuando usas un chatbot como ChatGPT?**

El documento explica el proceso que ocurre cuando un usuario interactúa con un chatbot de inteligencia artificial como ChatGPT. Aquí están los puntos clave:

1. **Dos componentes principales**:
    
    - **La aplicación (como ChatGPT)**: Es una interfaz web o móvil creada por ingenieros humanos (por ejemplo, de OpenAI).
    - **El modelo de IA**: Es el sistema que genera las respuestas.
2. **Procesamiento del mensaje del usuario**:
    
    - Cuando escribes algo, **primero lo recibe la aplicación**, no el modelo directamente.
    - La aplicación puede **filtrar o modificar tu entrada** antes de enviarla al modelo (por ejemplo, bloquear ciertas palabras).
3. **Historial del chat**:
    
    - El modelo no solo recibe tu último mensaje, sino **todo el historial de la conversación** para generar respuestas más coherentes.
    - Este historial es gestionado por la aplicación.
4. **Mensajes invisibles (System Prompt)**:
    
    - Existen mensajes ocultos que tú no ves, como configuraciones que indican al modelo cómo comportarse (por ejemplo, ser amable o evitar temas polémicos).
    - Estos también se incluyen en el historial que recibe el modelo.
5. **Filtrado de respuestas**:
    
    - La aplicación también puede **filtrar las respuestas del modelo** antes de mostrártelas.
    - A veces puedes ver una respuesta generándose y luego desaparecer, reemplazada por un mensaje como “Lo siento, no puedo ayudarte”.
6. **Otras formas de interactuar con modelos de IA**:
    
    - Puedes usar **APIs** o incluso **alojar tu propio modelo**.
    - Si alojas el modelo tú mismo, tienes más control y sabes exactamente qué se le envía.
    - Con APIs, puedes configurar el mensaje del sistema, pero aún puede haber procesamiento adicional por parte del proveedor.