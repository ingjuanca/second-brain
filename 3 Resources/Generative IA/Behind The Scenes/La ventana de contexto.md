
---
### 📌 **Resumen: La importancia de la ventana de contexto en los modelos de lenguaje**

Este documento explica un concepto clave en el funcionamiento de los modelos de lenguaje: la **ventana de contexto**.

---

### 🧠 **¿Qué es la ventana de contexto?**

![[Pasted image 20250709212923.png]]

- Es la **cantidad de texto** (tokens) que un modelo puede considerar al generar una respuesta.
- Incluye todo el historial del chat, preguntas anteriores, respuestas y cualquier texto adicional (como el contenido de un PDF).

---

### 📄 **¿Qué pasa cuando subes un documento?**

- El texto del PDF es **extraído automáticamente** por la aplicación (como ChatGPT) y **agregado al prompt** antes de tu pregunta.
- Esto significa que el modelo recibe **todo ese contenido como parte del contexto**.

---

### ⚙️ **Desafíos con textos largos**

- Cuanto más largo es el texto, **más difícil es para el modelo** mantener coherencia y entender relaciones entre partes distantes del texto.
- Por ejemplo, para resumir un documento de 30 páginas, el modelo debe comprender la relación entre el inicio, el medio y el final del texto.

---

### 📈 **Evolución de la ventana de contexto**

- Modelos antiguos como **GPT-3 (2022)** tenían ventanas de contexto pequeñas, útiles solo para preguntas cortas.
- Modelos modernos (como **Google Gemini**) pueden manejar **millones de tokens**, lo que permite tareas más complejas como:
    - Resumir libros completos.
    - Analizar múltiples artículos científicos.

---

### ⚠️ **Limitaciones actuales**

- Aunque las ventanas de contexto han crecido, **la calidad puede disminuir** con entradas muy largas.
- **Modelos más económicos** suelen tener ventanas más pequeñas, lo que puede ser un problema si se usan para analizar documentos extensos.

---

### ✅ **Conclusión**

- La **ventana de contexto es clave** para tareas complejas.
- Elegir el modelo adecuado según el tamaño del texto es esencial para obtener buenos resultados.

---