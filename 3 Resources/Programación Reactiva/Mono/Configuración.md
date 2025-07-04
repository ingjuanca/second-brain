
---
### ⚙️ Configuración del Proyecto

- **IDE sugerido**: IntelliJ (pero puedes usar cualquier otro).
- **Sistema de construcción**: Maven (o Gradle, si prefieres).
- **Versión de Java**: Se recomienda Java 21 (mínimo Java 17).
- **Nombre del proyecto**: _Reactive Programming Playground_ (puedes usar otro).

---

### 📦 Dependencias Maven

El archivo [pom.xml](https://github.com/vinsguru/java-reactive-programming-course/blob/master/01-reactive-programming-playground/pom.xml#L11-L80) debe incluir:

1. **Project Reactor**  
    → Implementación de Reactive Streams.    
2. **Reactor Netty**  
    → Para realizar llamadas HTTP en las demostraciones.
```xml
    <dependencies>
	    <dependency>
			<groupId>io.projectreactor</groupId>
			<artifactId>reactor-core</artifactId>
		</dependency>
		<dependency>
			<groupId>io.projectreactor.netty</groupId>
			<artifactId>reactor-netty-core</artifactId>
		</dependency>
		<dependency>
			<groupId>io.projectreactor.netty</groupId>
			<artifactId>reactor-netty-http</artifactId>
		</dependency>		
	 </dependencies>	

	<dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>io.projectreactor</groupId>
                <artifactId>reactor-bom</artifactId>
                <version>${reactor.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
```
3. **Logback**  
    → Para manejo de logs.
    
```xml
	<dependency>
		<groupId>ch.qos.logback</groupId>
		<artifactId>logback-classic</artifactId>
		<version>${logback.version}</version>
	</dependency>
```

```xml
    <configuration>
	    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
	        <encoder>
	            <pattern>%d{HH:mm:ss.SSS} %-5level [%15.15t] %cyan(%-30.30logger{30}) : %m%n</pattern>
	        </encoder>
	    </appender>
	    <logger name="io.netty.resolver.dns.DnsServerAddressStreamProviders" level="OFF"/>
	    <root level="INFO">
	        <appender-ref ref="STDOUT" />
	    </root>
	</configuration>
```
4. **Java Faker**  
    → Para generar datos de prueba aleatorios (nombres, países, libros, etc.).
```xml
    <dependency>
		<groupId>com.github.javafaker</groupId>
		<artifactId>javafaker</artifactId>
		<version>${faker.version}</version>
	</dependency>
```
4. **JUnit + Reactor Test**  
    → Para pruebas unitarias en programación reactiva.
```xml
    <!-- test dependencies -->
	<dependency>
		<groupId>org.junit.jupiter</groupId>
		<artifactId>junit-jupiter-engine</artifactId>
		<version>${junit.version}</version>
		<scope>test</scope>
	</dependency>
	<!-- step-verifier -->
	<dependency>
		<groupId>io.projectreactor</groupId>
		<artifactId>reactor-test</artifactId>
		<scope>test</scope>
	</dependency>
```

> ⚠️ Asegúrate de copiar correctamente las dependencias y cerrar correctamente las etiquetas XML (`</project>`).

---

### 🧪 Consideraciones sobre Testing Reactivo

- Las pruebas deben **suscribirse** al publicador para que este produzca datos.
- Reactor Test proporciona herramientas específicas para facilitar este tipo de pruebas.

---

### 📁 Archivos adicionales

- Se incluye un archivo `logback.xml` para configurar el sistema de logging.
    - Debe colocarse en: `src/main/resources/logback.xml`.

---