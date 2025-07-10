
---

### 🛠️ **Resumen: ¿Cómo usan herramientas los modelos de lenguaje como ChatGPT?**

Este documento explica cómo los modelos de lenguaje pueden usar herramientas como búsqueda web o ejecución de código, y aclara que **no lo hacen por sí solos**.

---

### 🤖 **¿Los modelos usan herramientas por sí mismos?**

- **No.** Los modelos de lenguaje (LLMs) **no pueden abrir navegadores ni ejecutar código por su cuenta**.
- Siguen siendo **máquinas de predicción de tokens**. No tienen agencia ni control directo sobre herramientas externas.

---

### 🧩 **¿Cómo funciona entonces?**

![[Pasted image 20250709215417.png]]

1. **Aplicación envolvente (como ChatGPT)**:
    
    - Es la interfaz que recibe tu entrada y **controla la interacción con el modelo**.
    - Puede insertar **mensajes invisibles** en el historial del chat, como instrucciones sobre qué herramientas puede usar el modelo.
2. **Sistema de instrucciones ocultas**:
    
    - Se le dice al modelo (mediante el _system prompt_) que puede usar herramientas como `web_search` o `code_execution`.
    - Si el modelo detecta que necesita buscar algo, puede generar una salida como:  
        `web_search: "¿Quién es el presidente actual de EE.UU.?"`
3. **La aplicación interpreta esa salida**:
    
    - No se muestra directamente al usuario.
    - La aplicación detecta que se debe hacer una búsqueda, **ejecuta el código necesario** (escrito por ingenieros humanos), y obtiene los resultados.
4. **Resultados como entrada oculta**:
    
    - Los resultados de la herramienta (por ejemplo, la búsqueda web) se **insertan de nuevo en el historial del chat** como mensajes invisibles.
    - El modelo los usa para generar una respuesta final que **sí ves tú como usuario**.

---

### 🔄 **Ejemplo del flujo completo**

1. Tú haces una pregunta.
2. El modelo sugiere usar una herramienta.
3. La aplicación ejecuta esa herramienta.
4. Los resultados se insertan en el contexto.
5. El modelo genera una respuesta basada en esos resultados.

---

### ✅ **Conclusión**

- Los modelos **no usan herramientas directamente**.
- Es la **aplicación** la que gestiona el uso de herramientas, guiada por instrucciones ocultas y configuraciones.
- Esto aplica a **búsqueda web, ejecución de código y otras funciones**.

---