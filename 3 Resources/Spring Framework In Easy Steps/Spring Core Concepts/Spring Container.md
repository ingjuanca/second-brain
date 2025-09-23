
---

En esta lección aprenderás qué es un **contenedor de Spring**.

Un **contenedor** es un programa o componente predefinido que viene con el **Spring Framework**, y es responsable de:

- **Crear objetos**,
- **Mantenerlos en memoria**,
- **Inyectarlos en otros objetos según se requiera**,
- **Gestionar el ciclo de vida completo de un objeto**, desde su **creación** hasta su **destrucción**.

---

### Requisitos del contenedor

El contenedor necesita dos elementos:

1. **Beans (clases Java de la aplicación)**  
    Son las clases para las que se requiere crear objetos.
    
2. **Configuración en XML**  
    Archivo que el desarrollador crea para indicar:
    
    - Qué dependencias tiene cada objeto,
        
    - Cómo deben ser creados.
        

Con esta información, el contenedor:

- **Crea todos los objetos necesarios**,
- **Mantiene su ciclo de vida**,
- Permite que el **código de la aplicación obtenga estos objetos** solicitándolos directamente.

---

### ApplicationContext

En Spring, la interfaz **ApplicationContext** representa el **contenedor**.  
Al ser una **interfaz**, **no se pueden crear instancias directamente**.  
Se utilizan sus **implementaciones**:

- **ClasspathXmlApplicationContext**: busca el archivo de configuración XML en el **classpath de Java**.
- **FileSystemXmlApplicationContext**: busca el archivo de configuración XML en el **sistema de archivos**, fuera del classpath.
- **AnnotationConfigApplicationContext**: se usa cuando los **beans** se configuran mediante **anotaciones** en vez de XML.

En las próximas prácticas, normalmente se utilizará **ClasspathXmlApplicationContext**.

---

**Resumen completo del documento:**

El **contenedor de Spring** es el componente central que **crea, gestiona y destruye objetos (beans)** de una aplicación, y se encarga de **inyectar dependencias automáticamente**.  
Para funcionar requiere:

1. **Clases Java (beans)**, y
2. **Un archivo de configuración XML** que define las dependencias.

Con estos elementos, el contenedor construye los objetos necesarios y controla su ciclo de vida completo.

La interfaz **ApplicationContext** representa este contenedor. Sus implementaciones principales son:

- **ClasspathXmlApplicationContext** (configuración en el classpath),
- **FileSystemXmlApplicationContext** (configuración en el sistema de archivos),
- **AnnotationConfigApplicationContext** (configuración mediante anotaciones).

En la práctica, es común utilizar **ClasspathXmlApplicationContext** para cargar la configuración desde el classpath.