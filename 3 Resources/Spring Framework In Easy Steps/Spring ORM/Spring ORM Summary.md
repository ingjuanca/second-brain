
---

En esta lección se resume todo lo aprendido en la sección de **Spring ORM**.

**ORM** significa _Object Relational Mapping_ (Mapeo Objeto-Relacional).  
Herramientas o frameworks como **Hibernate** permiten **mapear los objetos Java a filas de bases de datos relacionales**.

Estas herramientas:

- Generan automáticamente el SQL necesario para **insertar, eliminar, actualizar y consultar** registros.
    
- **Convierten los objetos Java en filas de una tabla** y, cuando los datos son recuperados, **transforman las filas nuevamente en objetos**.
    

---

### 🔹 **Integración de Spring con Hibernate**

Spring simplifica el uso de Hibernate mediante una integración que provee la clase **`HibernateTemplate`**.

Para usarla, es necesario configurarla.  
El `HibernateTemplate` depende de un **`SessionFactory`**, el cual a su vez requiere:

1. **DataSource:**
    
    - Se encarga de crear la conexión con la base de datos.
        
    - Incluye el driver, la URL, el usuario y la contraseña.
        
2. **hibernateProperties:**  
    Son un conjunto de propiedades clave-valor que configuran el comportamiento de Hibernate.  
    Las principales son:
    
    - **`hibernate.dialect`**
        
        - Especifica la clase que genera el SQL adecuado según la base de datos utilizada.
            
        - Cada motor (MySQL, Oracle, PostgreSQL, etc.) tiene su propio dialecto.
            
        - Ejemplo:
            
            `hibernate.dialect=org.hibernate.dialect.MySQLDialect`
            
    - **`hibernate.show_sql`**
        
        - Es una propiedad booleana (verdadero/falso).
            
        - Por defecto está en **false**.
            
        - Si se configura en **true**, Hibernate mostrará en la consola todas las consultas SQL generadas, lo cual ayuda a depurar y comprender qué está ocurriendo en la base de datos.
            
3. **annotatedClasses:**
    
    - Define las clases que representan entidades.
        
    - Es el primer paso al trabajar con Hibernate: **crear las entidades (clases Java mapeadas a tablas)**.
        
    - Esto se hace mediante anotaciones de **JPA (Java Persistence API)**, tales como:
        
        - `@Entity`
            
        - `@Table`
            
        - `@Column`
            
        - `@Id`
            

---

### 🔹 **Uso de HibernateTemplate**

Una vez configurado el `HibernateTemplate`, podemos realizar todas las operaciones **CRUD** (Create, Read, Update, Delete) mediante sus métodos incorporados:

|Método|Descripción|
|---|---|
|`save()`|Inserta un nuevo registro en la base de datos.|
|`update()`|Actualiza un registro existente.|
|`delete()`|Elimina un registro existente.|
|`get()`|Recupera un solo objeto (una fila) usando su ID.|
|`loadAll()`|Recupera una lista de objetos (todas las filas de la tabla).|

Al utilizar estos métodos:

- **No se escribe SQL manualmente.**
    
- El programador trabaja directamente con **objetos Java**.
    
- Hibernate se encarga de **convertir los objetos en sentencias SQL** y ejecutarlas.
    

---

### 🧠 **Resumen general**

|Concepto|Descripción|
|---|---|
|**ORM (Object Relational Mapping)**|Técnica que mapea objetos Java a tablas de base de datos.|
|**Hibernate**|Framework ORM más popular, implementa el estándar JPA.|
|**Spring ORM**|Módulo de Spring que integra Hibernate mediante `HibernateTemplate`.|
|**DataSource**|Maneja las conexiones a la base de datos.|
|**SessionFactory**|Fabrica de sesiones Hibernate; centraliza la configuración y cache.|
|**hibernate.dialect**|Define el dialecto SQL del motor de base de datos usado.|
|**hibernate.show_sql**|Muestra en consola las consultas SQL generadas.|
|**annotatedClasses**|Lista de entidades mapeadas (clases con anotaciones JPA).|
|**@Entity / @Table / @Column / @Id**|Anotaciones que vinculan clases y campos Java con tablas y columnas SQL.|
|**HibernateTemplate CRUD**|Proporciona métodos (`save`, `update`, `delete`, `get`, `loadAll`) para operar sin escribir SQL.|

---

### ✅ **Conclusión final**

- **ORM** automatiza el vínculo entre objetos y tablas, evitando escribir SQL.
    
- **Hibernate** implementa ORM usando el estándar **JPA**, y **Spring** lo simplifica aún más mediante **`HibernateTemplate`**.
    
- La configuración requiere:
    
    - Un `DataSource` (conexión).
        
    - Propiedades (`hibernate.dialect`, `show_sql`).
        
    - Entidades anotadas (`@Entity`, `@Id`, etc.).
        
- Una vez configurado, es posible ejecutar operaciones CRUD completas usando solo métodos de alto nivel.
    

> En resumen: **Spring ORM con Hibernate permite trabajar con bases de datos de manera totalmente orientada a objetos**, eliminando el código SQL manual y reduciendo drásticamente la complejidad de acceso a datos. ✅