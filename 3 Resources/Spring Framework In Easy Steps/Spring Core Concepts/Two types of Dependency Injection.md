
---

En esta lección se hace un **resumen de la inyección de dependencias** y se explican los **dos tipos de inyección de dependencias** que soporta el **contenedor de Spring**.

La **inyección de dependencias** es el proceso de **inyectar, en tiempo de ejecución y de manera dinámica, los recursos que un objeto necesita**.  
El **contenedor de Spring** se encarga de realizar este proceso por nosotros.

---

### Ejemplo

Supongamos que tenemos una **clase Employee** que depende de:

- **ID**,
- **Name**,
- Y de un objeto **Address**.

La clase **Address** tiene sus propios campos:

- **Street**,
- **City**,
- **State**,
- **Country**.

En **tiempo de ejecución**, el contenedor de Spring:

1. **Crea el objeto Address**,
2. **Inyecta sus valores** (street, city, state, country),
3. **Crea el objeto Employee** con su ID y name,
4. **Inyecta el objeto Address dentro del objeto Employee**.

---

### Dos formas de inyección en Spring

Spring permite hacer esto de dos maneras:

#### 1. **Setter Injection (inyección por métodos set)**

- Se proveen **métodos getter y setter** en las clases.
    
- El contenedor de Spring:
    
    - Crea el objeto Address,
        
    - Establece los valores de street, city, state y country usando los **métodos setter**,
        
    - Crea el objeto Employee,
        
    - Llama a los métodos **setId**, **setName** y finalmente **setAddress** para inyectar la dependencia.
        

Por eso se llama **setter injection** o **property injection**, porque usa los **métodos set** para establecer las dependencias.

---

#### 2. **Constructor Injection (inyección por constructor)**

- En lugar de setters, se define un **constructor con parámetros**.
    
- El contenedor:
    
    - Crea el objeto Address pasando street, city, state y country al **constructor**,
        
    - Luego crea el objeto Employee, pasando ID, name y el objeto Address al **constructor parametrizado**.
        

Por eso se llama **constructor injection**, ya que el contenedor usa el **constructor con parámetros** para inyectar las dependencias.

---

**Resumen completo del documento:**

La **inyección de dependencias** permite que el **contenedor de Spring** cree y asigne, en tiempo de ejecución, los recursos que un objeto necesita, sin que el desarrollador tenga que instanciarlos manualmente.

En el ejemplo, Spring crea primero el objeto **Address**, inyecta sus valores, luego crea el objeto **Employee**, le asigna su ID, nombre y le inyecta el objeto Address.

Spring admite dos tipos de inyección:

1. **Setter Injection:** El contenedor usa los **métodos setter** de la clase para asignar las dependencias.
    
2. **Constructor Injection:** El contenedor usa un **constructor con parámetros** para pasar directamente las dependencias necesarias.
    

Estos dos mecanismos permiten flexibilidad para manejar dependencias en las aplicaciones desarrolladas con Spring.