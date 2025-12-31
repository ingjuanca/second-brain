
---

Para el desarrollador que trabaja en **Windows**, hemos creado una **access key**.

Usando esa access key, configuramos el archivo local de credenciales de AWS.  
Gracias a eso, ahora podemos **interactuar con los servicios de AWS** desde nuestra máquina local.

Estas credenciales (la access key) tienen **los mismos permisos que el usuario IAM**, por ejemplo acceso completo a S3, etc.

Hasta aquí, todo bien.

Pero ahora pensemos en este escenario:  
¿qué pasa si **perdemos estas credenciales por accidente**?

En ese caso, **cualquier persona** que obtenga esas credenciales podría interactuar con los servicios de AWS bajo nuestra cuenta:

- Podrían borrar buckets
    
- Leer archivos
    
- Modificar recursos
    
- Generar costos
    

Si crees que las credenciales se perdieron o fueron robadas, lo que debes hacer es:

- Ir a la consola de **IAM**
    
- Buscar el usuario
    
- Desplazarte hasta la sección de **Access Keys**
    
- **Desactivar o eliminar** esa access key
    

Y listo.  
Desde ese momento, esa access key **ya no funcionará**.

Por esta razón, lo ideal es:

- **Rotar las access keys periódicamente**
    
- Si una key ya no se usa, **eliminarla**
    

Siempre puedes crear una **nueva access key** cuando la necesites, no hay ningún problema.

De hecho, al momento de grabar esta lección, el instructor indica que **él mismo habría eliminado esa access key** después de terminar la demostración.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento explica las **mejores prácticas de seguridad** relacionadas con el uso de **Access Keys** en AWS, especialmente para desarrollo local.

---

## ⭐ 1. Riesgo principal de las Access Keys

- Una access key tiene **los mismos permisos que el usuario IAM**.
    
- Si se filtra o se pierde:
    
    - Un atacante puede operar servicios AWS
        
    - Borrar datos
        
    - Acceder a información sensible
        
    - Generar costos elevados
        

---

## ⭐ 2. Respuesta ante compromiso de credenciales

Si existe sospecha de filtración:

- **Desactivar inmediatamente** la access key
    
- O **eliminarla por completo**
    

Desde ese momento, la key deja de funcionar.

---

## ⭐ 3. Rotación de Access Keys

- Las access keys **no deben ser permanentes**
    
- Se deben **rotar periódicamente**
    
- Keys que no se usan → **se eliminan**
    

AWS permite crear nuevas access keys sin problemas.

---

## ⭐ 4. Buenas prácticas recomendadas

- Usar access keys **solo para desarrollo local**
    
- Nunca subirlas a repositorios (GitHub, GitLab, etc.)
    
- Eliminarlas después de usarlas
    
- En producción, usar **IAM Roles**, no access keys
    
- Aplicar siempre el **principio de menor privilegio**
    

---

# 🎯 **Idea principal**

Las **Access Keys son poderosas pero peligrosas** si no se gestionan correctamente.  
Deben tratarse como secretos críticos: **rotarse, eliminarse cuando no se usan y reemplazarse por IAM Roles en producción**.

Un buen manejo de access keys es clave para evitar incidentes de seguridad y costos inesperados en AWS.

