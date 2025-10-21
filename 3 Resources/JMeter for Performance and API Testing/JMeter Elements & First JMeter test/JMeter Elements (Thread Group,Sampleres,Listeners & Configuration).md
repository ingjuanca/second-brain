
---

El documento introduce y explica los diferentes componentes estructurales de la herramienta de pruebas de rendimiento Apache JMeter, los cuales son conocidos como **elementos**. Cada elemento está diseñado para un propósito específico dentro de un plan de prueba (Test Plan).

Los principales elementos de JMeter incluyen: **Thread Group** (Grupo de Hilos), **Samplers** (Muestreadores), **Logic Controllers** (Controladores Lógicos), **Listeners** (Oyentes), **Configuration Elements** (Elementos de Configuración), **Assertions** (Aserciones) y **Timers** (Temporizadores).

El documento se enfoca en describir los cuatro elementos más básicos y esenciales:

### 1. Thread Group (Grupo de Hilos)

- **Propósito:** Es una colección de hilos (_threads_) , donde cada hilo representa un **usuario** que utiliza la aplicación bajo prueba.
    
- **Función:** Simula la solicitud de un usuario real al servidor. Permite configurar el **número de hilos** (usuarios virtuales) para el grupo.
    
- **Ejemplo:** Si se configuran 100 hilos, JMeter simulará y creará 100 solicitudes de usuarios al servidor. Cada usuario envía una solicitud y recibe una respuesta.
    

### 2. Samplers (Muestreadores)

- **Propósito:** Representan el **tipo de solicitud** que los usuarios (los hilos) deben enviar al servidor.
    
- **Función:** Los hilos simulan solicitudes de usuario, y los Samplers definen la naturaleza de esa solicitud.
    
- **Tipos de Solicitudes (Samplers):**
    
    - **FTP Request** (Solicitud FTP)
        
    - **HTTP Request** (Solicitud HTTP)
        
    - **JDBC Request** (Solicitud JDBC)
        
    - Otros: BSF, Access Log, SMTP, etc..
        

### 3. Listeners (Oyentes)

- **Propósito:** Muestran los **resultados de la ejecución** de la prueba.
    
- **Función:** Generan reportes y visualizan los resultados en diferentes formatos.
    
- **Formatos de Resultados (Listeners):**
    
    - **Formato de Gráfico:** Muestra el tiempo de respuesta del servidor en un gráfico (por ejemplo, **Graph Result**).
        
    - **Formato Tabular:** Muestra un resumen de los resultados de la prueba en formato de tabla (por ejemplo, **Tableau Result**).
        
    - **Formato de Árbol/HTML:** Muestra el resultado de la solicitud del usuario en formato HTML básico (por ejemplo, **View Result**).
        
    - **Formato de Archivo de Registro (Log File):** Muestra el resumen de los resultados de la prueba en un archivo de texto.
        

### 4. Configuration Elements (Elementos de Configuración)

- **Propósito:** Se utilizan para configurar **variables predeterminadas y comunes** que pueden ser utilizadas por los Samplers.
    
- **Función:** Permiten definir variables compartidas, como URLs o conjuntos de datos, que son requeridas por múltiples tipos de Samplers.
    
- **Ejemplos:**
    
    - **CSV Data Set Config** (Configuración de Conjunto de Datos CSV)
        
    - **HTTP Cookie Manager** (Administrador de Cookies HTTP)
        
    - **HTTP Request Defaults** (Configuración por Defecto de Solicitud HTTP)
        

---