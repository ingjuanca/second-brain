
---

El **Spring Framework** está compuesto por **más de 20 módulos**, que se agrupan en:

- **Core Container (contenedor central)**
- **Data Access (acceso a datos)**
- **Web**
- **AOP (Programación Orientada a Aspectos)**
- **Instrumentación**
- **Testing**, entre otros.

Dependiendo de las **necesidades de la aplicación**, utilizamos solo los módulos requeridos. A continuación se describen los más importantes:

---

### 1. **Core Container (Contenedor central)**

Es el **corazón de Spring**.  
Proporciona:

- Implementación de **Inversión de Control (IoC)** y **soporte para Inyección de Dependencias (DI)**.
    
- Administra el **ciclo de vida de los objetos**, incluyendo:
    
    - Creación de objetos dependientes,
        
    - Inyección de dependencias,
        
    - Llamadas a métodos de inicialización (`init`) y destrucción (`destroy`).
        

---

### 2. **Data Access (Acceso a datos)**

Conjunto de varios submódulos:

- **JDBC**: Proporciona una capa de abstracción sobre JDBC que permite realizar operaciones en la base de datos con muy pocas líneas de código.
    
- **ORM**: Integra frameworks de mapeo objeto-relacional como **Hibernate**, facilitando su uso en pasos simples.
    
- **JMS**: Permite producir y consumir mensajes.
    
- **Transaction**: Soporta **gestión de transacciones** tanto de forma programática como declarativa.  
    Una transacción se considera una **unidad atómica de trabajo**.
    

---

### 3. **AOP (Aspect Oriented Programming)**

Permite **inyectar código o servicios en los objetos sin modificar sus clases**.  
Facilita la configuración de **concerns transversales**, como:

- Gestión de transacciones,
- Logging (registro),
- Seguridad.

---

### 4. **Web**

Uno de los módulos más importantes en el desarrollo de aplicaciones **Java EE**.  
Incluye:

- **Core Web**: Funcionalidades web básicas, como:
    
    - Carga de archivos,
        
    - Inicialización del contenedor Spring en un servidor de aplicaciones mediante **servlet listeners**,
        
    - Contenedor de aplicación web (Application Context) orientado a entornos web.
        
- **Spring MVC**: Implementación completa del patrón **Modelo-Vista-Controlador** para desarrollar **aplicaciones web dinámicas**.
    
- **WebSocket**: Soporte para comunicación en tiempo real.
    
- **Servlet y Portlet**:
    
    - **Servlet module**: Soporte para aplicaciones web tradicionales.
        
    - **Portlet module**: Implementación de MVC en entornos **Portlet**.
        

---

### 5. **Testing**

Facilita las **pruebas de aplicaciones Spring** usando:

- **JUnit** y **TestNG**.
    
- Proporciona **objetos simulados (mock objects)** para **probar capas de la aplicación Java EE de forma aislada**.
    

---

**Resumen completo del documento:**

El **Spring Framework** está estructurado en más de 20 módulos, organizados en grupos principales para cubrir diferentes áreas del desarrollo de aplicaciones Java EE:

- **Core Container**: núcleo de Spring, implementa **IoC y DI**, y gestiona el ciclo de vida completo de los objetos.
    
- **Data Access**: incluye submódulos para acceso a bases de datos (JDBC), integración con **ORM** como Hibernate, mensajería con **JMS** y **gestión de transacciones** programática y declarativa.
    
- **AOP**: permite implementar **servicios transversales** (logging, seguridad, transacciones) sin alterar el código original.
    
- **Web**: provee infraestructura para aplicaciones web, desde funcionalidades básicas (carga de archivos, inicialización del contenedor) hasta módulos especializados como **Spring MVC** (patrón MVC para aplicaciones dinámicas), **WebSocket**, **Servlet** y **Portlet**.
    
- **Testing**: soporta pruebas con **JUnit y TestNG**, y proporciona **mocks** para probar las distintas capas de forma aislada.
    

En conjunto, estos módulos hacen que Spring sea un **framework flexible y modular**, que se adapta a las necesidades específicas de cada aplicación Java EE.