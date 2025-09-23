
---

En la introducción al contenedor de Spring aprendiste que este necesita **dos insumos principales**:

1. **Java Beans o clases Java** de la aplicación, de las cuales el contenedor creará los objetos.
2. **Archivo de configuración XML**, donde se indica cómo debe realizar la **inyección de dependencias** cuando crea estos objetos.

En esta lección se muestran ejemplos de **archivos de configuración**, que se crearán desde cero en las próximas secciones.

---

### Estructura del archivo de configuración XML

- El **elemento raíz** del archivo de configuración de Spring es **`<beans>`**, que **envuelve todos los demás elementos**.

- Dentro de `<beans>` se definen **espacios de nombres (namespaces)** y sus prefijos.
    
    - El **namespace predeterminado** se declara con el atributo `xmlns`, que representa **XML namespace**.
        
    - Los elementos **sin prefijo** pertenecen al namespace:
        
        `http://www.springframework.org/schema/beans`
        
    - Existen otros **namespaces y prefijos** como: `context`, `c`, `schema`, `util`, etc., que se aprenderán en secciones posteriores.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

	<bean name="emp" class="com.bharath.spring.springcore.Employee" p:id="20" p:name="Bharath" />

</beans>
```

---

### Definición de beans

- Para declarar un bean se usa el elemento **`<bean>`**.
- Spring **creará una instancia de la clase indicada** en el atributo del bean y se le asignará un **nombre**.
- Este elemento es el más utilizado para definir clases y objetos en Spring.

```xml
<bean name="emp" class="com.bharath.spring.springcore.Employee" p:id="20" p:name="Bharath" />
```

---

### Tipos de inyección en el XML

#### 1. **Constructor Injection**

- Si una clase requiere parámetros para su constructor (por ejemplo, la clase `Addition` con dos números),  
    se usa el subelemento:
    
    `<constructor-arg>`
    
- Aquí se especifican los valores de cada parámetro.
    
- El contenedor invoca el **constructor parametrizado** pasando esos valores.

```xml
<bean name="addition" class="com.bharath.spring.springcore.Addition">
	<constructor-arg value="20.3">
	<constructor-arg value="10">
</bean>
```

#### 2. **Setter Injection**

- En lugar de `<constructor-arg>`, se utilizan subelementos:
    
    `<property name="..." value="..."/>`
    
- Spring invoca los métodos **setter** correspondientes (por ejemplo, `setNumb1`, `setNumb2`) para inyectar los valores.

```xml
<bean class="com.bharath.spring.springcore.Addition" name="addition">
	<property value="20.3" name="num1"/>
	<property value="10" name="num2"/>
</bean>
```

---

### Resumen completo del documento

El **archivo de configuración de Spring** es un **.xml** que indica al contenedor **qué objetos crear y cómo inyectar sus dependencias**.

- El **elemento raíz** es `<beans>`, que declara los **namespaces** y envuelve la definición de todos los beans.
    
- Cada **`<bean>`** define una **clase específica** que Spring instanciará, asignándole un **nombre único**.
    
- Se pueden especificar las dependencias de dos maneras:
    
    1. **Constructor Injection:** mediante `<constructor-arg>`, el contenedor llama al constructor con parámetros de la clase.
        
    2. **Setter Injection:** mediante `<property>`, el contenedor invoca los métodos setter para establecer los valores.


En resumen, este archivo XML es el puente entre el **código Java** y el **contenedor de Spring**, permitiendo que Spring gestione de forma automática la **creación de objetos y la inyección de dependencias**.