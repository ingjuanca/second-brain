
---

### 🔹 ¿Qué es ORM?

**ORM (Object Relational Mapping)** significa _Mapeo Objeto-Relacional_.  
Su propósito es facilitar el trabajo con bases de datos al **convertir objetos de Java en filas de tablas** y viceversa, sin necesidad de escribir SQL manualmente.

Cuando usamos **JDBC** o incluso **Spring JDBC**, el programador debe:

1. Crear manualmente la consulta SQL usando los datos del objeto.    
2. Ejecutarla contra la base de datos.    
3. Convertir los resultados del `ResultSet` nuevamente a objetos Java.
    

Todo ese proceso es manual, repetitivo y propenso a errores.

El ORM automatiza ese flujo:

- El desarrollador solo define una **clase entidad (POJO)** con su **mapeo** a la tabla de base de datos.    
- El framework ORM genera automáticamente las consultas SQL necesarias (INSERT, UPDATE, SELECT, DELETE).    
- También convierte los resultados en objetos Java.
    

---

### 🔹 ¿Qué es JPA?

**JPA (Java Persistence API)** es el **estándar de Java EE** para ORM.  
Fue definido por Oracle y funciona como especificación (contrato).

- **API:** usada por los desarrolladores (anotaciones, interfaces, clases).    
- **Implementaciones:** como Hibernate, EclipseLink o TopLink, que cumplen esa especificación.
    

👉 **Hibernate** es la implementación más popular de JPA (actualmente en versión 5.0).

---

### 🔹 Ventajas del ORM

- No se escribe SQL manualmente.    
- El framework genera las consultas automáticamente.    
- Convierte objetos a registros y viceversa.    
- Simplifica el trabajo del desarrollador orientado a objetos.
    

---

## 🧩 Spring ORM

Así como Spring simplifica JDBC mediante `JdbcTemplate`, también simplifica el uso de herramientas ORM como Hibernate mediante **`HibernateTemplate`**.

El `HibernateTemplate`:

- Oculta la creación y gestión de sesiones de Hibernate.
    
- Permite ejecutar operaciones comunes:
    
    - `save()` – Inserta un objeto.        
    - `update()` – Actualiza un objeto.        
    - `delete()` – Elimina un objeto.        
    - `get()` – Obtiene un registro único.        
    - `loadAll()` – Obtiene una lista de registros (lista de objetos).
        

Estas operaciones trabajan directamente con **objetos Java**, no con SQL.

---

### 🔹 Arquitectura de una aplicación Spring ORM

Ejemplo: una entidad `Product`.

1. **Entidad (Product):** clase Java con anotaciones de mapeo.
    
2. **ProductDAO:** interfaz que declara operaciones con la base de datos.
    
3. **ProductDAOImpl:** implementación que usa `HibernateTemplate`.
    
4. **HibernateTemplate:** depende de un **SessionFactory**.
    
5. **SessionFactory:** administra las sesiones de Hibernate.
    
    - Su implementación en Spring es `LocalSessionFactoryBean`.
        
6. **DataSource:** provee la conexión a la base de datos.
    
7. **TransactionManager:** maneja las transacciones (asegura que las operaciones sean atómicas).
    

---

### 🔹 Dependencias principales de `LocalSessionFactoryBean`

El `LocalSessionFactoryBean` requiere tres dependencias:

1. **DataSource:**
    
    - Define el driver, URL, usuario y contraseña de la base de datos.
        
2. **hibernateProperties:**
    
    - Son configuraciones de Hibernate como:
        
        - `hibernate.dialect`: Clase que genera SQL según el motor de base de datos (MySQL, Oracle, etc.).  
            Ejemplo:
            
            `hibernate.dialect = org.hibernate.dialect.MySQLDialect`
            
        - `hibernate.show_sql`: Muestra las consultas SQL generadas automáticamente en la consola.
            
            `hibernate.show_sql = true`
            
3. **annotatedClasses:**
    
    - Lista de las clases mapeadas como entidades (por ejemplo, `Product.class`).
        

---

## 🧩 Mapeo de Entidades a Tablas (Anotaciones JPA)

Al usar ORM, lo único que el desarrollador debe proveer es el **mapeo entre los campos de las clases y las columnas de la tabla**.

Esto se puede hacer con **XML** o **anotaciones**, aunque las **anotaciones** son más comunes.

### 🔹 Anotaciones principales de JPA

|Anotación|Descripción|
|---|---|
|**@Entity**|Marca una clase como entidad gestionada por JPA. Es obligatoria.|
|**@Id**|Indica el campo que actúa como clave primaria (Primary Key). Obligatoria.|
|**@Table**|Es opcional. Permite especificar un nombre de tabla distinto al de la clase.|
|**@Column**|También opcional. Permite mapear un campo a una columna con nombre distinto.|

---

### 🔹 Ejemplo: Clase `Employee`

```java
@Entity 
@Table(name="emp") public class Employee {     
	@Id     
	private int id;     
	private String firstName;     
	private String lastName; 
}
```

**Explicación:**

- `@Entity` → indica que la clase es una entidad gestionada por Hibernate.
    
- `@Table(name="emp")` → la clase se asocia a la tabla `emp`.
    
- `@Id` → el campo `id` será la clave primaria.
    
- Los campos `firstName` y `lastName` se asocian automáticamente a las columnas del mismo nombre.
    

> Si los nombres de los atributos y columnas son diferentes, se puede usar `@Column(name="columna_db")`.

---

## 🧩 Creación de la tabla `Product` en MySQL

El documento indica cómo crear la tabla `product` en la base de datos **mydb**.

### 🔹 Pasos:

1. Abrir MySQL Workbench.
    
2. Seleccionar la base de datos:
    
    `USE mydb;`
    
3. Crear la tabla:
    
```sql
CREATE TABLE product (     
	id INT,     
	name VARCHAR(20),     
	description VARCHAR(100),     
	price DECIMAL(8,3) 
);
```
    
    - `id` → entero (clave primaria).        
    - `name` → texto de hasta 20 caracteres.        
    - `description` → texto de hasta 100 caracteres.        
    - `price` → número decimal con 8 dígitos en total y 3 después del punto.        
4. Ejecutar:
    
    `SELECT * FROM product;`
    
    Inicialmente no habrá registros.
    

---

## 🧩 Resumen general

| Tema                           | Descripción                                                                                 |
| ------------------------------ | ------------------------------------------------------------------------------------------- |
| **ORM**                        | Permite mapear objetos Java a tablas de base de datos automáticamente.                      |
| **JPA**                        | Estándar de Java para ORM. Hibernate es su implementación más usada.                        |
| **HibernateTemplate (Spring)** | Simplifica el uso de Hibernate dentro de Spring.                                            |
| **SessionFactory**             | Administra las sesiones de Hibernate; implementado en Spring por `LocalSessionFactoryBean`. |
| **DataSource**                 | Administra las conexiones a la base de datos.                                               |
| **Hibernate Properties**       | Configuran el comportamiento (dialecto, mostrar SQL, etc.).                                 |
| **Entity Mappings**            | Se definen con anotaciones `@Entity`, `@Id`, `@Table`, `@Column`.                           |
| **Product Table**              | Ejemplo práctico de creación de una tabla para mapear con una clase `Product`.              |

---

## ✅ **Conclusión**

En este documento se introducen los fundamentos de:

- Qué es **ORM** y cómo facilita la interacción entre objetos y bases de datos.
    
- Cómo **Spring ORM** simplifica Hibernate mediante el uso de `HibernateTemplate` y `SessionFactory`.
    
- Qué **configuraciones** se necesitan (`DataSource`, `dialect`, `show_sql`, `annotatedClasses`).
    
- Cómo **mapear una entidad Java** a una tabla mediante las anotaciones `@Entity`, `@Id`, `@Table`, `@Column`.
    
- Y finalmente, cómo crear una tabla `product` en la base de datos para trabajar con una entidad real.
    

> En resumen, ORM automatiza la conversión entre objetos y tablas; Spring ORM integra Hibernate con facilidad, y las anotaciones JPA permiten definir el mapeo de forma declarativa y elegante.