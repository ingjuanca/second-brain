
---

### **1️⃣ Introducción**

Hasta este punto, se ha utilizado el **JdbcTemplate** para realizar operaciones CRUD (crear, leer, actualizar y eliminar) en la base de datos.  
En esta lección, se explica cómo **autoconfigurar (autowire)** el `JdbcTemplate` usando **anotaciones**, eliminando la necesidad de configurarlo manualmente en el archivo XML.

---

### **2️⃣ Situación inicial**

En la clase `EmployeeDAOImpl`, el `JdbcTemplate` se había inyectado mediante el archivo de configuración `config.xml`, donde se definía explícitamente el bean.

Ahora, el objetivo es **eliminar esa definición del bean** y permitir que Spring lo gestione automáticamente mediante anotaciones.

---

### **3️⃣ Pasos para usar anotaciones**

#### **Paso 1:** Marcar la clase DAO con `@Component`

En `EmployeeDAOImpl`, se añade la anotación:

```java
@Component 
public class EmployeeDAOImpl implements EmployeeDAO { ... }
```

Esta anotación le indica a Spring que puede **crear un objeto (bean)** de esta clase y administrarlo dentro del contenedor.  
En otras palabras, convierte la clase en un **bean gestionado por Spring**.

---

#### **Paso 2:** Autowired del `JdbcTemplate`

Dentro de `EmployeeDAOImpl`, se reemplaza la inyección manual del `JdbcTemplate` por una **inyección automática** usando la anotación:

```java
@Autowired 
private JdbcTemplate jdbcTemplate;
```

Esto le dice a Spring que debe **inyectar automáticamente** una instancia válida de `JdbcTemplate` sin que el desarrollador tenga que declararla manualmente en `config.xml`.

```java
package com.bharath.spring.springjdbc.employee.dao.impl;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;

import com.bharath.spring.springjdbc.employee.dao.EmployeeDao;
import com.bharath.spring.springjdbc.employee.dao.rowmapper.EmployeeRowMapper;
import com.bharath.spring.springjdbc.employee.dto.Employee;

@Component
public class EmployeeDaoImpl implements EmployeeDao {

	@Autowired
	private JdbcTemplate jdbcTemplate;

	@Override
	public int create(Employee employee) {
		String sql = "insert into employee values(?,?,?)";
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

---

#### **Paso 3:** Habilitar el escaneo de componentes**

En el archivo de configuración XML, se agrega la siguiente línea:

`<context:component-scan base-package="com.bharath.employee.dao.impl" />`

- Esta instrucción le indica al contenedor de Spring que **busque clases anotadas con `@Component`, `@Repository`, `@Service` o `@Controller`** dentro del paquete indicado y sus subpaquetes.
    
- Es importante escanear el **paquete de implementación (`impl`)**, no el de las interfaces (`dao`), ya que las interfaces no se pueden instanciar.
    

También se debe asegurar que las anotaciones estén habilitadas mediante:

`<context:annotation-config />`

---

### **4️⃣ Eliminación del bean manual**

En `config.xml`, se elimina la declaración manual del bean:

```xml
<bean id="employeeDAO" class="com.bharath.employee.dao.impl.EmployeeDAOImpl">    
	<property name="jdbcTemplate" ref="jdbcTemplate" /> 
</bean>
```

Ahora Spring gestionará automáticamente la creación del bean `EmployeeDAOImpl` y la inyección del `JdbcTemplate` gracias a las anotaciones.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p" xmlns:c="http://www.springframework.org/schema/c"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">

	<context:component-scan
		base-package="com.bharath.spring.springjdbc.employee.dao.impl" />

	<bean class="org.springframework.jdbc.datasource.DriverManagerDataSource"
		name="dataSource" p:driverClassName="com.mysql.jdbc.Driver" p:url="jdbc:mysql://localhost/mydb"
		p:username="root" p:password="test" />

	<bean class="org.springframework.jdbc.core.JdbcTemplate" name="jdbcTemplate"
		p:dataSource-ref="dataSource" />

</beans>
```

---

### **5️⃣ Ejecución del test y solución de error**

Después de realizar los cambios, al ejecutar la clase de prueba, ocurre el error:

`NoSuchBeanDefinitionException: No bean named 'employeeDAO' available`

**Causa:**  
Por defecto, Spring crea el nombre del bean basándose en el **nombre de la clase** con la primera letra en minúscula (`employeeDAOImpl`).  
Sin embargo, en el código de prueba se intenta obtener el bean con el nombre `"employeeDAO"`.

**Solución:**  
Asignar explícitamente un nombre al bean en la anotación `@Component`:

```java
@Component("employeeDAO") 
public class EmployeeDAOImpl implements EmployeeDAO { ... }
```

Esto garantiza que el bean tenga el mismo nombre que se usa en la prueba.

---

### **6️⃣ Verificación final**

Una vez corregido el nombre y vuelto a ejecutar el test:

- Spring escanea el paquete indicado.
    
- Detecta la clase anotada con `@Component("employeeDAO")`.
    
- Inyecta automáticamente el `JdbcTemplate` en la clase DAO.
    
- El test se ejecuta correctamente y las operaciones con la base de datos funcionan como antes.
    

---

### **7️⃣ Resumen del proceso**

|Paso|Acción|Descripción|
|---|---|---|
|1️⃣|**Agregar `@Component`**|Permite que Spring gestione la clase como un bean.|
|2️⃣|**Usar `@Autowired`**|Inyecta automáticamente el `JdbcTemplate`.|
|3️⃣|**Habilitar `component-scan`**|Spring busca las clases anotadas en el paquete indicado.|
|4️⃣|**Eliminar configuración XML manual**|Ya no es necesario definir el bean en `config.xml`.|
|5️⃣|**Dar nombre al bean (`@Component("employeeDAO")`)**|Evita errores de referencia en el test.|

---

### ✅ **Conclusión**

En esta lección se logró **autoconfigurar el `JdbcTemplate`** y eliminar completamente la dependencia de la configuración XML.  
Los puntos clave son:

- Marcar la clase DAO con `@Component`.
    
- Usar `@Autowired` para la inyección automática.
    
- Habilitar `component-scan` en la configuración XML.
    
- Asignar un nombre al bean para evitar errores.
    

Con esto, Spring:

- Detecta las clases automáticamente.
    
- Crea los objetos necesarios.
    
- Inyecta dependencias sin código adicional.
    

En resumen, el `JdbcTemplate` ahora se inyecta **automáticamente mediante anotaciones**, haciendo la aplicación más limpia, moderna y alineada con las buenas prácticas de **Spring Framework**. ✅