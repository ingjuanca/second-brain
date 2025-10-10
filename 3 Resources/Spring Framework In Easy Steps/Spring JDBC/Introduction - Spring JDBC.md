
---

## Introducción

El documento explica cómo usar **Spring JDBC Template**, una herramienta de Spring que **simplifica el acceso a bases de datos** eliminando gran parte del código repetitivo que normalmente se debe escribir con **JDBC (Java Database Connectivity)**.  
Además, muestra cómo crear un proyecto Maven desde cero, configurar las dependencias necesarias, y realizar una operación **INSERT** en una tabla llamada `Employee`.

---

## 1️⃣ Problema con el uso directo de JDBC

Cuando se usa JDBC puro, el desarrollador debe escribir mucho código para:

- Crear una conexión a la base de datos.
    
- Crear un `Statement` o `PreparedStatement`.
    
- Asignar parámetros.
    
- Ejecutar la consulta.
    
- Procesar los resultados.
    
- Cerrar los recursos.
    

Este código se repite en diferentes clases y genera redundancia.

---

## 2️⃣ Solución: **Spring JDBC Template**

Spring ofrece la clase `JdbcTemplate`, que combina **JDBC** con el **patrón de diseño Template**.  
Este componente contiene todo el “boilerplate code” (código repetitivo), por lo que el desarrollador solo necesita concentrarse en la lógica del negocio.

**Para usar `JdbcTemplate`, se necesita:**

- Un **DataSource** (fuente de datos).
    
- Spring provee una implementación llamada **DriverManagerDataSource**.
    
- Este objeto requiere cuatro parámetros:
    
    1. `driverClassName` (nombre del driver JDBC).
        
    2. `url` (dirección de la base de datos).
        
    3. `username`.
        
    4. `password`.
        

El `DataSource` es responsable de crear la conexión con la base de datos.

---

## 3️⃣ Creación de la tabla **Employee**

Antes de usar Spring JDBC, se debe crear una tabla para las pruebas.

### Pasos:

1. Abrir MySQL Workbench u otro cliente SQL.
    
2. Ejecutar los siguientes comandos:
    
```sql
USE mydb; 
CREATE TABLE employee (     
	id INT,     
	firstname VARCHAR(50),     
	lastname VARCHAR(50) 
); 
SELECT * FROM employee;
```
    
3. Inicialmente, la tabla estará vacía.
    

Esta tabla se usará más adelante para insertar registros usando `JdbcTemplate`.

---

## 4️⃣ Creación del proyecto Maven

### Pasos en Eclipse:

1. Ir a **File → New → Maven Project**.
    
2. Seleccionar la plantilla `maven-archetype-quickstart`.
    
3. Definir:
    
    - **Group Id:** (por defecto)
        
    - **Artifact Id:** `springjdbc`
        
4. Finalizar.
    

### Dependencias necesarias (`pom.xml`):

- `spring-core`
    
- `spring-context`
    
- `spring-jdbc`
    
- `mysql-connector-java`
    

También se incluye el plugin de compilación para usar **Java 1.8**.

### Ejemplo de configuración:

```xml

<properties>
	<springframework.version>4.3.6.RELEASE</springframework.version>
</properties>

<dependencies>     
	
	<dependency>         
		<groupId>org.springframework</groupId>         
		<artifactId>spring-core</artifactId>         
		<version>${springframework.version}</version>     
	</dependency>     
	
	<dependency>         
		<groupId>org.springframework</groupId>         
		<artifactId>spring-context</artifactId>       
		<version>${springframework.version}</version>     
	</dependency>     
	
	<dependency>         
	  <groupId>org.springframework</groupId>         
	  <artifactId>spring-jdbc</artifactId>        
	  <version>${springframework.version}</version>     
	</dependency>     
	
	<dependency>         
	  <groupId>mysql</groupId>         
	  <artifactId>mysql-connector-java</artifactId>    
	  <version>5.1.6</version>     
	</dependency> 
	
</dependencies>
```

Finalmente, se debe actualizar el proyecto con **Maven → Update Project** para descargar todas las dependencias.

---

## 5️⃣ Configurar el **DataSource** y el **JdbcTemplate**

En el archivo `config.xml`, se definen los beans de Spring.

### Bean de `DriverManagerDataSource`:

```xml
	<bean class="org.springframework.jdbc.datasource.DriverManagerDataSource"
		name="dataSource" p:driverClassName="com.mysql.jdbc.Driver" p:url="jdbc:mysql://localhost/mydb" p:username="root" p:password="test" />
```

### Bean de `JdbcTemplate`:

```xml
<bean class="org.springframework.jdbc.core.JdbcTemplate" name="jdbcTemplate"
		p:dataSource-ref="dataSource" />
```

**Relación:**  
El bean `jdbcTemplate` **depende** del bean `dataSource`, el cual le provee la conexión a la base de datos.

---

## 6️⃣ Uso del `JdbcTemplate` para insertar datos

Crear una clase de prueba, por ejemplo: `Test.java`.

```java
package com.bharath.spring.springjdbc;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.JdbcTemplate;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext("com/bharath/spring/springjdbc/config.xml");
		JdbcTemplate jdbcTemplate = (JdbcTemplate) context.getBean("jdbcTemplate");
		String sql = "insert into employee values(?,?,?)";
		int result = jdbcTemplate.update(sql, new Integer(1), "Bharath", "Thippireddy");
		System.out.println("Number of records inserted are: " + result);

	}

}

```

**Explicación:**

- Se obtiene el bean `JdbcTemplate` del contexto de Spring.
    
- Se define una sentencia SQL con placeholders (`?`).
    
- Se ejecuta `update(sql, valores...)` para insertar los datos.
    
- El método devuelve el número de filas afectadas.
    

Al ejecutar, se mostrará:

`Number of records inserted: 1`

Y si se ejecuta:

`SELECT * FROM employee;`

El resultado será:

`1 | John | Doe`

---

## 7️⃣ Flujo interno de ejecución

1. Spring carga el archivo `config.xml`.
    
2. Se crean los beans:
    
    - `dataSource` → configura conexión con la base de datos.
        
    - `jdbcTemplate` → depende del `dataSource`.
        
3. Al ejecutar el test:
    
    - Spring crea el objeto `JdbcTemplate`.
        
    - Este usa el `dataSource` para abrir una conexión.
        
    - Crea y ejecuta el `PreparedStatement`.
        
    - Inyecta los valores (`id`, `firstname`, `lastname`).
        
    - Ejecuta el `INSERT`.
        
    - Devuelve el resultado (número de filas afectadas).
        

**Ventaja principal:**  
El desarrollador **no escribe código JDBC manualmente**, ni maneja conexiones o cierres de recursos.  
Spring lo hace automáticamente.

---

## 8️⃣ Resumen general

|Elemento|Descripción|
|---|---|
|**DriverManagerDataSource**|Crea y gestiona la conexión con la base de datos.|
|**JdbcTemplate**|Ejecuta operaciones SQL de manera simplificada.|
|**update()**|Método usado para ejecutar `INSERT`, `UPDATE` o `DELETE`.|
|**Boilerplate Code**|Código repetitivo que Spring elimina automáticamente.|
|**Ventaja clave**|Reduce el código manual, mejora la legibilidad y facilita el mantenimiento.|

---

## ✅ Conclusión

Spring JDBC Template:

- Simplifica la interacción con bases de datos.
    
- Elimina el código repetitivo de JDBC puro.
    
- Gestiona automáticamente conexiones, sentencias y resultados.
    
- Usa configuración por beans para definir `DataSource` y `JdbcTemplate`.
    
- Facilita operaciones como `INSERT`, `UPDATE` y `DELETE` con métodos simples.
    

En resumen, **Spring JDBC Template** permite realizar operaciones sobre bases de datos **de forma limpia, rápida y segura**, aprovechando la inyección de dependencias y el patrón de diseño _Template_ para un desarrollo mucho más eficiente. ✅