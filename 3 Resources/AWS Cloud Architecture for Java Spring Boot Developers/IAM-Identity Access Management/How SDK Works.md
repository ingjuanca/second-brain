
---

Esta será una lección rápida.

Si te das cuenta, todo nuestro proyecto es **muy simple**.  
Solo tenemos **una clase**, el `main`, los recursos y nada más.  
Básicamente está vacío, no hay nada especial.

Entonces la pregunta es:  
¿cómo es posible que podamos **escribir un archivo en un bucket S3** y luego **leer ese archivo desde el mismo bucket**?

¿Dónde están las credenciales?

¿Cómo es que este código Java funciona correctamente en nuestra máquina local?

La respuesta es que **ya configuramos las credenciales previamente**.

Existe una **ruta estándar** donde esas credenciales están configuradas.  
El AWS SDK **las detecta automáticamente**, las carga y las usa para enviar la solicitud a S3.

Gracias a eso, el SDK puede:

- Enviar la solicitud a S3
    
- Escribir el archivo
    
- Leer el archivo
    

Nuestro proyecto queda **limpio**:

- No mantenemos credenciales en el código
    
- No hay riesgo de subir credenciales a GitHub
    
- No tenemos que preocuparnos por fugas de seguridad
    

Todo simplemente **funciona** en local.

---

### ¿Qué pasa en producción (EC2)?

Ahora imaginemos que empaquetamos esta aplicación y la desplegamos en una **instancia EC2**.

Si recuerdas lo que vimos antes:

- La instancia EC2 tendrá asignado un **IAM Role**
    
- Ese rol tendrá permisos para acceder a S3
    

Entonces, usando ese rol:

- Nuestro código podrá contactar a S3
    
- Podrá leer archivos
    
- Podrá escribir archivos
    

Todo esto **sin cambiar una sola línea de código**.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento explica **cómo el AWS SDK maneja automáticamente las credenciales**, tanto en desarrollo local como en producción, sin necesidad de incluirlas en el código.

---

## ⭐ 1. ¿Dónde están las credenciales?

- En **desarrollo local**:
    
    - Las credenciales están configuradas en rutas estándar (`~/.aws/credentials`)
        
    - El SDK las detecta automáticamente
        
    - El código no necesita saber nada sobre ellas
        
- En **EC2 (producción)**:
    
    - No hay Access Keys locales
        
    - La instancia usa un **IAM Role**
        
    - AWS provee credenciales temporales automáticamente
        

---

## ⭐ 2. Cadena de proveedores de credenciales (Credential Provider Chain)

El AWS SDK sigue un orden interno para buscar credenciales, por ejemplo:

1. Variables de entorno
    
2. Archivos de credenciales locales
    
3. IAM Role asignado a EC2
    
4. Otros mecanismos administrados por AWS
    

Gracias a esto, **el mismo código funciona en todos los entornos**.

---

## ⭐ 3. Beneficios clave del enfoque

- ✅ No hay credenciales hardcodeadas
    
- ✅ No hay riesgo al subir código a GitHub
    
- ✅ El mismo artefacto funciona en local y en producción
    
- ✅ Seguridad alineada con mejores prácticas AWS
    
- ✅ Código más limpio y mantenible
    

---

## 🎯 **Idea principal**

El **AWS SDK abstrae completamente la gestión de credenciales**.  
En local usa Access Keys configuradas en el sistema, y en producción usa IAM Roles asignados a la infraestructura.  
Por eso, **no necesitas cambiar el código al mover tu aplicación de local a EC2**, lo que hace que el desarrollo sea seguro, limpio y profesional.

---

Si quieres, el siguiente paso ideal sería:

✅ Ver la **Credential Provider Chain** en detalle  
✅ Cómo usar esto en **Spring Boot** (profiles, beans, etc.)  
✅ Comparar **Access Keys vs IAM Roles** de forma definitiva  
✅ Preparar este tema para **entrevistas técnicas AWS**