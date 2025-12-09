
---

En esta lección vamos a subir un par de archivos a nuestro bucket S3 y vamos a proporcionar **acceso público**.

Nuevamente, esto lo hacemos **intencionalmente con fines de aprendizaje**.  
Más adelante en el curso veremos **opciones mucho mejores y más seguras**.  
Lo que estamos haciendo ahora luego será reemplazado correctamente, pero para entender cómo funciona, debemos avanzar paso a paso.

---

Primero, subamos los archivos.  
Ya te he compartido los archivos previamente.  
Simplemente copia los archivos y arrástralos al bucket.  
Desplázate hacia abajo, presiona _Upload_ y listo.

Ahora vamos al bucket.  
Aquí podemos ver todos los archivos.

---

## 1. Desbloquear acceso público

Este es nuestro bucket.  
Ve a **Bucket permissions**.

Por defecto, **el acceso público está bloqueado**.

Editamos esa sección, desmarcamos la casilla y guardamos.  
AWS pedirá confirmación; aceptamos.

Ahora el acceso público está desactivado.  
Pero **eso no significa que el bucket sea público todavía**.  
Todavía no funcionará.

Para que funcione, debemos agregar una **Bucket Policy**.

Piensa en esto como una validación adicional:

- Primero, desactivamos el bloqueo de acceso público.
    
- Luego debemos **adjuntar explícitamente una política** que diga qué objetos queremos exponer y a quién.
    

---

## 2. Crear una Bucket Policy

Una bucket policy es un conjunto de permisos escritos en **JSON** que se adjunta al bucket.

La política dice:

- Qué objetos se exponen
    
- Qué acciones pueden hacerse
    
- Qué entidades (principals) tienen acceso
    

Hacemos clic en **Edit** para crear la política.

Luego vamos a **Policy Generator**, que abre una ventana nueva.

Configuramos:

- **Tipo de política**: S3 Bucket Policy
    
- **Effect**: Allow (permitir)
    
- **Principal**: `*` (cualquiera en el mundo)
    
- **Service**: Amazon S3
    
- **Action**: `GetObject` (solo lectura)
    

AWS permite acciones muy finas, pero aquí solo daremos acceso de lectura.

---

## 3. Definir el ARN y los recursos

Copiamos el ARN del bucket.

Luego, en la política, debemos añadir la clave (key):

- La key es el archivo: file1.txt, file2.txt, etc.
    
- Si queremos exponer **todos** los archivos, usamos `*`.
    

Entonces queda:

```
arn:aws:s3:::nombre-bucket/*
```

Generamos la política, copiamos el JSON y lo pegamos en _Edit bucket policy_.  
Guardamos.

---

## 4. Verificación del acceso público

Vamos al bucket.  
Si copiamos la URL de un archivo y la pegamos en el navegador, ahora funciona.  
Cualquier persona con esa URL puede ver el archivo.

Probamos otro archivo y también es accesible.

---

## 5. Restringir qué archivos son públicos

Supongamos que solo queremos exponer archivos `.png`, pero no `.jpg`.

Entonces editamos la política:  
En lugar de:

```
arn:aws:s3:::bucket/*
```

Usamos:

```
arn:aws:s3:::bucket/*.png
```

Guardamos.

Ahora:

- `.png` → accesible públicamente
    
- `.jpg` → ya no funciona
    

---

## 6. Nota importante

No recomiendo poner archivos sensibles y públicos en el **mismo bucket** y luego usar patrones como `*.png` o `*secret*`.

Esto es solo para aprender cómo funcionan las bucket policies.

En la vida real:

- Usa **buckets separados** para contenido público y contenido privado.
    
- Nunca guardes archivos sensibles en un bucket cuyo acceso público ha sido desactivado manualmente.
    
- Debemos ser extremadamente cuidadosos.
    

Aquí solo estamos **experimentando**.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento explica cómo hacer que ciertos objetos dentro de un bucket S3 sean públicos utilizando **bucket policies**, y advierte sobre buenas prácticas de seguridad.

---

## ⭐ 1. Subida de archivos

Se suben varios archivos al bucket mediante arrastrar y soltar.  
Los objetos aparecen listados dentro del bucket.

---

## ⭐ 2. Desbloqueo del acceso público

- Los buckets S3 bloquean el acceso público por defecto.
    
- Se debe desactivar manualmente la opción _Block Public Access_.
    
- Esto **no** hace el bucket público automáticamente; solo permite configurarlo.
    

---

## ⭐ 3. Creación de una Bucket Policy

Para hacer accesible un archivo se necesita una política:

- Escrita en JSON
    
- Adjunta al bucket
    
- Permite definir quién (Principal) puede hacer qué (Action) sobre qué recursos (ARN)
    

Configuración usada en el documento:

- Principal = `*` (público en general)
    
- Action = `GetObject` (solo lectura)
    
- Resource = `arn:aws:s3:::bucket/*` (todos los objetos)
    

Después de aplicarla, los archivos son accesibles vía URL pública.

---

## ⭐ 4. Control granular del contenido público

Se puede restringir qué archivos son públicos usando patrones como:

- `arn:aws:s3:::bucket/*.png`
    

Resultado:

- `.png` → accesibles
    
- `.jpg` → no accesibles
    
- Otros archivos → bloqueados
    

---

## ⭐ 5. Advertencia de seguridad

Esta práctica es solo para aprendizaje.

En entornos reales:

- **No mezclar archivos sensibles y públicos en el mismo bucket**
    
- Usar **buckets separados**
    
- Evitar patrones amplios como `*` en políticas públicas
    
- Usar mejores opciones como CloudFront + OAC, presigned URLs, IAM, etc.
    

---

# 🎯 **Idea principal**

La configuración del acceso público en S3 requiere dos pasos:

1. Desactivar el bloqueo de acceso público
    
2. Adjuntar una bucket policy que permita acceso explícito a ciertos objetos
    

Este proceso debe hacerse con extremo cuidado, pues un error puede exponer información sensible.

---

Si quieres, también puedo:

✅ generar la bucket policy final limpia y formateada  
✅ explicarte cómo hacer esto desde Spring Boot con AWS SDK  
✅ darte un checklist profesional de seguridad S3  
✅ convertir esta sección en notas para certificación AWS SAA o Developer Associate