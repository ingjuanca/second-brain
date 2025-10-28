
---

## 🧩 **Título: Configuración basada en Java en Spring (sin XML)**

---

### 🔹 **1. Introducción**

A partir de **Spring 3.0**, es posible **configurar y usar Spring sin archivos XML**.  
La configuración se realiza completamente mediante **clases Java** marcadas con la anotación:

`@Configuration`

Esta anotación le indica a Spring que la clase contiene **definiciones de beans**.  
Cada método dentro de esta clase, marcado con `@Bean`, devuelve una **instancia de un bean**.

📘 **Ejemplo:**

```java
@Configuration 
public class SpringConfig {      
	@Bean     
	public Car car() {         
		return new Car();     
	} 
}
```

Aquí `car()` reemplaza lo que antes definíamos en un `<bean>` de un XML.

📍 Se pueden crear **múltiples clases de configuración** y combinarlas usando la anotación:

`@Import(OtherConfig.class)`

Para cargar esta configuración, se utiliza el contenedor:

`AnnotationConfigApplicationContext`

en lugar del tradicional `ClassPathXmlApplicationContext`.

---

## 🧱 **2. Caso de uso: DAO y Servicio**

El ejemplo práctico consiste en implementar una pequeña aplicación con:

- Una clase **DAO** (acceso a datos, sin base de datos real).
    
- Una clase **Service** que usa al DAO.
    
- Una clase de **configuración Java**.
    
- Una clase **Test** que recupera los beans desde el contenedor.
    

---

## 🔹 **3. Creación del proyecto Maven**

### Pasos:

1. En **Eclipse** → `File → New → Maven Project`.
    
2. Selecciona el arquetipo: **QuickStart**.
    
3. Nombra el proyecto: `springJavaConfig`.
    
4. Elimina la carpeta de test `src/test/java` y la clase `App.java`.
    

### Editar `pom.xml`:

- Copia las propiedades y dependencias desde un proyecto de Spring Core existente.
    
- Asegúrate de incluir:
    

```xml
<dependency>     
	<groupId>org.springframework</groupId>     
	<artifactId>spring-core</artifactId> 
</dependency> 
<dependency>     
	<groupId>org.springframework</groupId>     
	<artifactId>spring-context</artifactId> 
</dependency>
```

Esto basta para aplicaciones standalone con configuración Java.

⚙️ Luego:  
`Right click → Maven → Update Project`

---

## 🔹 **4. Creación del DAO y configuración Java**

### DAO:

```java
@Component 
public class Dao {    
	public void create() {         
		System.out.println("Created (simulado)");     
	} 
}
```

> `@Component` permite que Spring detecte automáticamente la clase como un bean.

### Configuración Java:

```java
@Configuration 
public class SpringConfig {      
	@Bean     
	public Dao dao() {         
		return new Dao();     
	} 
}
```

- `@Configuration` indica que esta clase define beans.
    
- `@Bean` equivale a `<bean>` en XML.
    

---

## 🔹 **5. Creación de la clase Test**

Usa el contenedor **AnnotationConfigApplicationContext** para leer las configuraciones.

```java
public class Test {     
	public static void main(String[] args) {         
		AnnotationConfigApplicationContext context =             new AnnotationConfigApplicationContext(SpringConfig.class);          
		Dao dao = context.getBean(Dao.class);         
		dao.create();          context.close();     
	} 
}
```

📘 **Flujo:**

- Spring carga la clase de configuración.
    
- Crea el bean `Dao`.
    
- Lo obtiene mediante `getBean()`.
    
- Invoca su método `create()`.
    

✅ **Salida esperada:**

`Created (simulado)`

---

## 🔹 **6. Creación de la clase Service**

### Service:

```java
public class Service {      
	
	@Autowired     
	private Dao dao;      
	
	public void save() {         
		dao.create();     
	}      
	
	public void init() {         
		System.out.println("init()");     
	}      
	
	public void destroy() {         
		System.out.println("destroy()");     
	} 
}
```

- `@Autowired` inyecta el bean `Dao` automáticamente.
    
- Los métodos `init()` y `destroy()` se usarán más adelante para el ciclo de vida del bean.
    

### Actualiza la configuración:

```java
@Configuration 
public class SpringConfig {      
	@Bean     
	public Dao dao() {         
		return new Dao();     
	}      
	
	@Bean(initMethod = "init", destroyMethod = "destroy")     
	public Service service() {         
		return new Service();     
	} 
}
```

- `initMethod` y `destroyMethod` se llaman **automáticamente** cuando el contenedor arranca y se cierra.
    

---

## 🔹 **7. Probar el servicio**

Modifica la clase Test:

`public class Test {     public static void main(String[] args) {         AnnotationConfigApplicationContext context =             new AnnotationConfigApplicationContext(SpringConfig.class);          Service service = context.getBean(Service.class);         service.save();          context.close();     } }`

✅ **Salida esperada:**

`init() Created (simulado) destroy()`

📘 **Explicación del flujo:**

1. Spring inicializa el contenedor y llama a `init()`.
    
2. Crea los beans `Dao` y `Service`.
    
3. Inyecta el `Dao` dentro del `Service` usando `@Autowired`.
    
4. Ejecuta `service.save()`, que a su vez llama a `dao.create()`.
    
5. Al cerrar el contexto, se invoca automáticamente `destroy()`.
    

---

## 🔹 **8. Importar múltiples configuraciones**

En aplicaciones reales, las configuraciones suelen separarse en varios archivos.

Ejemplo:

### `DaoConfig.java`

`@Configuration public class DaoConfig {      @Bean     public Dao dao() {         return new Dao();     } }`

### `SpringConfig.java`

`@Configuration @Import(DaoConfig.class) public class SpringConfig {      @Bean     public Service service() {         return new Service();     } }`

- `@Import(DaoConfig.class)` permite **combinar configuraciones**.
    
- Los beans definidos en `DaoConfig` se pueden usar en `SpringConfig`.
    

📘 **Ventaja:**  
Permite dividir la configuración por capas (DAO, Service, Web, etc.) y mantener el código modular.

---

## 🔹 **9. Ciclo de vida de un Bean**

Con configuración Java, los métodos de inicialización y destrucción se definen igual que en XML, pero usando atributos en la anotación `@Bean`.

`@Bean(initMethod = "init", destroyMethod = "destroy") public Service service() {     return new Service(); }`

📘 **Pasos:**

1. Cuando se inicializa el contenedor, se ejecuta el método `init()`.
    
2. Cuando se cierra (`context.close()`), se llama automáticamente a `destroy()`.
    

---

## 🧾 **Resumen general**

|Concepto|Descripción|
|---|---|
|**@Configuration**|Define una clase de configuración de Spring (reemplaza los XML).|
|**@Bean**|Define un método que devuelve un objeto gestionado por Spring.|
|**@Component / @Autowired**|Permiten inyección automática de dependencias.|
|**AnnotationConfigApplicationContext**|Carga la configuración Java en lugar de XML.|
|**@Import**|Permite combinar múltiples clases de configuración.|
|**initMethod / destroyMethod**|Definen métodos del ciclo de vida del bean.|

---

## ✅ **Conclusión final**

Con la configuración basada en Java, **Spring elimina completamente la necesidad de XML**, permitiendo un desarrollo más limpio y orientado a código.  
Todo lo que antes se hacía en `applicationContext.xml` puede hacerse con clases y anotaciones:

- Los **beans** se crean con `@Bean`.
    
- Las **dependencias** se inyectan con `@Autowired`.
    
- Las **configuraciones** se combinan con `@Import`.
    
- Los **ciclos de vida** se gestionan con `initMethod` y `destroyMethod`.
    

> 💡 En resumen:  
> Spring Java Config simplifica el desarrollo, mejora la legibilidad y hace que las aplicaciones sean completamente **modulares, mantenibles y sin XML**.