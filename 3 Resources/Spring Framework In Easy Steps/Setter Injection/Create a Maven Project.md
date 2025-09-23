
---

En esta lección se explica cómo **crear un proyecto en Eclipse con Maven** para implementar **inyección de dependencias** en las siguientes prácticas.

---

### Creación del proyecto en Eclipse

1. **Abrir Eclipse IDE**.
2. Ir a **File → New → Maven Project** y dejar las opciones por defecto en la primera pantalla.
3. Hacer clic en **Next** y seleccionar **maven-archetype-quickStart**, utilizado para crear proyectos Java independientes (**standalone**).
4. **Group ID:** Identificador único similar al nombre de paquete en el repositorio Maven (ejemplo: `com.bharath.spring`).  
    _Maven creará las carpetas necesarias con este nombre._
5. **Artifact ID:** ID del proyecto (por ejemplo, `spring-core`).
6. Hacer clic en **Finish**.  
    _Eclipse generará un proyecto Maven con la estructura básica:_
    
    - `src/main/java` – Código fuente principal.
    - `src/test/java` – Código de pruebas.

```xml
<groupId>com.bharath.spring</groupId>
<artifactId>springcore</artifactId>
<version>0.0.1-SNAPSHOT</version>
<packaging>jar</packaging>
```

---

### Archivo `pom.xml`

El **`pom.xml`** es el **corazón de cualquier proyecto Maven**:

- Contiene las **coordenadas del proyecto**: `groupId`, `artifactId`, `version`, etc.
- Define el tipo de empaquetado (`jar` para un proyecto Java independiente).
- Incluye por defecto la dependencia de **JUnit**.


Maven gestiona las dependencias automáticamente:

- Descarga los archivos `.jar` necesarios,
- Los agrega al proyecto,
- Actualiza el **classpath**, evitando hacerlo manualmente.

---

### Configuración de dependencias Spring

Para este curso se añaden dos dependencias esenciales en el `pom.xml`:

- **spring-core**
- **spring-context**

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
</dependencies>
```

Versión recomendada: **4.3.6** (aunque ya existe la 5.x, esta es la más usada en proyectos).  
Se define una **propiedad para la versión** de Spring, facilitando el cambio futuro en un solo lugar.

También es necesario:

- Volver a incluir la dependencia de **JUnit** si se eliminó el test por defecto,
- Configurar el **compiler plugin** para que utilice **Java 1.8** (en lugar de 1.5 o 1.6 por defecto).

```xml
<build>
	<pluginManagement>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.2</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
				</configuration>
			</plugin>
		</plugins>
	</pluginManagement>
</build>
```

---

### Actualización del proyecto

Cada vez que se modifique el `pom.xml`:

1. **Clic derecho en el proyecto → Maven → Update Project → OK**.
    
2. Maven **reconstruirá el proyecto** y descargará automáticamente las dependencias declaradas, así como las **transitivas** (aquellas requeridas indirectamente por las dependencias principales).
    

---

**Resumen completo del documento:**

La lección guía la creación de un **proyecto Maven en Eclipse** para trabajar con **Spring Core** y practicar inyección de dependencias:

- Se crea un **proyecto Maven QuickStart**, definiendo un **Group ID** y un **Artifact ID**.
    
- El archivo **`pom.xml`** se configura para:
    
    - Incluir las dependencias **spring-core** y **spring-context**,
        
    - Añadir nuevamente **JUnit**,
        
    - Configurar el **compiler plugin** para **Java 1.8**,
        
    - Definir una propiedad para manejar de forma centralizada la **versión de Spring**.
        
- Maven gestiona la descarga de dependencias y sus requisitos **transitivos**, evitando descargar manualmente los `.jar`.
    
- Para aplicar los cambios del `pom.xml` es necesario **actualizar el proyecto Maven** desde Eclipse.
    

En conclusión, Maven simplifica la configuración de proyectos Java con Spring al automatizar la gestión de dependencias y el proceso de compilación.