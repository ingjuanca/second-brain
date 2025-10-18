
---

## 🧩 **Título: Implementación de operaciones CRUD en HibernateTemplate**

---

### 🔹 **1. Actualizar registros (update method)**

**Objetivo:**  
Actualizar la información de un producto existente en la base de datos (por ejemplo, cambiar el precio de 700 $ a 800 $).

#### **Pasos:**

1. En la interfaz `ProductDao`, agregar el método:
    
```java
package com.bharath.spring.springorm.product.dao;

import com.bharath.spring.springorm.product.entity.Product;

public interface ProductDao {
	int create(Product product);
	
	void update(Product product);	
}
```
    
2. En `ProductDaoImpl`, implementar el método:
    
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

	@Override
	@Transactional
	public void update(Product product) {
		hibernateTemplate.update(product);

}

```
    
3. En la clase `Test.java`:
    
    - Comentar el bloque de código donde se crea un nuevo producto.
        
    - Modificar los datos del objeto `product`, por ejemplo:
        
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
		productDao.update(product);
	}
}
```
        
    - Ejecutar el test.
        
4. Hibernate genera automáticamente la sentencia SQL:
    
    `update product set name=?, description=?, price=? where id=?`
    
    Esto ocurre porque el campo `id` en la clase `Product` está anotado con `@Id`, y Hibernate lo usa en la cláusula `WHERE`.
    
5. Al ejecutar `SELECT * FROM product;`, se confirma que el precio cambió a **800 $**.
    

> ✅ Con `hibernateTemplate.update()`, Hibernate identifica el registro por su **id** y actualiza los campos modificados.

---

### 🔹 **2. Eliminar registros (delete method)**

**Objetivo:**  
Eliminar un producto de la base de datos según su identificador.

#### **Pasos:**

1. En la interfaz `ProductDao`:
    
```java
package com.bharath.spring.springorm.product.dao;

import com.bharath.spring.springorm.product.entity.Product;

public interface ProductDao {
	int create(Product product);
	
	void update(Product product);	
	
	void delete(Product product);
}
```
    
2. En `ProductDaoImpl`:
    
    `@Transactional @Override public void delete(Product product) {     hibernateTemplate.delete(product); }`

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

	@Override
	@Transactional
	public void update(Product product) {
		hibernateTemplate.update(product);

	@Override 
	@Transactional 	
	public void delete(Product product) {     
		hibernateTemplate.delete(product); 
	}
	
}

```

2. En `Test.java`:
    
    - Comentar el bloque del método `update()`.
        
    - Llamar a:
        
        `productDao.delete(product);`

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
		Product product = new Product(); 
		product.setId(1);
		product.setName("Iphone"); 
		product.setDesc("Its awesome!!");
		product.setPrice(800); 
		productDao.delete(product);
	}
}
```

2. Hibernate ejecuta internamente:
    
    - Un `SELECT` para verificar que el registro exista.
        
    - Luego ejecuta un `DELETE` para eliminarlo.
        
3. Al ejecutar `SELECT * FROM product;`, la tabla queda vacía.
    

> ✅ Con `hibernateTemplate.delete()`, basta con pasar un objeto que contenga solo el campo `id`. Hibernate hace el resto.

---

### 🔹 **3. Consultar un registro individual (find method)**

**Objetivo:**  
Obtener un producto específico de la base de datos según su **id**.

#### **Pasos:**

1. Insertar manualmente algunos registros en la base de datos:
    
```sql
INSERT INTO product VALUES (1, 'iPhone', 'Good', 800); 
INSERT INTO product VALUES (2, 'MacBook', 'Good', 1400);
```
    
2. En `ProductDao`:
    
    `Product find(int id);`

```java
package com.bharath.spring.springorm.product.dao;

import com.bharath.spring.springorm.product.entity.Product;

public interface ProductDao {
	int create(Product product);
	
	void update(Product product);	
	
	void delete(Product product);
	
	Product find(int id);
}
```

2. En `ProductDaoImpl`:
    
    `@Override public Product find(int id) {     Product product = hibernateTemplate.get(Product.class, id);     return product; }`

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

	@Override
	@Transactional
	public void update(Product product) {
		hibernateTemplate.update(product);

	@Override 
	@Transactional 	
	public void delete(Product product) {     
		hibernateTemplate.delete(product); 
	}
	
	@Override 
	public Product find(int id) {     
		Product product = hibernateTemplate.get(Product.class, id);     
		return product; 
	}
}

```

2. En `Test.java`:
    
    `Product product = productDao.find(1); System.out.println(product);`

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
		Product product = productDao.find(1); 
		System.out.println(product);
	}
}
```

2. Hibernate genera automáticamente:
    
    `select * from product where id=1;`
    
    y devuelve un objeto `Product`.
    

> 💡 Si la clase `Product` no tiene un método `toString()`, no mostrará los datos legibles; es recomendable generarlo automáticamente desde Eclipse (`Source → Generate toString()`).

**Resultado final:**

`Product [id=1, name=iPhone, description=Good, price=800.0]`

> ✅ `hibernateTemplate.get()` devuelve una sola entidad usando la clave primaria.

---

### 🔹 **4. Consultar todos los registros (findAll method)**

**Objetivo:**  
Obtener una lista de todos los productos almacenados en la base de datos.

#### **Pasos:**

1. En la interfaz `ProductDao`:
    
    `List<Product> findAll();`

```java
package com.bharath.spring.springorm.product.dao;

import java.util.List;
import com.bharath.spring.springorm.product.entity.Product;

public interface ProductDao {
	int create(Product product);
	
	void update(Product product);	
	
	void delete(Product product);
	
	Product find(int id);
	
	List<Product> findAll();
}
```

1. En `ProductDaoImpl`:
    
    `@Override public List<Product> findAll() {     return hibernateTemplate.loadAll(Product.class); }`

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

	@Override
	@Transactional
	public void update(Product product) {
		hibernateTemplate.update(product);

	@Override 
	@Transactional 	
	public void delete(Product product) {     
		hibernateTemplate.delete(product); 
	}
	
	@Override 
	public Product find(int id) {     
		Product product = hibernateTemplate.get(Product.class, id);     
		return product; 
	}
	
	@Override 
	public List<Product> findAll() {     
		return hibernateTemplate.loadAll(Product.class); 
	}
}

```

1. En `Test.java`:
    
    `List<Product> products = productDao.findAll(); for (Product p : products) {     System.out.println(p); }`

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
		List<Product> products = productDao.findAll(); 
		System.out.println(products);
	}
}
```

1. Hibernate ejecuta automáticamente una consulta:
    
    `select * from product;`
    
    y convierte cada fila en un objeto `Product`.
    

**Resultado en consola:**

`Product [id=1, name=iPhone, description=Good, price=800.0] Product [id=2, name=MacBook, description=Good, price=1400.0]`

> ✅ `hibernateTemplate.loadAll()` devuelve una lista con todas las entidades del tipo especificado.

---

## 🧠 **Resumen técnico**

|Método|Descripción|HibernateTemplate usado|SQL generado|Requiere @Transactional|
|---|---|---|---|---|
|**create()**|Inserta un nuevo registro|`save()`|`INSERT INTO ...`|✅|
|**update()**|Modifica un registro existente|`update()`|`UPDATE ... WHERE id=?`|✅|
|**delete()**|Elimina un registro existente|`delete()`|`DELETE FROM ... WHERE id=?`|✅|
|**find()**|Recupera un registro por id|`get()`|`SELECT * FROM ... WHERE id=?`|❌|
|**findAll()**|Recupera todos los registros|`loadAll()`|`SELECT * FROM ...`|❌|

---

## ✅ **Conclusión**

En esta sección se completó la implementación de todas las operaciones CRUD usando **Spring ORM** con **HibernateTemplate**, logrando un flujo simple y eficiente:

- `save()` → Crear
- `update()` → Actualizar
- `delete()` → Eliminar
- `get()` → Leer uno
- `loadAll()` → Leer todos


Spring, junto con Hibernate, se encarga de:

- Generar las consultas SQL.
    
- Manejar las transacciones.
    
- Convertir filas SQL en objetos Java.
    
- Asegurar consistencia de datos mediante `@Transactional`.
    

> En resumen, HibernateTemplate convierte el manejo de base de datos en una operación orientada a objetos, eliminando completamente el código SQL manual.