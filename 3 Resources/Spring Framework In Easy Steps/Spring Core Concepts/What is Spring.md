
---

En esta lección aprenderás qué es **Spring**.  
Al final, quiero que recuerdes dos puntos clave:

1. **Spring es un framework de inyección de dependencias.**
    
2. **Spring complementa los estándares existentes de Java EE**, haciéndolos más fáciles de usar para los desarrolladores.
    

---

### Inyección de dependencias

Al desarrollar aplicaciones de software, organizamos el código en **componentes**; en Java, estos componentes son **clases**.

Ejemplo:

- La **Clase A** depende de la **Clase B** para realizar su trabajo.
    
- En lugar de que Clase A cree un objeto de Clase B, **delegamos esta responsabilidad al framework Spring**.
    
- En tiempo de ejecución, **Spring crea dinámicamente el objeto de Clase B e inyecta la instancia en Clase A**.
    
- Así, los desarrolladores se enfocan en la **lógica de negocio**, no en la creación de objetos.
    

Este proceso de trasladar el control de creación de objetos del código de la aplicación a un **framework externo** se llama **Inversión de Control (IoC)**, un patrón de diseño.

---

### Aplicación en Java EE

Una aplicación Java EE suele estar organizada en **capas**:

- **Capa de UI (interfaz de usuario)**
    
- **Capa de servicios**
    
- **Capa de acceso a datos**, entre otras.
    

Cada capa utiliza los servicios de la capa inferior:

- Por ejemplo, el **Order Controller** de la UI utiliza el **Order Service**, y este a su vez usa el **Order DAO**.
    

Aquí es donde **Spring inyecta dependencias**:

- Crea automáticamente el **Order DAO** e inyecta en el **Order Service**.
    
- Crea el **Order Service** e inyecta en el **Order Controller**.
    

---

### Spring y las API/Frameworks existentes

Además de la inyección de dependencias, Spring **facilita el uso de APIs y frameworks existentes**:

- **Spring JDBC:** un envoltorio (wrapper) sobre JDBC de Oracle que permite ejecutar sentencias SQL con una sola línea, eliminando código repetitivo.
    
- **Spring ORM:** simplifica el uso de herramientas de mapeo objeto-relacional (ORM) como **Hibernate**.
    
- **Integración con frameworks de UI:** por ejemplo, **Struts** o **JavaServer Faces (JSF)** para crear interfaces de usuario de forma sencilla.
    

Cuando la comunidad de Spring considera que un estándar Java EE es difícil de usar (como Struts o JSF), crean implementaciones paralelas:

- **Spring MVC:** permite desarrollar la capa web de aplicaciones de manera sencilla.  
    Más adelante en el curso se estudia en detalle.
    

Spring también ofrece servicios listos para configurar como:

- **Seguridad**,
    
- **Gestión de transacciones**.
    

---

### Resumen general

Spring es, ante todo, un **framework de inyección de dependencias**.  
Permite delegar en el contenedor la creación e inyección de objetos en las distintas capas de una aplicación Java EE (UI, servicios, datos).

Además:

- **Simplifica** el uso de estándares como JDBC y ORM,
    
- **Integra** tecnologías de interfaz como Struts o JSF,
    
- **Proporciona** servicios como seguridad y gestión de transacciones de forma fácil de configurar.
    

En pocas palabras, **Spring reduce la complejidad del desarrollo Java EE** y permite concentrarse en la lógica de negocio en lugar de en detalles de infraestructura.