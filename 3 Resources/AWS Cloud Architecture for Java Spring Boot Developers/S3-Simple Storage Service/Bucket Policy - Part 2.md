
---

En esta lección juguemos con la opción **Crear carpeta**.

Creemos una carpeta llamada _public_.  
Así.  
La creamos.

Ahora podemos verla aquí.

Lo que voy a hacer ahora es seleccionar estos dos archivos y moverlos dentro de esta carpeta _public_.

Vamos a **Actions → Move**.

¿A dónde quieres moverlos?

Elegimos el destino: hacemos clic en _Browse_ y seleccionamos la carpeta _public_.  
Si quisiéramos moverlos a otro bucket, también podríamos.

Y eso es todo.  
Presionamos **Move**.

---

Esto será una lección rápida.

Vayamos nuevamente a nuestros buckets de S3.

Entramos al bucket _Vince demo_.

Si quieres mantener las cosas más organizadas —por ejemplo, en un entorno real podrías tener cientos de archivos— puede que no quieras tener todo al mismo nivel.  
Puedes crear carpetas para organizarlos.

Creo rápidamente una carpeta llamada _public_.  
Eso es todo.

Ahora selecciono estos dos archivos PNG.  
Vamos a _Actions → Move_.

Seleccionamos el destino.  
Podrías seleccionar otro bucket, pero en este caso solo tenemos uno.  
Queremos mover los archivos a _public_, así que seleccionamos esa carpeta.

Seleccionamos el destino y presionamos **Move**.

La operación ha finalizado.

Ahora, si miras la URL anterior (del archivo fuera de la carpeta), ya no funcionará, porque esos archivos _ya no existen_ en esa ubicación.

Para accederlos ahora, debemos incluir la carpeta **public** en la ruta.

El ARN también habrá cambiado.  
El nombre de la carpeta pasa a ser parte del nombre del archivo.

Por eso al refrescar, la URL anterior falla: los archivos se movieron.

---

¿Qué debemos hacer si queremos dar acceso público **solo** a los archivos dentro de _public_?

Debemos **editar la bucket policy**.

Podemos escribir una política que permita acceso únicamente a todos los archivos PNG bajo la carpeta _public_, por ejemplo:

```
arn:aws:s3:::bucket/public/*.png
```

Guardamos los cambios.

Ahora, en la URL debemos incluir _public_.

Así damos acceso solo a esos archivos.

---

Ahora compartiré un dato interesante.  
Tal vez no es súper importante, pero es bueno saberlo.

Aunque en la interfaz aparece como si existieran **carpetas**, en realidad **S3 no tiene carpetas reales**.

El UI da la ilusión de una estructura jerárquica, pero eso no existe como tal.

Funciona así:

- Antes, el nombre del archivo era: `01.png`.
    
- Cuando movimos los archivos a la carpeta _public_, Amazon simplemente **cambió el nombre del archivo** a:
    

```
public/01.png
```

Eso es todo.

Así es como se almacena realmente.  
La “carpeta” es simplemente parte del **key** del objeto.  
La key completa del archivo ahora es:

```
public/01.png
```

Esto demuestra que **S3 no es almacenamiento de archivos**, sino **almacenamiento de objetos**.

---

Ahora regresamos a _Permissions_, revertimos la política, guardamos los cambios y terminamos esta lección.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento explica cómo funcionan las “carpetas” en S3, cómo mover objetos dentro de ellas y cómo ajustar políticas de acceso para esos nuevos paths. También aclara la diferencia entre almacenamiento de objetos y almacenamiento de archivos tradicional.

---

## ⭐ 1. Crear carpetas en S3 (pero no son carpetas reales)

- En la consola de S3 puedes crear carpetas como _public_.
    
- Esto ayuda a **organizar visualmente** los objetos.
    
- Sin embargo, **no existen carpetas reales** en S3.
    
- S3 solo manipula **keys**, y las carpetas son una convención visual del UI.
    

Ejemplo real del objeto:

- Antes: `01.png`
    
- Después de moverlo: `public/01.png`
    
- _public/_ es solo parte del nombre del archivo.
    

---

## ⭐ 2. Mover archivos dentro de “carpetas”

- Seleccionas los objetos y eliges **Move**.
    
- Defines la carpeta destino (o incluso otro bucket).
    
- Una vez movidos, la URL anterior deja de funcionar.
    
- Debes incluir el nuevo path:  
    `bucket/public/archivo.png`.
    

---

## ⭐ 3. Actualizar bucket policies

Si deseas dar acceso público **solo** a archivos en la carpeta _public_, debes cambiar el ARN en la bucket policy:

Antes (todo el bucket):

```
arn:aws:s3:::bucket/*
```

Después (solo public/*.png):

```
arn:aws:s3:::bucket/public/*.png
```

Tanto los ARNs como las URLs dependen del _key_ completo.

---

## ⭐ 4. S3 es almacenamiento de objetos, no de archivos

- No existen carpetas reales.
    
- No existe un sistema jerárquico real.
    
- Las “carpetas” son solo una representación visual.
    
- El almacenamiento se basa en **keys individuales**, no en rutas de un filesystem.
    

---

# 🎯 **Idea principal**

Mover objetos en S3 cambia **su key**, y por lo tanto, cambia toda su ruta, ARN y URL.  
Las carpetas son simplemente prefijos en los nombres de los objetos.  
Por ello, cualquier política o permiso debe actualizarse para reflejar estos nuevos paths.

---

Si quieres, también puedo:

✅ generar la bucket policy final limpia  
✅ darte un ejemplo de cómo mover archivos desde Java/Spring Boot usando el SDK  
✅ crear un diagrama visual del funcionamiento de keys en S3  
✅ preparar preguntas tipo entrevista sobre S3 y almacenamiento de objetos