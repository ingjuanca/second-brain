
---

Cuando el **contenedor de Spring** crea un objeto y realiza la **inyección de dependencias**, lo hace según **el tipo de datos** de las dependencias que ese objeto necesita.  
En base a esto, existen **tres tipos de dependencias**:

1. **Dependencias de tipo primitivo**
    
    - Son los campos que utilizan **tipos primitivos de Java**, tales como:
        
        - `byte`, `short`, `int`, `long`, `float`, `double`,
          `boolean`, `char`, y también **`String`**.
        
    - Spring identifica estos tipos como **dependencias primitivas** y sabe cómo inyectarlas.
        
2. **Dependencias de tipo colección**
    
    - Incluyen las estructuras de datos de Java:
        
        - `List`, `Set`, `Map` y `Properties`.
            
    - Spring sabe crear e inyectar estos objetos de colección en las clases que los requieran.
        
3. **Dependencias de tipo referencia (objetos)**
    
    - Son las **más comunes en una aplicación Java**.
        
    - Ocurren cuando una clase **depende de otra clase** (un objeto de otro tipo).
        
    - Spring crea automáticamente una instancia de la clase requerida y la **inyecta en la clase dependiente**.
        

---

**Resumen completo del documento:**

Al crear e inyectar dependencias, el **contenedor de Spring** clasifica las dependencias en **tres categorías**:

1. **Primitivos:** datos básicos de Java (`int`, `double`, `boolean`, `String`, etc.).
    
2. **Colecciones:** estructuras de datos (`List`, `Set`, `Map`, `Properties`).
    
3. **Referencias (objetos):** cuando una clase necesita una **instancia de otra clase**.
    

Spring sabe **crear y suministrar automáticamente** los valores u objetos para cada tipo, de modo que el desarrollador no debe preocuparse por la instanciación manual. Estas tres categorías se usan frecuentemente en las aplicaciones Java y se abordarán en las prácticas del curso.