
---


En esta lección vamos a configurar un **proyecto Maven muy sencillo**.

Ya he compartido las dependencias, así que usando esas dependencias quiero que configures un proyecto similar.  
Puedes darle el nombre que quieras.

Yo estoy usando **Java JDK 22**, pero eso **no es obligatorio**.  
No vamos a usar ninguna característica especial de la versión más reciente de Java, así que puedes usar la versión que prefieras.

Una vez que el proyecto esté creado con las dependencias dadas, esta será la dependencia principal:  
la **dependencia de S3**.

También verás que se usa **SLF4J** para logging.  
Puedes usar _logback_ u otro framework si quieres.  
En este caso, solo se usa una implementación _no-op_ para simplificar.

---

### Estructura del proyecto

Dentro de `src/main/java`, bajo el paquete correspondiente, se crea una clase llamada **S3Demo**.

Ahora vamos a hacer dos cosas:

1. **Escribir un archivo en un bucket S3**
    
2. **Leer un archivo desde S3**
    

---

### Crear un archivo de prueba

Primero creamos un archivo de ejemplo llamado **hello.txt**.

El contenido del archivo será algo simple, por ejemplo:

```
hello world
```

Este será el archivo que subiremos al bucket S3.

---

### Configuración básica

El nombre del bucket será, por ejemplo: **VinceDemo**.

Creamos el método `public static void main`.

---

### Crear el cliente de S3

Primero creamos un **S3Client**:

- Usamos `S3Client.builder()`
    
- Indicamos la **región** (por ejemplo `us-east-1`)
    
- Llamamos a `build()`
    

El `S3Client` implementa `AutoCloseable`, así que usamos **try-with-resources** para manejarlo correctamente.

---

### Subir un archivo a S3 (PutObject)

Para escribir el archivo en S3:

1. Creamos un `PutObjectRequest`
    
2. Indicamos:
    
    - **bucket** → nombre del bucket
        
    - **key** → nombre del objeto en S3  
        (por ejemplo `hello.txt` o `public/hello.txt`)
        
3. Construimos el request
    

Luego llamamos a:

- `client.putObject(request, Path.of("hello.txt"))`
    

Esto sube el archivo local `hello.txt` al bucket S3.

Hay muchas más opciones disponibles (como cifrado), pero para este ejemplo simple no son necesarias.

---

### Descargar un archivo desde S3 (GetObject)

Ahora vamos a leer un archivo desde S3.

1. Creamos un `GetObjectRequest`
    
2. Indicamos:
    
    - **bucket**
        
    - **key** del archivo a descargar (por ejemplo `public/02aws.png`)
        
3. Construimos el request
    

Luego llamamos a:

- `client.getObject(request, Path.of("aws.png"))`
    

Esto descarga el archivo desde S3 y lo guarda en la máquina local con el nombre indicado.

---

### Ejecución del programa

Al ejecutar el programa:

- El archivo **hello.txt** se sube correctamente a S3
    
- El archivo PNG se descarga desde S3 al proyecto local
    

---

### Verificación en la consola de AWS

Desde la consola de AWS:

- Entramos a S3
    
- Abrimos el bucket
    
- Verificamos que **hello.txt** existe
    

Luego se muestra cómo **eliminar el archivo** desde la consola:

- Seleccionamos el objeto
    
- Usamos _Delete_
    
- Confirmamos la eliminación escribiendo el texto solicitado
    

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento muestra cómo usar el **AWS SDK para Java (v2)** para interactuar con **Amazon S3** desde un proyecto Maven, cubriendo las operaciones básicas de subida y descarga de archivos.

---

## ⭐ 1. Proyecto Maven + AWS SDK

- Se crea un proyecto Java estándar con Maven
    
- Se agrega la dependencia de **S3**
    
- Se configura logging básico con SLF4J
    
- No depende de una versión específica de Java
    

---

## ⭐ 2. Creación del S3Client

- Se construye usando `S3Client.builder()`
    
- Se especifica la región
    
- Se usa _try-with-resources_ para manejo correcto del ciclo de vida
    

---

## ⭐ 3. Subida de archivos (PutObject)

- Se usa `PutObjectRequest`
    
- Se define:
    
    - Bucket
        
    - Key (nombre/ruta del objeto en S3)
        
- Se sube un archivo local usando `Path.of(...)`
    

Esto permite escribir archivos generados localmente en S3.

---

## ⭐ 4. Descarga de archivos (GetObject)

- Se usa `GetObjectRequest`
    
- Se especifica el objeto a descargar
    
- Se guarda en una ruta local
    

Ideal para recuperar archivos almacenados en S3.

---

## ⭐ 5. Verificación y limpieza

- Se valida la operación desde la consola de AWS
    
- Se eliminan objetos manualmente para mantener el bucket limpio
    

---

# 🎯 **Idea principal**

El **AWS SDK para Java** permite interactuar fácilmente con S3 desde aplicaciones Java usando un cliente simple y APIs claras.  
Con `putObject` y `getObject` se cubren la mayoría de casos básicos: **subir archivos generados por la aplicación y descargar archivos almacenados en la nube**.

---

Si quieres, el siguiente paso natural sería:

✅ Integrar este código en **Spring Boot**  
✅ Manejar **credenciales con perfiles y roles**  
✅ Subir archivos usando **streams** (no solo archivos locales)  
✅ Manejar errores y retries de forma profesional  
✅ Diseñar un **servicio S3 real para producción**