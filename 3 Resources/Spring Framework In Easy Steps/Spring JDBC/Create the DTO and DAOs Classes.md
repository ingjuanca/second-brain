
---

### 1️⃣ Creación de las clases **DTO** y **DAO**

Hasta ahora, la aplicación usaba directamente `JdbcTemplate` para insertar registros.  
En este punto se busca hacerla más realista siguiendo buenas prácticas de arquitectura.

**Paso 1:** Crear una clase `Employee` (DTO o entidad), que representa la tabla `employee` en la base de datos.  
Tendrá los campos:

- `int id`
- `String firstName`
- `String lastName`
    

Esta clase contiene los **getters**, **setters** y un **método toString()**.  
Sirve para transferir los datos (de ahí el nombre Data Transfer Object).

```java
package com.bharath.spring.springjdbc.employee.dto;

public class Employee {

	private int id;
	private String firstName;
	private String lastName;

	@Override
	public String toString() {
		return "Employee [id=" + id + ", firstName=" + firstName + ", lastName=" + lastName + "]";
	}

	// add the getters and setters

}
```

---

**Paso 2:** Crear la interfaz `EmployeeDAO`, que contendrá los métodos para realizar operaciones en la base de datos.  
Inicialmente se define un solo método:

`int create(Employee employee);`

```java
package com.bharath.spring.springjdbc.employee.dao;

import java.util.List;

import com.bharath.spring.springjdbc.employee.dto.Employee;

public interface EmployeeDao {
	
	int create(Employee employee);

}

```

**Paso 3:** Crear la clase `EmployeeDAOImpl` que implementa la interfaz anterior.  
Aquí se define cómo ejecutar realmente el método usando `JdbcTemplate`.

En esta clase se:

- Declara un campo `private JdbcTemplate jdbcTemplate;`
    
- Se crean los **métodos getter y setter** para ese campo.
    
- En el método `create()`, se ejecuta:
    
    `String sql = "INSERT INTO employee VALUES (?, ?, ?)";` 
    `int result = jdbcTemplate.update(sql, employee.getId(), employee.getFirstName(), employee.getLastName()); return result;`
    

```java
package com.bharath.spring.springjdbc.employee.dao.impl;

import org.springframework.jdbc.core.JdbcTemplate;

import com.bharath.spring.springjdbc.employee.dao.EmployeeDao;
import com.bharath.spring.springjdbc.employee.dto.Employee;

public class EmployeeDaoImpl implements EmployeeDao {

	private JdbcTemplate jdbcTemplate;

	@Override
	public int create(Employee employee) {
		String sql = "INSERT INTO employee VALUES (?, ?, ?)";
		int result = jdbcTemplate.update(sql, employee.getId(), employee.getFirstName(), employee.getLastName());
		return result;
	}

	public JdbcTemplate getJdbcTemplate() {
		return jdbcTemplate;
	}

	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
		this.jdbcTemplate = jdbcTemplate;
	}

}

```

Esta clase separa la lógica de acceso a datos del resto de la aplicación, aplicando el patrón **DAO**.

---

### 2️⃣ Configuración con Spring

En el archivo `config.xml` se agregan los beans:

```xml
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver" />    
    <property name="url" value="jdbc:mysql://localhost/mydb" />     
    <property name="username" value="root" />     
    <property name="password" value="test" /> 
</bean>  

<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">     
	<property name="dataSource" ref="dataSource" /> 
</bean>  

<bean id="employeeDAO" class="com.bharath.employee.dao.impl.EmployeeDAOImpl">    
	<property name="jdbcTemplate" ref="jdbcTemplate" /> 
</bean>
```

Así, Spring:

1. Crea el `DataSource`.
    
2. Lo inyecta en `JdbcTemplate`.
    
3. Inyecta el `JdbcTemplate` en `EmployeeDAOImpl`.
    

---

### 3️⃣ Creación y ejecución del **test**

En el archivo `Test.java`:

- Se obtiene el contexto Spring:
    
    `ApplicationContext context = new ClassPathXmlApplicationContext("com/springjdbc/employee/test/config.xml"); EmployeeDAO dao = (EmployeeDAO) context.getBean("employeeDAO");`
    
- Se crea un nuevo `Employee`:
    
    `Employee e = new Employee(); 
    e.setId(2); 
    e.setFirstName("John"); 
    e.setLastName("Ferguson");`
    
- Y se llama:
    
    `int result = dao.create(e); System.out.println("Number of records inserted are: " + result);`
    

Tras ejecutar el programa, en la base de datos se ve que el registro se insertó correctamente.

```java
package com.bharath.spring.springjdbc;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.JdbcTemplate;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springjdbc/employee/test/config.xml");
		EmployeeDao dao = (EmployeeDao) context.getBean("employeeDao");
		Employee employee = new Employee();
		employee.setId(2);
		employee.setFirstName("John");
		employee.setLastName("Ferguson");
		int result = dao.create(employee);
		System.out.println("Number of records inserted are: " + result);
	}

}

```

---

### 4️⃣ Flujo general de la aplicación

1. Spring lee `config.xml` y crea los beans:
    
    - `dataSource`
        
    - `jdbcTemplate`
        
    - `employeeDAO`
        
2. Cada bean se **inyecta** en el siguiente según sus dependencias.
    
3. El test crea un `Employee` y llama al método `create()`.
    
4. Dentro del DAO, `JdbcTemplate.update()` ejecuta la consulta SQL.
    
5. Spring reemplaza los placeholders `?` con los valores del objeto.
    

---

### 5️⃣ Implementación del método **update()**

Se agrega al `EmployeeDAO`:

```java
package com.bharath.spring.springjdbc.employee.dao;

import java.util.List;

import com.bharath.spring.springjdbc.employee.dto.Employee;

public interface EmployeeDao {
	
	int create(Employee employee);
	
	int update(Employee employee);

}

```

Y en la implementación:

```java
package com.bharath.spring.springjdbc.employee.dao.impl;

import org.springframework.jdbc.core.JdbcTemplate;

import com.bharath.spring.springjdbc.employee.dao.EmployeeDao;
import com.bharath.spring.springjdbc.employee.dto.Employee;

public class EmployeeDaoImpl implements EmployeeDao {

	private JdbcTemplate jdbcTemplate;

	@Override
	public int create(Employee employee) {
		String sql = "INSERT INTO employee VALUES (?, ?, ?)";
		int result = jdbcTemplate.update(sql, employee.getId(), employee.getFirstName(), employee.getLastName());
		return result;
	}
	
	@Override
	public int update(Employee employee) {
		String sql = "update employee set firstname=?,lastname=? where id=?";
		int result = jdbcTemplate.update(sql, employee.getFirstName(), employee.getLastName(), employee.getId());
		return result;
	}

	public JdbcTemplate getJdbcTemplate() {
		return jdbcTemplate;
	}

	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
		this.jdbcTemplate = jdbcTemplate;
	}

}

```

En el test:

- Se comenta el bloque de `create()`.
    
- Se actualiza el registro:
    
```java
package com.bharath.spring.springjdbc;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.JdbcTemplate;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springjdbc/employee/test/config.xml");
		EmployeeDao dao = (EmployeeDao) context.getBean("employeeDao");
		Employee employee = new Employee();
		employee.setId(2);
		employee.setFirstName("Bob");
		employee.setLastName("Ferguson");
		int result = dao.update(employee);
		System.out.println("Number of records inserted are: " + result);
	}

}
```
    

Después de ejecutarlo, el registro con ID 2 pasa de “John” a “Bob”.

---

### 6️⃣ Implementación del método **delete()**

Se agrega:

`int delete(int id);`

```java
package com.bharath.spring.springjdbc.employee.dao;

import java.util.List;

import com.bharath.spring.springjdbc.employee.dto.Employee;

public interface EmployeeDao {
	
	int create(Employee employee);
	
	int update(Employee employee);
	
	int delete(int id);

}

```

Y en el DAO:

`String sql = "DELETE FROM employee WHERE id=?"; int result = jdbcTemplate.update(sql, id); return result;`

```java
package com.bharath.spring.springjdbc.employee.dao.impl;

import org.springframework.jdbc.core.JdbcTemplate;

import com.bharath.spring.springjdbc.employee.dao.EmployeeDao;
import com.bharath.spring.springjdbc.employee.dto.Employee;

public class EmployeeDaoImpl implements EmployeeDao {

	private JdbcTemplate jdbcTemplate;

	@Override
	public int create(Employee employee) {
		String sql = "INSERT INTO employee VALUES (?, ?, ?)";
		int result = jdbcTemplate.update(sql, employee.getId(), employee.getFirstName(), employee.getLastName());
		return result;
	}

	@Override
	public int update(Employee employee) {
		String sql = "update employee set firstname=?,lastname=? where id=?";
		int result = jdbcTemplate.update(sql, employee.getFirstName(), employee.getLastName(), employee.getId());
		return result;
	}

	@Override
	public int delete(int id) {
		String sql = "delete from employee where id=?";
		int result = jdbcTemplate.update(sql, id);
		return result;
	}

	public JdbcTemplate getJdbcTemplate() {
		return jdbcTemplate;
	}

	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
		this.jdbcTemplate = jdbcTemplate;
	}

}

```


En el test:

`int result = dao.delete(1); System.out.println("Number of records deleted: " + result);`

```java
package com.bharath.spring.springjdbc;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.JdbcTemplate;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springjdbc/employee/test/config.xml");
		EmployeeDao dao = (EmployeeDao) context.getBean("employeeDao");
		Employee employee = new Employee();
		employee.setId(2);
		employee.setFirstName("Bob");
		employee.setLastName("Ferguson");
		int result = dao.delete(1);
		System.out.println("Number of records inserted are: " + result);
	}

}
```

Tras ejecutarlo, el registro con ID 1 desaparece de la base de datos.

---

### 7️⃣ Consultas **SELECT** (Lecturas)

Hasta aquí se usaban operaciones **no select** (insert, update, delete).  
Ahora se añaden lecturas con `queryForObject` y `query`:

- `queryForObject`: devuelve **un solo registro**.
    
- `query`: devuelve **una lista** de objetos.
    

Ambos necesitan un **RowMapper**, una interfaz que transforma cada fila del `ResultSet` en un objeto Java.

---

### 8️⃣ Creación del **RowMapper**

Se crea una clase:

```java
package com.bharath.spring.springjdbc.employee.dao.rowmapper;

import java.sql.ResultSet;
import java.sql.SQLException;

import org.springframework.jdbc.core.RowMapper;

import com.bharath.spring.springjdbc.employee.dto.Employee;

public class EmployeeRowMapper implements RowMapper<Employee> {

	@Override
	public Employee mapRow(ResultSet rs, int rowNum) throws SQLException {
		Employee emp = new Employee();
		emp.setId(rs.getInt(1));
		emp.setFirstName(rs.getString(2));
		emp.setLastName(rs.getString(3));
		return emp;
	}

}

```

Esta clase convierte cada fila de la base de datos en un objeto `Employee`.

---

### 9️⃣ Método **read()** (leer un registro por ID)

En la interfaz:

`Employee read(int id);`

```java
package com.bharath.spring.springjdbc.employee.dao;

import java.util.List;

import com.bharath.spring.springjdbc.employee.dto.Employee;

public interface EmployeeDao {
	
	int create(Employee employee);
	
	int update(Employee employee);
	
	int delete(int id);
	
	Employee read(int id);

}
```

En la implementación:

`String sql = "SELECT * FROM employee WHERE id=?"; EmployeeRowMapper rowMapper = new EmployeeRowMapper(); Employee emp = jdbcTemplate.queryForObject(sql, rowMapper, id); return emp;`

```java
package com.bharath.spring.springjdbc.employee.dao.impl;

import org.springframework.jdbc.core.JdbcTemplate;

import com.bharath.spring.springjdbc.employee.dao.EmployeeDao;
import com.bharath.spring.springjdbc.employee.dto.Employee;

public class EmployeeDaoImpl implements EmployeeDao {

	private JdbcTemplate jdbcTemplate;

	@Override
	public int create(Employee employee) {
		String sql = "INSERT INTO employee VALUES (?, ?, ?)";
		int result = jdbcTemplate.update(sql, employee.getId(), employee.getFirstName(), employee.getLastName());
		return result;
	}

	@Override
	public int update(Employee employee) {
		String sql = "update employee set firstname=?,lastname=? where id=?";
		int result = jdbcTemplate.update(sql, employee.getFirstName(), employee.getLastName(), employee.getId());
		return result;
	}

	@Override
	public int delete(int id) {
		String sql = "delete from employee where id=?";
		int result = jdbcTemplate.update(sql, id);
		return result;
	}
	
	@Override
	public Employee read(int id) {
		String sql = "select * from employee where id=?";
		EmployeeRowMapper rowmapper = new EmployeeRowMapper();
		Employee employee = jdbcTemplate.queryForObject(sql, rowmapper, id);
		return employee;
	}

	public JdbcTemplate getJdbcTemplate() {
		return jdbcTemplate;
	}

	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
		this.jdbcTemplate = jdbcTemplate;
	}

}
```

En el test:

`Employee e = dao.read(2); System.out.println(e);`

```java
package com.bharath.spring.springjdbc;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.JdbcTemplate;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springjdbc/employee/test/config.xml");
		EmployeeDao dao = (EmployeeDao) context.getBean("employeeDao");
		Employee employee = new Employee();
		employee.setId(2);
		employee.setFirstName("Bob");
		employee.setLastName("Ferguson");
		Employee employee = dao.read(2);
		System.out.println("Employee Record: " + result);
	}

}
```

Resultado: se imprime el objeto `Employee` con ID 2.

---

### 🔟 Método **read()** (leer todos los registros)

Se agrega otro método:

`List<Employee> read();`

```java
package com.bharath.spring.springjdbc.employee.dao;

import java.util.List;

import com.bharath.spring.springjdbc.employee.dto.Employee;

public interface EmployeeDao {
	
	int create(Employee employee);
	
	int update(Employee employee);
	
	int delete(int id);
	
	Employee read(int id);
	
	List<Employee> read();

}
```

Y en la implementación:

`String sql = "SELECT * FROM employee"; EmployeeRowMapper rowMapper = new EmployeeRowMapper(); List<Employee> employees = jdbcTemplate.query(sql, rowMapper); return employees;`

```java
package com.bharath.spring.springjdbc.employee.dao.impl;

import org.springframework.jdbc.core.JdbcTemplate;

import com.bharath.spring.springjdbc.employee.dao.EmployeeDao;
import com.bharath.spring.springjdbc.employee.dto.Employee;

public class EmployeeDaoImpl implements EmployeeDao {

	private JdbcTemplate jdbcTemplate;

	@Override
	public int create(Employee employee) {
		String sql = "INSERT INTO employee VALUES (?, ?, ?)";
		int result = jdbcTemplate.update(sql, employee.getId(), employee.getFirstName(), employee.getLastName());
		return result;
	}

	@Override
	public int update(Employee employee) {
		String sql = "update employee set firstname=?,lastname=? where id=?";
		int result = jdbcTemplate.update(sql, employee.getFirstName(), employee.getLastName(), employee.getId());
		return result;
	}

	@Override
	public int delete(int id) {
		String sql = "delete from employee where id=?";
		int result = jdbcTemplate.update(sql, id);
		return result;
	}
	
	@Override
	public Employee read(int id) {
		String sql = "select * from employee where id=?";
		EmployeeRowMapper rowmapper = new EmployeeRowMapper();
		Employee employee = jdbcTemplate.queryForObject(sql, rowmapper, id);
		return employee;
	}
	
	@Override
	public List<Employee> read() {
		String sql = "select * from employee";
		EmployeeRowMapper rowmapper = new EmployeeRowMapper();
		List<Employee> result = jdbcTemplate.query(sql,rowmapper);
		return result;
	}

	public JdbcTemplate getJdbcTemplate() {
		return jdbcTemplate;
	}

	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
		this.jdbcTemplate = jdbcTemplate;
	}

}
```

En el test:

```java
package com.bharath.spring.springjdbc;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.JdbcTemplate;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springjdbc/employee/test/config.xml");
		EmployeeDao dao = (EmployeeDao) context.getBean("employeeDao");
		Employee employee = new Employee();
		employee.setId(2);
		employee.setFirstName("Bob");
		employee.setLastName("Ferguson");
		List<Employee> result = dao.read();
		System.out.println("Employee Record: " + result);
	}

}

```

Al ejecutarlo, se muestran todos los registros de la tabla `employee`.

---

## 🧠 CONCLUSIÓN GENERAL

|Concepto|Descripción|
|---|---|
|**DTO / Entity**|Clase que representa la tabla y transporta datos.|
|**DAO (Data Access Object)**|Clase que centraliza la lógica de acceso a la base de datos.|
|**JdbcTemplate**|Simplifica el uso de JDBC eliminando el código repetitivo.|
|**RowMapper**|Transforma filas del `ResultSet` en objetos Java.|
|**Spring Beans**|Objetos gestionados por el contenedor Spring para automatizar inyecciones.|
|**queryForObject / query**|Ejecutan SELECT y devuelven uno o varios registros.|

---

### ✅ En resumen:

El documento enseña paso a paso cómo estructurar una aplicación con **Spring JDBC Template**, aplicando buenas prácticas:

- Crear **DTOs** para representar entidades.
    
- Crear **DAOs** para centralizar las operaciones de base de datos.
    
- Configurar la inyección de dependencias con Spring.
    
- Implementar operaciones **CRUD** completas:  
    **Create, Read, Update, Delete**.
    
- Usar **RowMapper** para mapear resultados SQL a objetos Java.
    

El resultado final es una aplicación modular, mantenible y lista para entornos reales.