
---

## Resumen de **Autowiring** en Spring

En esta lección se hace un **repaso rápido** de todo lo aprendido sobre **autowiring** (autoconexión) en Spring.

---

### ¿Qué es Autowiring?

Es el **proceso mediante el cual el contenedor de Spring inyecta automáticamente los objetos requeridos** sin necesidad de declarar manualmente `<ref>` en el XML.

Existen **dos grandes enfoques** para implementarlo:

---

### 1️⃣ Configuración mediante **XML**

Spring ofrece tres formas:

1. **byType**
    
    - Busca un bean cuyo **tipo de clase** coincida con la dependencia.
        
    - Usa **setter injection** para inyectar el objeto.
        
2. **byName**
    
    - Busca un bean cuyo **id (nombre)** en el XML coincida exactamente con el **nombre de la propiedad** en la clase.
        
    - También usa **setter injection**.
        
3. **byConstructor**
    
    - Usa **inyección por constructor**.
        
    - Localiza un bean cuyo tipo coincida con el **parámetro del constructor**.
        

---

### 2️⃣ Configuración mediante **anotaciones**

La forma **más utilizada**:

- **@Autowired**
    
    - Se coloca sobre un campo, un método setter o un constructor.
        
    - Spring inyecta automáticamente el bean del tipo requerido.
        
- **@Qualifier**
    
    - Se usa junto con `@Autowired` cuando hay **varios beans del mismo tipo**.
        
    - Permite especificar el **nombre exacto del bean** que se debe inyectar.
        

---

**Resumen completo del documento:**

El **autowiring** en Spring permite que el contenedor **inyecte automáticamente las dependencias**, sin tener que escribir referencias manuales en el archivo de configuración.

Hay **dos formas principales**:

1. **XML**:
    
    - `byType` y `byName` realizan la inyección mediante **setters**,
        
    - `byConstructor` usa **constructor injection**.
        
2. **Anotaciones**:
    
    - `@Autowired` marca los campos o métodos para inyección automática.
        
    - Si existen varios beans del mismo tipo y surge ambigüedad, se combina con `@Qualifier` para indicar el **bean específico**.
        

En pocas palabras, Spring ofrece tanto **configuración XML** como **anotaciones** para lograr autowiring, siendo las anotaciones la técnica **más usada en aplicaciones modernas**.