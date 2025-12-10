
---

En esta lección hablaremos sobre **algunas configuraciones importantes** de S3 que debes conocer.

Para eso, vayamos a crear un bucket.  
En realidad no crearemos un bucket nuevo, pero quiero mostrarte rápidamente estas opciones.

---

## **1. Object Ownership**

La opción que usamos fue la predeterminada.  
Eso es lo que queremos en nuestro caso.

¿Qué significa por defecto?

- Todos los objetos en este bucket **son propiedad de esta cuenta de AWS**.
    
- Es decir, tú creaste el bucket y cualquier objeto que subas (01.png, secret.jpg, etc.) será controlado por **tu** cuenta.
    

En una organización grande hay múltiples cuentas de AWS:

- equipo de desarrollo
    
- equipo DevOps
    
- equipo QA, etc.
    

A veces un equipo quiere permitir que otro equipo sea dueño de los objetos que suban.  
Para esos casos existen configuraciones adicionales.

Esto podría ser confuso, así que no nos preocuparemos por ello ahora.  
Para nuestro caso simple, la opción por defecto es suficiente.

---

## **2. Bucket Versioning**

Imagina un bucket _vince-demo_, y dentro tienes un archivo `file1.txt`.

Tienes una nueva versión de ese archivo con contenido actualizado. Lo subes a S3.

- **Si versioning está deshabilitado** → el archivo viejo es reemplazado.
    
- **Si versioning está habilitado** → S3 mantiene **ambas versiones**.
    

Accedes siempre a la **última versión**, pero si accidentalmente dañaste un archivo o subiste algo erróneo, puedes volver a una versión anterior.

Nada es gratis:  
S3 cobrará por **cada versión** almacenada.

Versioning está deshabilitado por defecto, pero puedes activarlo fácilmente.

---

## **3. Tags**

Nada nuevo: son **pares clave–valor**, igual que en EC2.  
Se usan para organización, facturación, trazabilidad, etc.

---

## **4. Encryption (muy importante)**

Cuando creas un bucket —por ejemplo _vince-demo_— AWS guarda físicamente el contenido en centros de datos, en algún disco duro administrado por Amazon.

Ahora supongamos que subes un archivo altamente sensible como `file1.txt` con datos de clientes, SSN, etc.

¿Cómo confiar en la seguridad si alguien roba ese disco?

Para eso existe la **cifra de lado servidor (SSE)**.

AWS hace lo siguiente:

1. Genera una **llave única** para cada objeto.
    
2. Usa esa llave para **cifrar el archivo**.
    
3. Aunque alguien robe el disco, no podrá leer nada, solo bytes cifrados.
    

En la consola puedes ver el archivo normalmente porque tienes permisos.

Si deseas mayor seguridad, puedes **cifrar tú mismo** el archivo antes de subirlo.

Opciones de cifrado:

- **SSE-S3** (por defecto): llaves manejadas automáticamente por Amazon.
    
- **SSE-KMS (CMK propio)**: tú administras las llaves.
    
- **Doble capa de cifrado** (nuevo): dos llaves distintas; si una es comprometida aún queda la otra como protección.
    

---

## **5. Advanced Settings – Object Lock**

Object Lock permite el modo **WORM: Write Once, Read Many**.

Esto significa:

- Una vez subes un archivo, **no se puede sobrescribir**.
    
- Tampoco se puede **borrar accidentalmente**.
    

Ideal para cumplimiento normativo, logs, auditoría, etc.

---

## **6. Replication (tema avanzado)**

S3 tiene una función de replicación:

- Puedes replicar automáticamente objetos del bucket en **US East 1** a otro bucket en **Singapur**, por ejemplo.
    

Esto se usa para:

- DR (disaster recovery)
    
- Auditorías
    
- Compliance
    
- Multi-región
    

Pero estos son temas avanzados y se cubrirán más adelante.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento repasa configuraciones clave de S3: propiedad de objetos, versioning, encriptación, Object Lock y replicación. Son aspectos fundamentales para seguridad, gobernanza y operación profesional de buckets.

---

## ⭐ 1. Object Ownership

- Controla quién es dueño de los objetos subidos.
    
- Por defecto, **tu propia cuenta** es la dueña de todos los objetos del bucket.
    
- Útil en organizaciones grandes con múltiples cuentas.
    

---

## ⭐ 2. Versioning

- Permite mantener múltiples versiones del mismo archivo.
    
- Con versioning deshabilitado → se sobrescribe el archivo.
    
- Con versioning habilitado → se conservan todas las versiones.
    
- Permite “deshacer” cambios.
    
- Aumenta costos de almacenamiento.
    

---

## ⭐ 3. Tags

- Pares clave/valor usados para organizar, clasificar y controlar costos.
    
- Mismo concepto que en EC2 y otros servicios.
    

---

## ⭐ 4. Encryption (uno de los temas más importantes)

S3 cifra automáticamente los objetos con **llaves manejadas por Amazon (SSE-S3)**.

Beneficios:

- Si alguien roba físicamente un disco del centro de datos → no puede leer el contenido.
    
- Se puede agregar doble cifrado (dos capas).
    
- También se puede usar **SSE-KMS** para administrar llaves propias.
    
- Incluso puedes cifrar tú mismo el archivo antes de subirlo.
    

---

## ⭐ 5. Object Lock

- Permite proteger archivos contra sobrescritura o eliminación.
    
- Ideal para auditoría, logs, cumplimiento regulatorio y preservación de datos.
    

---

## ⭐ 6. Replicación

- Permite copiar objetos automáticamente entre buckets en diferentes regiones.
    
- Funcionalidad avanzada para alta disponibilidad y disaster recovery.
    

---

# 🎯 **Idea principal**

S3 incluye varias configuraciones fundamentales que afectan seguridad, control de versiones, propiedad, y protección de datos. Conocerlas permite usar S3 de forma segura y profesional, más allá del simple almacenamiento.

---

Si quieres, también puedo:

✅ preparar una tabla comparativa de S3 encryption (SSE-S3 vs SSE-KMS vs client-side encryption)  
✅ darte un resumen ultra corto (5–7 líneas)  
✅ crear un checklist de seguridad para S3 para tus proyectos  
✅ convertir este contenido en notas de certificación AWS SAA o Developer Associate