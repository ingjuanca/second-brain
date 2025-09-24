
---

### Resumen de la **Inyección por Setter**

En esta lección se hace un **repaso rápido** de lo aprendido en la sección de **inyección por métodos setter**.

---

**Definición:**

- La **inyección por setter** es el proceso mediante el cual el **contenedor de Spring** usa los **métodos setter de un objeto** para realizar la **inyección de dependencias**.
    

---

### Pasos principales para aplicar inyección de dependencias en Spring

1. **Crear los Spring Beans**
    
    - Definir las clases de la aplicación como **POJOs** (Plain Old Java Objects).
        
2. **Crear el archivo de configuración XML de Spring**
    
    - Especificar en él:
        
        - Cómo se deben crear los **beans**,
            
        - Qué **dependencias** deben inyectarse.
            
3. **Obtener los beans desde el contenedor de Spring**
    
    - Usarlos en **clases de prueba** o en las **clases principales de la aplicación**.
        

---

### Tipos de datos inyectados

1. **Tipos primitivos**
    
    - Se inyectaron de tres formas:
        
        - Usando `<value>` como **elemento**,
            
        - Usando `value` como **atributo**,
            
        - Usando **P schema (namespace p)**, una manera práctica y conveniente.
            
2. **Colecciones**
    
    - Se inyectaron utilizando los elementos de colección en XML:
        
        - `<list>`,
            
        - `<set>`,
            
        - `<map>`,
            
        - `<props>`.
            
3. **Objetos o tipos de referencia**
    
    - Se inyectaron con el elemento `<ref>` dentro de `<property>`:
        
        - `<ref>` puede usarse como **elemento**,
            
        - o como **atributo** del bean.
            

---

**Resumen completo del documento:**

La **inyección por setter** en Spring consiste en que el **contenedor llama a los métodos setter de las clases POJO** para suministrar las dependencias requeridas.  
El proceso se realiza en **tres pasos**:

1. **Crear los beans** de la aplicación (clases Java simples).
    
2. **Definir el archivo XML de configuración**, indicando la creación de los beans y las dependencias que Spring debe inyectar.
    
3. **Recuperar los beans del contenedor** y utilizarlos en pruebas o en el código de la aplicación.
    

Durante la práctica se aprendió a inyectar:

- **Tipos primitivos**, de tres formas (value como elemento, value como atributo y P schema),
    
- **Colecciones** (list, set, map, props),
    
- **Objetos o referencias** (usando `<ref>` como elemento o atributo).
    

Este repaso concluye la sección de **setter injection**, resaltando las técnicas básicas de inyección de dependencias que proporciona Spring.