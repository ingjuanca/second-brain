
---

El documento describe el proceso paso a paso para configurar, ejecutar y analizar una prueba de rendimiento básica utilizando Apache JMeter.

### 1. Lanzamiento de JMeter y el Test Plan

1. **Lanzamiento:** Para iniciar JMeter, se debe navegar a la carpeta `bin` de la ubicación de instalación, abrir la Símbolo del Sistema (_Command Prompt_), y ejecutar el archivo `Jmeter.bat`.
    
2. **Test Plan (Plan de Prueba):** La primera entidad visible es el _Test Plan_, que actúa como un contenedor para todos los elementos de JMeter y donde se guarda todo el proyecto.
    

### 2. Configuración del Thread Group (Grupo de Hilos)

El _Thread Group_ simula a los usuarios que realizan la prueba y es el elemento más importante de la configuración.

- **Creación:** Se añade haciendo clic derecho en el _Test Plan_ > **Add** > **Threads** (**Users**) > **Thread Group**.
    
- **Propiedades Esenciales:**
    
    - **Number of Threads (Users):** La cantidad de usuarios que realizarán la prueba. Por defecto es 1.
        
    - **Ramp-up Period (in seconds):** El tiempo de retardo entre el inicio de cada usuario. Por ejemplo, un _Ramp-up_ de 3 segundos con 5 usuarios significa que un nuevo usuario enviará su solicitud cada 3 segundos.
        
    - **Loop Count:** La cantidad de veces que el mismo conjunto de usuarios debe repetir la solicitud. Si se selecciona "Forever" (_Para siempre_), la prueba se ejecutará continuamente.
        

### 3. Adición del Sampler (Muestreador)

El _Sampler_ define el tipo de solicitud que el _Thread Group_ enviará al servidor.

- **Creación:** Se añade haciendo clic derecho en el _Thread Group_ > **Add** > **Sampler** > **HTTP Request**.
    
- **Configuración del HTTP Request:** Se introducen los detalles de la solicitud:
    
    - **Protocol:** `http` o `https`.
        
    - **Server Name or IP:** La URL del sitio web (ejemplo: `onlinetrainings.com`). **No** se incluye el protocolo (`http://` o `https://`).
        
    - **Path:** La ruta dentro del servidor (ejemplo: `/` representa la página de inicio o raíz).
        

### 4. Adición de Listeners (Oyentes)

Los _Listeners_ son cruciales para visualizar y reportar los resultados de la prueba.

- **Creación:** Se añaden haciendo clic derecho en el _Thread Group_ > **Add** > **Listener**.
    
- **Reportes Comunes:** Se recomienda añadir al menos dos tipos de oyentes:
    
    - **View Results Tree (Árbol de Resultados):** Muestra los detalles de cada solicitud (pasó/falló, tiempo de carga, latencia).
        
    - **View Results in Table (Resultados en Tabla):** Muestra un resumen tabular de las métricas clave, como el tiempo de conexión y el tiempo de latencia.
        

### 5. Ejecución, Detención y Limpieza de la Prueba

1. **Guardar:** Antes de ejecutar, el plan de prueba debe guardarse haciendo clic en el botón **Save**. El archivo se guarda con la extensión `.jmx`.
    
2. **Ejecución:** Seleccionar el _Test Plan_ y hacer clic en el botón **Start**. La ejecución comenzará inmediatamente.
    
3. **Análisis de Resultados:** Los _Listeners_ capturarán y mostrarán los resultados de la prueba, incluyendo el estado de la solicitud y las métricas de tiempo.
    
4. **Detención:**
    
    - **Stop:** Detiene la ejecución en curso.
        
    - **Shutdown (Apagado):** Detiene la ejecución y elimina completamente todos los hilos internos.
        
5. **Limpieza:** Para borrar los resultados de los _Listeners_ antes de una nueva ejecución, se utiliza la opción **Clear All**.