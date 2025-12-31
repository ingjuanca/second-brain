
---

## Creación de un usuario (IAM User)

Volvamos a la **consola de AWS**.

Vamos a trabajar con **IAM**.

Como es habitual, buscamos _IAM_ y lo marcamos como favorito para poder acceder fácilmente en el futuro.

Aquí podemos ver:

- User groups
    
- Users
    
- Roles
    
- Policies
    

Lo primero que vamos a hacer es **crear un usuario**.

Por ahora:

- No vamos a crear un user group
    
- No vamos a crear un role  
    Solo vamos a crear **un usuario**
    

Imaginemos que hemos contratado a **un desarrollador** para ayudarnos a manejar el negocio.

Este desarrollador necesita iniciar sesión en nuestra cuenta de AWS para administrar recursos como **EC2**, etc.

---

### Creación del usuario

Asignamos un nombre de usuario.  
Por ejemplo: **VinceDev**.

Luego habilitamos:

- **AWS Management Console access** (acceso a la consola)
    

AWS recomienda otras opciones (por ejemplo integración con Active Directory), pero para este ejemplo **usaremos acceso directo a la consola**.

Indicamos que **administraremos manualmente este usuario**.

Configuramos una contraseña simple (solo para la demo, **no recomendado en producción**).

Podemos marcar la opción:

- _User must create a new password at next sign-in_  
    (lo que obliga al usuario a cambiar la contraseña al iniciar sesión)
    

En este caso, la desmarcamos para simplificar la demostración.

---

### Permisos del usuario

En este punto, AWS pregunta:

- ¿Qué permisos queremos darle al usuario?
    
- ¿Queremos agregarlo a un grupo?
    

Por ahora:

- **No le damos ningún permiso**
    
- **No lo agregamos a ningún grupo**
    

Continuamos.

Revisamos la configuración y creamos el usuario.

El usuario queda creado y AWS nos muestra:

- Username
    
- Password
    
- URL de login
    

👉 Es importante **guardar estas credenciales**, ya que las necesitaremos.

---

### Inicio de sesión con el nuevo usuario

Abrimos otro navegador (por ejemplo Firefox o Safari).

Pegamos la URL de login de IAM.

Ingresamos:

- Username
    
- Password
    

El usuario puede iniciar sesión correctamente.

---

### Verificación de permisos

Una vez dentro, intentamos acceder a distintos servicios:

#### EC2

- Acceso denegado
    
- No puede ver instancias
    
- No puede lanzar instancias
    
- No puede ver VPCs ni configuraciones necesarias
    

#### S3

- No puede listar buckets
    
- No puede acceder a ningún recurso
    

Esto demuestra que:  
👉 **El usuario existe, pero no tiene permisos para hacer nada**

El usuario solo puede iniciar sesión, nada más.

---

### Mala práctica vs buena práctica

Podríamos asignar permisos directamente al usuario (**no es buena práctica**).

La forma correcta es:

- Crear un **User Group**
    
- Asociar permisos al grupo
    
- Agregar el usuario al grupo
    

Eso es lo que haremos a continuación para darle permisos al desarrollador.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento demuestra paso a paso cómo **crear un usuario en AWS IAM**, validar su acceso y comprobar el comportamiento cuando **no se le asignan permisos**, reforzando conceptos clave de seguridad en AWS.

---

## ⭐ 1. Creación de usuarios IAM

- Un usuario IAM representa a una **persona real**
    
- Puede tener acceso a la consola de AWS
    
- Se le asignan credenciales propias (username + password)
    

---

## ⭐ 2. Usuarios sin permisos

- Un usuario recién creado **no tiene acceso a ningún servicio**
    
- Puede iniciar sesión, pero:
    
    - No puede usar EC2
        
    - No puede usar S3
        
    - No puede ver recursos
        
- Esto valida el **principio de menor privilegio por defecto**
    

---

## ⭐ 3. Principio de menor privilegio

AWS sigue el enfoque:

> _Un usuario no puede hacer nada hasta que explícitamente se le concedan permisos_

Esto reduce:

- Riesgos de seguridad
    
- Errores humanos
    
- Costos innecesarios
    

---

## ⭐ 4. Buenas prácticas IAM

- ❌ No asignar permisos directamente a usuarios
    
- ✅ Crear **User Groups**
    
- ✅ Asignar policies a los grupos
    
- ✅ Agregar usuarios a los grupos según su rol
    

---

# 🎯 **Idea principal**

La creación de usuarios en IAM debe hacerse **sin permisos por defecto**, y los accesos deben otorgarse de forma controlada mediante **grupos y políticas**, aplicando siempre el **principio de menor privilegio**.

---
