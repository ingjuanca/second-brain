
---

Hasta ahora, como desarrolladores, hemos estado trabajando directamente desde la **consola de AWS**.

Pero en la vida real, como desarrolladores, normalmente escribimos código **en nuestra máquina local** y necesitamos que ese código interactúe con servicios de AWS.

Por ejemplo, imaginemos que como parte de un requerimiento debemos escribir un archivo en un bucket **S3**.

¿Cómo probamos eso?

No podemos desplegar código directamente en AWS cada vez que queremos probar algo.  
Necesitamos poder **probar desde nuestra máquina local**.

---

### Instalación del AWS CLI

Primero, vamos al sitio oficial de AWS para instalar el **AWS CLI**.

AWS proporciona instrucciones detalladas para:

- Linux
    
- macOS
    
- Windows
    

La instalación es sencilla.

Por ejemplo, en macOS se puede usar:

```bash
brew install awscli
```

En Linux y Windows se siguen las instrucciones oficiales.

Una vez instalado, si ejecutas en la terminal:

```bash
aws
```

y el comando es reconocido (no aparece “command not found”), entonces el CLI está instalado correctamente.

---

### Escenario: desarrollador trabajando en local

Volvemos a la consola de AWS (cuenta administrador).

Tenemos un usuario desarrollador (por ejemplo, **VinceDev**).

Este desarrollador quiere:

- Escribir código Java localmente
    
- Probar que su aplicación pueda subir archivos a S3
    

El **username y password** del usuario IAM **solo sirven para acceso al navegador**, no para que una aplicación Java o el CLI se autentiquen contra AWS.

Para eso necesitamos **Access Keys**.

---

### Creación de Access Keys

Desde la consola IAM:

- Seleccionamos el usuario desarrollador
    
- Creamos un **Access Key**
    
- Indicamos que el caso de uso es **desarrollo local**
    
- Confirmamos que Identity Center no aplica en este caso
    
- AWS genera:
    
    - **Access Key ID**
        
    - **Secret Access Key**
        

⚠️ Estas credenciales se muestran **una sola vez** y deben guardarse.

---

### Configuración del AWS CLI

En la terminal ejecutamos:

```bash
aws configure
```

Y proporcionamos:

- Access Key ID
    
- Secret Access Key
    
- Región por defecto (por ejemplo, `us-east-1`)
    
- Output format (opcional)
    

Para macOS y Linux, las credenciales se guardan en:

```
~/.aws/credentials
```

Para Windows, AWS indica la ruta correspondiente en su documentación.

---

### Prueba inicial (sin permisos)

Ahora intentamos:

```bash
aws s3 ls
```

Aunque las credenciales están configuradas, el comando **falla**, porque el usuario desarrollador **no tiene permisos S3**.

---

### Asignar permisos al usuario

Desde la cuenta administrador:

- Vamos al **grupo developers**
    
- Agregamos la policy:
    
    - **AmazonS3FullAccess** (solo para la demo)
        

Ahora el desarrollador tiene permisos sobre S3.

---

### Prueba final desde local

Volvemos a la terminal y ejecutamos nuevamente:

```bash
aws s3 ls
```

Ahora el comando funciona correctamente.

Esto confirma que:

- El desarrollador puede acceder a S3
    
- El acceso se hace **desde su máquina local**
    
- Usando **Access Keys + AWS CLI**
    
- Controlado por **IAM policies**
    

En la siguiente lección, se utilizarán estas credenciales para **subir archivos a S3 usando Java**.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento explica cómo permitir que un **desarrollador trabaje localmente con servicios AWS**, usando **Access Keys y el AWS CLI**, sin depender de la consola web.

---

## ⭐ 1. Problema que se resuelve

- El acceso por navegador (username/password) **no sirve para aplicaciones**
    
- El desarrollo local requiere credenciales programáticas
    
- AWS CLI y SDKs necesitan **Access Keys**
    

---

## ⭐ 2. AWS CLI como herramienta base

- Permite interactuar con AWS desde terminal
    
- Usa el mismo modelo de autenticación que los SDKs
    
- Es ideal para pruebas locales y debugging
    

---

## ⭐ 3. Uso de Access Keys

- Se generan desde IAM para un usuario
    
- Se configuran con `aws configure`
    
- Se almacenan localmente en archivos seguros
    
- Permiten que código Java, scripts y CLI accedan a AWS
    

---

## ⭐ 4. Control mediante IAM

- Tener Access Keys **no implica tener permisos**
    
- Los permisos se otorgan vía:
    
    - User Groups
        
    - Policies
        
- Sin policy S3 → acceso denegado
    
- Con policy S3 → acceso permitido
    

---

## ⭐ 5. Buenas prácticas implícitas

- Usar Access Keys **solo para desarrollo local**
    
- Controlar permisos con IAM (principio de menor privilegio)
    
- No hardcodear credenciales en el código
    
- En producción usar **IAM Roles**, no Access Keys
    

---

# 🎯 **Idea principal**

Para desarrollo local, AWS permite usar **Access Keys + AWS CLI** para que aplicaciones y scripts interactúen con servicios como S3.  
La seguridad y el acceso siguen estando completamente controlados por **IAM**, y los permisos pueden ajustarse según el rol del desarrollador.

---

Si quieres, el siguiente paso natural sería:

✅ Escribir **código Java/Spring Boot** para subir archivos a S3  
✅ Comparar **Access Keys vs IAM Roles** (local vs producción)  
✅ Ver cómo usar **AWS SDK v2 con credenciales locales**  
✅ Checklist de seguridad para credenciales en desarrollo