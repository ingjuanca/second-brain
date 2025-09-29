
---

## Externalización de Beans: lectura de propiedades desde un archivo

En esta lección se explica un concepto avanzado de **Spring Core** llamado **externalización de beans** o **lectura de propiedades desde un archivo** para inyectarlas en un bean.

En aplicaciones reales es común tener **valores de configuración** (por ejemplo: nombre de base de datos, puerto del servidor, usuario, contraseña) que **no conviene dejar “hardcodeados”** en el código.  
En su lugar se usan **archivos `.properties`**, lo que permite:

- Cambiar los valores en **un solo lugar** sin recompilar la aplicación.
    
- Reutilizarlos en diferentes partes del proyecto.
    

---

### Pasos para usar propiedades externas en Spring

Spring utiliza la característica **Property Placeholder** para enlazar un archivo de propiedades con la configuración XML en **tres pasos**:

---

### 1️⃣ Crear el archivo de propiedades

- En Eclipse: **New → File**, crear un archivo llamado `database.properties`.
    
- Moverlo al paquete:
    
    `com.bharath.springcore.propertyplaceholder`
    
- Añadir propiedades:
    
```properties
    dbServer=bharathserver 
    dbPort=3306 
    dbUser=test 
    dbPass=test
```
    
    - Las claves y valores son de tipo `String`.
        
    - Si una clave se repite, **Spring tomará el último valor**.
        
    - Para comentar una línea, usar `#`.  
        Ejemplo:
        
        `# Default database port`
        
    - **Las claves son sensibles a mayúsculas/minúsculas** (`dbUser` ≠ `dbuser`).
        

---

### 2️⃣ Crear la clase POJO y el bean de Spring

- Crear la clase `MyDAO` en el paquete `propertyplaceholder`:
    
```java
private String dbServer;  

public MyDAO(String dbServer) {     
	this.dbServer = dbServer; 
}  

@Override public String toString() {     
	return "MyDAO [dbServer=" + dbServer + "]"; 
}
```
    
    _Se usará **inyección por constructor** para asignar el valor de `dbServer`._
    
- En el archivo `config.xml`:
    
    ```xml
    <beans xmlns:context="http://www.springframework.org/schema/context"        ...>     <context:property-placeholder         location="com/bharath/springcore/propertyplaceholder/database.properties"/>      <bean id="myDAO" class="com.bharath.springcore.propertyplaceholder.MyDAO">         <constructor-arg value="${dbServer}"/>     </bean> </beans>
    ```
    
    - `<context:property-placeholder>` enlaza el archivo de propiedades.
        
    - Con `${dbServer}` se indica que se debe **leer el valor** de la clave `dbServer`  
        del archivo `database.properties`.
        

---

### 3️⃣ Crear la clase de prueba

- Clase `Test`:
    
    `ApplicationContext context =     new ClassPathXmlApplicationContext(         "com/bharath/springcore/propertyplaceholder/config.xml");  MyDAO dao = (MyDAO) context.getBean("myDAO"); System.out.println(dao);`
    
- Al ejecutar:
    
    - Spring carga el archivo `database.properties`,
        
    - Lee la propiedad `dbServer`,
        
    - Y la inyecta en el constructor de `MyDAO`.
        

---

### Resultado

La consola imprime el nombre del servidor de base de datos:

`MyDAO [dbServer=bharathserver]`

demostrando que el valor proviene del archivo de propiedades.

---

**Resumen completo del documento:**

El documento explica cómo **externalizar propiedades** en Spring para evitar valores fijos en el código:

1. **Crear un archivo `.properties`** con las claves y valores de configuración (ej.: `dbServer`, `dbPort`, `dbUser`, `dbPass`).
    
2. **Configurar `context:property-placeholder`** en el XML de Spring para enlazar el archivo de propiedades.
    
3. **Inyectar las propiedades** en los beans usando la sintaxis `${propiedad}` dentro de `<value>` o `<constructor-arg>`.
    

Esto permite:

- Cambiar configuraciones sin recompilar,
    
- Centralizar los valores en un único archivo,
    
- Y usar inyección de dependencias de forma flexible y mantenible.