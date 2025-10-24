
---

El documento explica paso a paso cómo desarrollar un **caso de uso de registro de usuarios** en una aplicación **Spring MVC + ORM (Hibernate)**, abarcando desde la configuración inicial hasta el despliegue completo y pruebas.

### 1. **Pasos generales del desarrollo**

Se implementa un flujo completo con:

- **Spring MVC** para la capa de presentación (controladores y vistas JSP).
    
- **Spring ORM + Hibernate** para la capa de persistencia.
    
- **Maven** para la gestión del proyecto y dependencias.
    

### 2. **Configuración inicial**

1. **Crear un proyecto Maven** en Eclipse usando el arquetipo `maven-archetype-webapp`.
    
2. Agregar dependencias en el `pom.xml`:
    
    - `spring-webmvc`
    - `spring-orm`
    - `hibernate-core`
    - `mysql-connector-java`
    - jstl

```xml
<properties>
	<springframework.version>4.3.6.RELEASE</springframework.version>
</properties>
<dependencies>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-webmvc</artifactId>
		<version>${springframework.version}</version>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-orm</artifactId>
		<version>${springframework.version}</version>
	</dependency>
	<dependency>
		<groupId>org.hibernate</groupId>
		<artifactId>hibernate-core</artifactId>
		<version>5.2.5.Final</version>
	</dependency>
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
		<version>5.1.6</version>
	</dependency>
	<dependency>
		<groupId>jstl</groupId>
		<artifactId>jstl</artifactId>
		<version>1.2</version>
	</dependency>
	<dependency>
		<groupId>taglibs</groupId>
		<artifactId>standard</artifactId>
		<version>1.1.2</version>
	</dependency>
</dependencies>
```
3. **Configurar Tomcat** como servidor de ejecución.
    
4. **Actualizar el archivo `web.xml`** agregando el `DispatcherServlet`, que actúa como **Front Controller** de Spring MVC.
    
```xml
<web-app xmlns="http://java.sun.com/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
          http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
	version="3.0">
	<display-name>Archetype Created Web Application</display-name>

	<servlet>
		<servlet-name>dispatcher</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>dispatcher</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
</web-app>
```
### 3. **Archivo de configuración de Spring**

- Crear `dispatcher-servlet.xml` para definir:
    
    - `DataSource` (conexión a base de datos MySQL)
        
    - `SessionFactory` (manejo de sesiones de Hibernate)
        
    - `HibernateTemplate` (operaciones CRUD simplificadas)
        
    - `ViewResolver` (mapeo de vistas JSP)
    
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p" xmlns:c="http://www.springframework.org/schema/c"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd
    http://www.springframework.org/schema/tx
    http://www.springframework.org/schema/tx/spring-tx.xsd">
    
	<context:component-scan base-package="com.bharath.spring.springmvcorm.user" />
	
	<tx:annotation-driven />
	
	<bean class="org.springframework.jdbc.datasource.DriverManagerDataSource"
		name="dataSource" p:driverClassName="com.mysql.jdbc.Driver" p:url="jdbc:mysql://localhost/mydb"
		p:username="root" p:password="test" />

	<bean class="org.springframework.orm.hibernate5.LocalSessionFactoryBean"
		name="sessionFactory" p:dataSource-ref="dataSource">
		<property name="hibernateProperties">
			<props>
				<prop key="hibernate.dialect">org.hibernate.dialect.MySQLDialect</prop>
				<prop key="hibernate.show_sql">true</prop>
			</props>
		</property>
		<property name="annotatedClasses">
			<list>
				<value>com.bharath.spring.springmvcorm.user.entity.User</value>
			</list>
		</property>
	</bean>
	
	<bean class="org.springframework.orm.hibernate5.HibernateTemplate"
		name="hibernateTemplate" p:sessionFactory-ref="sessionFactory" />
		
	<bean class="org.springframework.orm.hibernate5.HibernateTransactionManager"
		name="transactionManager" p:sessionFactory-ref="sessionFactory" />
		
	<bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver"
		name="viewResolver">
		<property name="prefix">
			<value>/WEB-INF/jsps/</value>
		</property>
		<property name="suffix">
			<value>.jsp</value>
		</property>
	</bean>
	
</beans>
```
### 4. **Modelo de datos**

- Crear la clase `User` con campos `id`, `name`, y `email`.
    
- Anotar con:
```java
    @Entity
    @Table(name="user")
    @Id
```

para que Hibernate la mapee a la tabla `user`.

```java
package com.bharath.spring.springmvcorm.user.entity;

import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name="user")
public class User implements Comparable<User>{

	@Override
	public String toString() {
		return "User [id=" + id + ", name=" + name + ", email=" + email + "]";
	}
	
	@Id
	private Integer id;
	private String name;
	private String email;
	public int getId() {
		return id;
	}
	
	public void setId(int id) {
		this.id = id;
	}
	
	public String getName() {
		return name;
	}
	
	public void setName(String name) {
		this.name = name;
	}
	
	public String getEmail() {
		return email;
	}
	
	public void setEmail(String email) {
		this.email = email;
	}
	
	@Override
	public int compareTo(User user) {
		return this.id.compareTo(user.id);
	}
	
}
```
    
### 5. **Capas del proyecto**

El sistema sigue una arquitectura por capas:

|Capa|Elemento principal|Descripción|
|---|---|---|
|**Entidad / Modelo**|`User`|Representa la tabla de base de datos|
|**DAO (Data Access Object)**|`UserDao` y `UserDaoImpl`|Gestiona operaciones con HibernateTemplate|
|**Servicio**|`UserService` y `UserServiceImpl`|Contiene la lógica de negocio, usa DAO|
|**Controlador**|`UserController`|Maneja las peticiones HTTP y llama a los servicios|

Todas las clases se anotan con:

- `@Repository` para DAO

```java
package com.bharath.spring.springmvcorm.user.dao;

import java.util.List;

import com.bharath.spring.springmvcorm.user.entity.User;

public interface UserDao {
	int create(User user);	
	List<User> findUsers();	
	User findUser(Integer id);
}

```

```java
package com.bharath.spring.springmvcorm.user.dao.impl;

import java.io.Serializable;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.orm.hibernate5.HibernateTemplate;
import org.springframework.stereotype.Repository;

import com.bharath.spring.springmvcorm.user.dao.UserDao;
import com.bharath.spring.springmvcorm.user.entity.User;

@Repository
public class UserDaoImpl implements UserDao {

	@Autowired
	private HibernateTemplate hibernateTemplate;

	public HibernateTemplate getHibernateTemplate() {
		return hibernateTemplate;
	}

	public void setHibernateTemplate(HibernateTemplate hibernateTemplate) {
		this.hibernateTemplate = hibernateTemplate;
	}

	@Override
	public int create(User user) {
		Integer result = (Integer) hibernateTemplate.save(user);
		return result;
	}

	@Override
	public List<User> findUsers() {
		return hibernateTemplate.loadAll(User.class);
	}

	@Override
	public User findUser(Integer id) {
		return hibernateTemplate.get(User.class, id);
	}

}

```

- `@Service` para servicios

```java
package com.bharath.spring.springmvcorm.user.services;

import java.util.List;

import com.bharath.spring.springmvcorm.user.entity.User;

public interface UserService {
	
	int save(User user);

	List<User> getUsers();
	
	User getUser(Integer id);
}

```

```java
package com.bharath.spring.springmvcorm.user.services;

import java.util.Collections;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import com.bharath.spring.springmvcorm.user.dao.UserDao;
import com.bharath.spring.springmvcorm.user.entity.User;

@Service
public class UserServiceImpl implements UserService {

	@Autowired
	private UserDao dao;

	public UserDao getDao() {
		return dao;
	}

	public void setDao(UserDao dao) {
		this.dao = dao;
	}

	@Override
	@Transactional
	public int save(User user) {
		// Business Logic
		return dao.create(user);
	}

	@Override
	public List<User> getUsers() {
		List<User> users = dao.findUsers();
		Collections.sort(users);
		return users;
	}

	@Override
	public User getUser(Integer id) {
		return dao.findUser(id);
	}
}

```
- `@Controller` para controladores

```java
package com.bharath.spring.springmvcorm.user.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

import com.bharath.spring.springmvcorm.user.entity.User;
import com.bharath.spring.springmvcorm.user.services.UserService;

@Controller
public class UserController {

	@Autowired
	private UserService service;

	@RequestMapping("registrationPage")
	public String showRegistrationPage() {
		return "userReg";
	}

	@RequestMapping(value = "registerUser", method = RequestMethod.POST)
	public String registerUser(@ModelAttribute("user") User user, ModelMap model) {
		int result = service.save(user);
		model.addAttribute("result", "User Created With Id" + result);

		return "userReg";
	}

	@RequestMapping("getUsers")
	public String getUser(ModelMap model) {
		List<User> users = service.getUsers();
		model.addAttribute("users", users);
		return "displayUsers";
	}

	public UserService getService() {
		return service;
	}

	public void setService(UserService service) {
		this.service = service;
	}

}

```

- `@Autowired` para inyección automática de dependencias
    

### 6. **Flujo funcional**

1. **El usuario abre la página de registro (`registrationPage.jsp`)**.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<form action="registerUser" method="post">
		<pre>
Id: <input type="text" name="id"/>
Name: <input type="text" name="name" />
Email: <input type="text" name="email" />
<input type="submit" name="register" />
</pre>
	</form>
	<br />${result}
</body>
</html>
```

2. Al enviar el formulario, se ejecuta el método `registerUser()` del `UserController`.
    
3. Este método recibe el modelo `User` y llama al servicio para guardar el usuario.
    
4. `UserServiceImpl` llama a `UserDaoImpl`, que usa `HibernateTemplate.save(user)` para guardar el registro.
    
5. El ID generado se muestra en la misma página como mensaje de confirmación.
    

### 7. **Visualización de datos**

- Se implementa una funcionalidad para listar todos los usuarios:
    
    - En `UserDaoImpl`: método `findUsers()` usa `hibernateTemplate.loadAll(User.class)`.
        
    - En `UserServiceImpl`: método `getUsers()`.
        
    - En `UserController`: método `getUsers()` devuelve la vista `displayUsers.jsp`.
        
- En `displayUsers.jsp` se usa **JSTL** (`<c:forEach>`) para recorrer la lista y mostrar los usuarios en una tabla HTML.
    

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<table border="1">
		<tr>
			<th>id</th>
			<th>name</th>
			<th>email</th>
		</tr>
		<c:forEach items="${users}" var="user">
			<tr>
				<td>${user.id}</td>
				<td>${user.name}</td>
				<td>${user.email}</td>
			</tr>
		</c:forEach>
	</table>
</body>
</html>
```
### 8. **Mejoras**

- Se habilita `@ComponentScan` en el XML para detectar automáticamente las clases anotadas.
    
- Se ajustan los mapeos de entidades y paquetes.
    
- Se elimina el DTD antiguo en `web.xml` para evitar errores con EL (Expression Language).
    
- Se agrega ordenamiento por `id` implementando `Comparable<User>` en la clase `User`.
    

### 9. **Prueba final**

- Se ejecuta la aplicación en Tomcat.
    
- Desde el navegador:
    
    - `/registrationPage` → formulario de registro.
        
    - `/getUsers` → lista de usuarios registrados ordenados por ID.
        

El flujo completo demuestra **Spring MVC + Hibernate + JSP + JSTL en acción**, mostrando cómo construir una aplicación de registro con persistencia y vistas dinámicas.

---

## 🧾 **Resumen final**

El documento enseña cómo construir paso a paso un **módulo completo de registro de usuarios** usando **Spring MVC y Hibernate**, incluyendo:

- Configuración de Maven, Tomcat y dependencias.
    
- Creación de capas (modelo, DAO, servicio, controlador).
    
- Inyección de dependencias con anotaciones de Spring.
    
- Persistencia con HibernateTemplate.
    
- Vistas JSP con JSTL y Expression Language.
    
- Ordenamiento de resultados e integración completa entre capas.
    

> En resumen, es una guía práctica para entender cómo implementar una arquitectura **MVC + ORM** en Java usando **Spring Framework**, desde la base de datos hasta la interfaz web.

---

¿Quieres que te organice este resumen en formato **guía paso a paso con comandos y ejemplos de código** (por ejemplo, para usarlo como material de estudio o práctica)?