
---

Vamos a nuestra consola de AWS.

Asegúrate de estar en la región correcta.

Ahora intentemos acceder al servicio S3.

Escribamos “S3” y añadámoslo a favoritos para poder acceder fácilmente más adelante.

Si nunca has creado un bucket, esto es lo que probablemente verás.

Puedes crear un bucket desde aquí, usando este botón.

Si haces clic, podrás ver todos los buckets si seleccionas la opción _Buckets_.

Actualmente no tenemos nada. Perfecto.

Este directorio llamado _buckets_ es un concepto nuevo.  
AWS lo introdujo recientemente.

Los **general purpose buckets** (buckets de propósito general) son los tradicionales.  
Son los que vamos a usar.

Creemos entonces un bucket.

El bucket se creará en esta región.

Hay un concepto nuevo aquí, pero lo ignoraremos por ahora.

Seleccionamos **general purpose** porque es lo que necesitamos.

Ahora debemos proporcionar un nombre **globalmente único**.

Demos algún nombre —esperemos que esté disponible.

Asignamos un nombre, y todas las demás opciones las dejamos por defecto.  
Si intento explicarlas ahora sería aburrido; lo haré más adelante.

Por ahora, dejamos todo por defecto y creamos el bucket.

Perfecto, el bucket fue creado.

Hacemos clic en él.

Este es mi bucket en la nube.

Ahora puedo subir mis archivos y mantenerlos seguros si quiero.

Subamos un archivo: toma alguna imagen o cualquier archivo y arrástralo dentro.

Ahora tengo un archivo llamado “01.png”.

Solo lo arrastré y solté. Eso es todo.

Hacemos scroll hacia abajo y presionamos _Upload_.

El archivo ya se ha subido.

Ahora vamos nuevamente al bucket.

Aquí puedo ver el archivo.  
Hagamos clic sobre él.

Aquí obtenemos una vista general del objeto: última modificación, tamaño, tipo, etc.

Este es el **S3 URI**, que usaremos para acceder al archivo desde EC2 u otros servicios.  
Demostraré eso más adelante.

Cualquier recurso que subimos es considerado por AWS como un recurso de Amazon, por lo que tendrá un nombre único.

Y aquí vemos una URL.

Si la copiamos e intentamos acceder a ella, **no funcionará**, porque no hemos otorgado ningún permiso.  
Lo cual es bueno: por defecto nadie puede acceder.

Pero si queremos dar acceso, podemos hacerlo.

Llegaremos a eso más adelante.

En S3 existe el concepto de **pre-signed URL**.

Esto sirve para cuando nadie debe poder acceder al archivo, pero quieres compartirlo temporalmente de forma segura.

Vamos a _Object Actions_ y seleccionamos **Share with a pre-signed URL**.

Esto generará un token temporal que expirará después de un tiempo.

Podemos crear un token que expire en un minuto.

Creamos la pre-signed URL.

Ya se generó.  
Cópiala si quieres.

Si la pegas ahora, notarás que es la misma URL, pero con un **token de seguridad** añadido.

Cualquier persona que tenga esta URL con token podrá acceder al archivo temporalmente.

Esto permite compartir archivos de forma segura con personas específicas, solo durante esa ventana de tiempo.

Si refrescas la página dentro del minuto, funcionará.  
Después de un minuto, si refrescas, probablemente ya no funcionará.

Eso es todo.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento demuestra cómo crear un bucket en S3, subir un archivo y compartirlo de manera temporal y segura mediante **pre-signed URLs**.

---

## ⭐ 1. Creación del bucket

- Se accede al servicio S3 desde la consola de AWS.
    
- Se selecciona la región correcta.
    
- Se crea un **bucket de propósito general** (el tipo tradicional).
    
- El nombre del bucket debe ser **globalmente único**.
    
- Todas las opciones se dejan por defecto.
    

---

## ⭐ 2. Subida de archivos (objetos)

- Dentro del bucket se pueden subir archivos arrastrando y soltando.
    
- Cada archivo subido se considera un **objeto S3**.
    
- S3 muestra metadatos: tamaño, tipo, fecha de modificación, S3 URI, etc.
    
- La URL pública del objeto no funciona por defecto, ya que los buckets son privados.
    

---

## ⭐ 3. Seguridad por defecto

- Los objetos **no son accesibles públicamente**.
    
- Esto es deseable: evita filtraciones por error.
    
- Cualquier acceso público requiere pasos explícitos.
    

---

## ⭐ 4. Pre-signed URLs

- Permiten compartir un objeto sin hacerlo público.
    
- Se generan desde _Object Actions → Share with a pre-signed URL_.
    
- La URL incluye un token que expira (por ejemplo, 1 minuto).
    
- Mientras no expire, cualquiera que tenga la URL puede acceder al archivo.
    
- Después de la expiración, la URL deja de funcionar.
    

---

# 🎯 **Idea principal**

Este documento enseña el flujo básico de trabajo con S3:

1. Crear un bucket
    
2. Subir un archivo
    
3. Mantenerlo seguro por defecto
    
4. Compartirlo temporalmente usando una URL firmada sin comprometer la seguridad del bucket
    

---

Si quieres, también puedo:

✅ darte un resumen ultra breve (5 líneas)  
✅ explicarte cómo manejar permisos S3 de forma profesional (IAM, bucket policies, ACL)  
✅ mostrarte cómo generar pre-signed URLs desde Java/Spring Boot  
✅ preparar un checklist de seguridad S3 para tus proyectos