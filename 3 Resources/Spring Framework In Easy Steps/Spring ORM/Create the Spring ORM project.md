
---

## 🧩 **Título: Creación de un proyecto Spring ORM con Hibernate**

---

### 🧱 **1. Creación del proyecto Maven**

- En **Eclipse**, se crea un nuevo proyecto Maven:  
    `File → New → Maven Project → maven-archetype-quickstart`.
    
- **GroupId:** `com.bharath.spring`
    
- **ArtifactId:** `springorm`
    
- Se abre el archivo `pom.xml` y se copian las propiedades y dependencias del proyecto anterior (`springjdbc`).
    
- Se eliminan las dependencias innecesarias como `spring-jdbc`, y se agregan:
    
    - `spring-core`
    - `spring-orm`
    - `hibernate-core`
    - `mysql-connector-java`

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
</dependencies>
```

- Se elimina la carpeta `src/test/java` y se actualiza el proyecto Maven (`Maven → Update Project`).
    
- Asegúrate de usar **Java 1.8** en el plugin del compilador.
    

> ⚠️ _Nota:_ Si usas una versión reciente de Hibernate, elimina la etiqueta `<type>` del `pom.xml` para evitar errores de dialecto MySQL.

---

### 🧩 **2. Creación de la entidad `Product`**

- La clase **Product** representa una tabla `product` en la base de datos.
    
- Campos:
    
```java 
private int id; 
private String name; 
private String desc; 
private double price;
```
    
- Se agregan los métodos `getters` y `setters`.
    
- Se anotan con **JPA**:
    
```java
@Entity 
@Table(name="product") 
public class Product {     
	@Id     
	@Column
	(name="id")     
	... 
}
```
    
- `@Entity` indica que la clase es una entidad.
    
- `@Table` especifica la tabla.
    
- `@Column` vincula cada propiedad con su columna en la base de datos.

```java
package com.bharath.spring.springorm.product.entity;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name = "product")
public class Product {
	@Id
	@Column(name = "id")
	private int id;
	@Column(name = "name")
	private String name;
	@Column(name = "description")
	private String desc;
	@Column(name = "price")
	private double price;
	
	@Override
	public String toString() {
		return "Product [id=" + id + ", name=" + name + ", desc=" + desc + ", price=" + price + "]";
	}
	// Add the getters and setters
}
```


> ✅ Con esto, la clase Java se convierte en una **entidad persistente**.

---

### 🧩 **3. Creación del DAO**

- Se crea una interfaz `ProductDao` dentro de `springorm.product.dao`:
    
```java
package com.bharath.spring.springorm.product.dao;

import com.bharath.spring.springorm.product.entity.Product;

public interface ProductDao {
	
	int create(Product product);	
	
}
```
    
- Se crea la implementación `ProductDaoImpl` en `springorm.product.dao.impl`:
    
```java
package com.bharath.spring.springorm.product.dao.impl;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.orm.hibernate5.HibernateTemplate;
import org.springframework.stereotype.Component;
import org.springframework.transaction.annotation.Transactional;

import com.bharath.spring.springorm.product.dao.ProductDao;
import com.bharath.spring.springorm.product.entity.Product;

@Component("productDao")
public class ProductDaoImpl implements ProductDao {
	@Autowired
	HibernateTemplate hibernateTemplate;
	@Override
	@Transactional
	public int create(Product product) {
		Integer result = (Integer) hibernateTemplate.save(product);
		return result;
	}
}

```
    
- Esta clase usa `HibernateTemplate` (de `org.springframework.orm.hibernate5`) para interactuar con la base de datos.
    

---

### ⚙️ **4. Configuración de Spring**

#### **4.1 Archivo de configuración `config.xml`**

- Se parte del archivo `config.xml` del proyecto anterior (`springjdbc`) y se ajusta.
    
- Se mantiene el bean `DriverManagerDataSource` para la conexión.
    
- Se elimina el `JdbcTemplate`.

---

#### **4.2 Configurar el `SessionFactory`**

- Se agrega el bean `LocalSessionFactoryBean`:
    
```xml
<bean id="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean" p:dataSource-ref="dataSource">     
	<property name="hibernateProperties">         
	<props>             
		<prop key="hibernate.dialect">org.hibernate.dialect.MySQLDialect</prop>          <prop key="hibernate.show_sql">true</prop>         
	</props>     
	</property>     
	<property name="annotatedClasses">         
		<list>
			<value>com.bharath.springorm.product.entity.Product</value>        
		</list>     
	</property> 
</bean>
```
    

> 🔹 Este bean:
> 
> - Define el dialecto SQL (MySQL).
>     
> - Muestra las consultas generadas en consola.
>     
> - Registra las clases anotadas (entidades).
>     

---

#### **4.3 Configurar `HibernateTemplate`**

```xml
<bean id="hibernateTemplate" class="org.springframework.orm.hibernate5.HibernateTemplate"       
p:sessionFactory-ref="sessionFactory"/>
```

Y se agrega:

```xml
<context:component-scan base-package="com.bharath.springorm.product.dao.impl"/>
```

Esto permite que Spring detecte e inyecte automáticamente los componentes DAO.

---

#### **4.4 Configurar el `TransactionManager`**

```xml
<bean id="transactionManager"       class="org.springframework.orm.hibernate5.HibernateTransactionManager"       p:sessionFactory-ref="sessionFactory"/>
```

Y se habilita el soporte transaccional:

```xml
<tx:annotation-driven/>
```

> 🔹 Con esto, se pueden usar anotaciones como `@Transactional` para manejar transacciones.

Versión final del archivo de configuración xml:

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
    
    <tx:annotation-driven/>
    
	<context:component-scan base-package="com.bharath.spring.springorm.product.dao.impl" />
	
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
				<value>com.bharath.spring.springorm.product.entity.Product</value>
			</list>
		</property>
	</bean>
	
	<bean class="org.springframework.orm.hibernate5.HibernateTemplate"
		name="hibernateTemplate" p:sessionFactory-ref="sessionFactory" />
		
	<bean class="org.springframework.orm.hibernate5.HibernateTransactionManager"
		name="transactionManager" p:sessionFactory-ref="sessionFactory" />
		
</beans>
```

---

### 🧩 **5. Implementar el método `create`**

En `ProductDaoImpl`:

```java
@Transactional 
@Override 
public Integer create(Product product) {     
	Integer result = (Integer) hibernateTemplate.save(product);     
	return result; }
```

> ✅ `@Transactional` indica que el método se ejecuta dentro de una transacción.  
> Si ocurre un error, Spring hace **rollback** automáticamente.

---

### 🧠 **6. Manejo de errores (Troubleshooting)**

Si surgen errores como:

- **UnsatisfiedDependencyException**
    
- **Error creating bean with name ‘sessionFactory’**
    

Entonces:

1. Eliminar la carpeta `.m2/repository` y reconstruir el proyecto.
    
2. Asegurarse de usar las versiones correctas de dependencias (`hibernate-core 5.4.10.Final`).
    
3. Revisar los imports: deben ser de `org.springframework.orm.hibernate5`.
    

---

### 🧪 **7. Crear clase de prueba y ejecutar**

Se crea una clase `Test` con un `main`:

```java
package com.bharath.spring.springorm.product.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import com.bharath.spring.springorm.product.dao.ProductDao;
import com.bharath.spring.springorm.product.entity.Product;

public class Test {

	public static void main(String[] args) {

		ApplicationContext context = new ClassPathXmlApplicationContext(
				"com/bharath/spring/springorm/product/test/config.xml");
		ProductDao productDao = (ProductDao) context.getBean("productDao");
		Product product = new Product(); product.setId(1);
		product.setName("Iphone"); 
		product.setDesc("Its awesome!!");
		product.setPrice(800); 
		productDao.create(product);

	}

}

```

Al ejecutarlo:

- Hibernate genera automáticamente la sentencia `INSERT`.
    
- Se valida que los datos se hayan insertado en la tabla `product`.
    

---

## ✅ **Resumen general**

|Etapa|Descripción|
|---|---|
|**Proyecto Maven**|Creación del proyecto con dependencias de Spring ORM y Hibernate.|
|**Entidad `Product`**|Clase anotada con JPA (`@Entity`, `@Table`, `@Column`).|
|**DAO**|Interfaz + implementación con `HibernateTemplate`.|
|**Configuración Spring**|Beans para `DataSource`, `SessionFactory`, `HibernateTemplate`, `TransactionManager`.|
|**Transacciones**|Se habilitan con `@Transactional` y `<tx:annotation-driven>`.|
|**Prueba final**|Inserta un registro en la base de datos usando Hibernate.|

---

### 🏁 **Conclusión**

Este proceso muestra cómo **Spring ORM + Hibernate** automatizan:

- La conexión a base de datos.
    
- El mapeo objeto-relacional (JPA).
    
- La gestión de transacciones.
    
- La eliminación del código repetitivo JDBC.
    

El resultado: un proyecto limpio, modular y listo para escalar, con una clara separación entre **entidades**, **DAO**, y **configuración de infraestructura**.