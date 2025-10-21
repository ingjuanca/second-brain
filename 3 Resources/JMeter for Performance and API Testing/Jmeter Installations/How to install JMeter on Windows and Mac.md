
---

El documento proporciona un tutorial paso a paso sobre cómo instalar y lanzar Apache JMeter, una herramienta de pruebas de rendimiento, en dos sistemas operativos principales: Windows y Mac OS.

### 1. Prerrequisitos (Java)

- **Requisito Indispensable:** JMeter es una aplicación basada puramente en Java, por lo que la instalación de Java es un prerrequisito para instalar JMeter.
    
- **Verificación en Windows:** Para verificar si Java está instalado, se debe abrir la Símbolo del sistema (_Command Prompt_) y ejecutar el comando `java -version`.
    
- **Verificación en Mac:** En Mac OS, se utiliza el **Terminal** para ejecutar el mismo comando `java -version` para verificar la instalación de Java.
    
- **Versión de Java:** JMeter, en su versión actual, soporta Java 8 y versiones superiores ("Java 8 plus").
    

### 2. Proceso de Instalación (Común para Ambos)

1. **Descarga:** Se accede al sitio web oficial de Apache JMeter buscando "download Jmeter" en Google.
    
2. **Selección de Archivo:** Se navega a la sección de **Binarios** y se descarga el archivo comprimido.
    
    - **Windows:** Se descarga un archivo `.zip`.
        
    - **Mac OS:** Se descarga un archivo `.tar` o binario (el primero en la lista).
        
3. **Extracción:** Una vez descargado, el archivo se extrae.
    
    - En **Windows**, se extrae el `.zip`.
        
    - En **Mac OS**, se utiliza la utilidad de archivos (_Archive utility app_) para extraer el `.tar`.
        
4. **Ubicación:** El contenido extraído (la carpeta `apache-jmeter-x.x`) se copia y se coloca en una ubicación accesible, como la unidad `C:` en Windows o el directorio de inicio en Mac OS.
    

### 3. Ejecución de JMeter

El proceso de lanzamiento varía según el sistema operativo:

#### **A. En Windows**

1. **Navegar al Directorio:** Abrir la carpeta raíz de JMeter y luego ir a la subcarpeta `bin`.
    
2. **Ejecutar el Archivo Batch:** Dentro de la carpeta `bin`, se busca el archivo `jmeter.bat`.
    
3. **Lanzamiento:** Se hace doble clic en el archivo `.bat` para ejecutarlo.
    
4. **Resultado:** Esto abrirá una ventana de comandos (_command window_) y luego lanzará la Interfaz de Usuario (UI) de Apache JMeter.
    

#### **B. En Mac OS**

1. **Navegar al Directorio:** Abrir la carpeta raíz de JMeter y luego ir a la subcarpeta `bin`.
    
2. **Ejecutar el Archivo Shell:** Dentro de la carpeta `bin`, se busca el archivo `jmeter.sh`.
    
3. **Lanzamiento desde Terminal:**
    
    - Abrir el **Terminal**.
        
    - Navegar al directorio `/bin` de JMeter usando el comando `cd`.
        
    - Ejecutar el archivo shell usando el comando `./jmeter.sh` (o solo `jmeter` en versiones recientes).
        
4. **Resultado:** El comando iniciará el proceso y lanzará la Interfaz de Usuario (UI) de Apache JMeter.