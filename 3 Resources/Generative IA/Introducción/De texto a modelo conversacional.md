
---
### 📌 **Resumen: De texto a modelo conversacional – Cómo se entrena un modelo de lenguaje**

Este documento explica en detalle cómo se transforma texto en bruto en un modelo de lenguaje conversacional como ChatGPT.

---

### 🧩 **1. De texto a tokens**

![[Pasted image 20250704094556.png]]

- El texto no se usa directamente para entrenar el modelo, sino que se convierte en **tokens** (fragmentos de palabras o palabras completas con un ID numérico).
- Hay alrededor de **200,000 tokens únicos**, incluyendo puntuación, espacios y mayúsculas/minúsculas.
- Ejemplo: “Hello” y “hello” son tokens distintos.

---

### 🔢 **2. De tokens a vectores (embeddings)**

![[Pasted image 20250704094704.png]]

- Los tokens se convierten en **vectores numéricos** (embeddings) que representan su significado en un espacio multidimensional.

![[Pasted image 20250704094747.png]]

- Palabras relacionadas (como “fuego” y “calor”) se ubican cerca en ese espacio.
- Esto permite que el modelo entienda relaciones semánticas.

---

### 🧠 **3. Entrenamiento del modelo base (pre-entrenamiento)**

![[Pasted image 20250704094848.png]]

- Los vectores se usan para entrenar una **red neuronal con miles de millones de parámetros**.
- El modelo aprende a **predecir el siguiente token** en una secuencia.
- Ejemplo: si el input es “El fuego está”, el modelo predice tokens como “ardiendo”, “caliente”, etc., con diferentes probabilidades.
- Este proceso es **costoso y largo**, y da como resultado un **modelo base** o _foundation model_.

---

### 💬 **4. Limitaciones del modelo base**

![[Pasted image 20250704095003.png]]

- El modelo base solo **completa texto**, no responde de forma útil.
- Ejemplo: si preguntas “¿Cuál es el sentido de la vida?”, podría responder con otra pregunta como “¿Y por qué existimos?”.

---

### 🛠️ **5. Ajuste fino (fine-tuning)**

![[Pasted image 20250704095402.png]]

- Para convertirlo en un **asistente útil**, se realiza un segundo entrenamiento con **conversaciones creadas por humanos**.

![[Pasted image 20250704095904.png]]

- Estas conversaciones enseñan al modelo a:
    - Responder preguntas.
    - Ser amable y evitar respuestas ofensivas.
    - No responder a ciertos temas sensibles.
- Hoy en día, parte de este entrenamiento puede hacerse con **datos sintéticos generados por otros modelos**.

---

### 🤖 **6. Resultado final**

- El modelo ajustado se convierte en un **asistente conversacional** como ChatGPT.
- Aunque sigue siendo un sistema de **predicción de tokens**, el ajuste fino lo hace parecer más inteligente, útil y amigable.

---