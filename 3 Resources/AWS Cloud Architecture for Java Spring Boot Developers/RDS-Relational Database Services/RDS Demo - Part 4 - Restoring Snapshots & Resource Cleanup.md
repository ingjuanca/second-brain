
---

Las bases de datos han sido eliminadas después de diez minutos.

Ahora vamos a la sección de **snapshots**.

Aquí es donde tengo mi snapshot.

Vamos a ver si podemos **restaurar la base de datos** a partir de este snapshot.

Seleccionamos el snapshot y luego **Actions → Restore snapshot**.

Elegimos:

- Motor: **PostgreSQL**
    
- DB instance identifier: asignamos un nombre (puede ser el mismo u otro, no importa)
    

Seleccionamos una instancia **T class**, ya que es la opción gratuita.

El almacenamiento (20 GB) está bien, no cambiaremos nada.

Usamos la configuración existente:

- VPC
    
- Security Group (no el default, usamos el mismo que antes)
    
- Autenticación por contraseña (todo permanece igual)
    

Seleccionamos **Restore DB instance**.

Este proceso tomará nuevamente **alrededor de 10 minutos**, así que hay que ser pacientes.

Después de unos diez minutos, la base de datos ya está disponible.

Cerramos la shell anterior y nos volvemos a conectar a la base de datos restaurada.

Copiamos el **nuevo endpoint** y lo usamos para conectarnos.

La contraseña sigue siendo la misma.

Ahora estamos conectados.

Ejecutamos la consulta para listar las bases de datos y vemos que **mydb** existe.

Nos conectamos a **mydb**.

Ejecutamos:

```sql
select * from customer;
```

Podemos ver **todos los registros**, exactamente como estaban antes.

Esto es justo lo que queríamos mostrar.

Ahora salimos y procedemos a **eliminar la instancia restaurada**.

Indicamos:

- No tomar snapshot final
    
- No conservar backups
    
- Confirmamos la eliminación
    

Mientras se elimina la instancia, vamos a **snapshots** y eliminamos el snapshot manual.

Si aparece algún error, solo hay que esperar un par de minutos y volver a intentarlo.

Luego vamos a **EC2 instances** y terminamos la instancia EC2 que ya no necesitamos.

Finalmente, vamos a **Security Groups**:

- Eliminamos primero el **security group de RDS**
    
- Luego eliminamos el **security group de la aplicación**
    
- En algunos casos hay que esperar unos minutos antes de que estén disponibles para borrar
    

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento demuestra cómo **restaurar una base de datos RDS desde un snapshot** y cómo realizar una **limpieza completa y correcta de recursos en AWS** para evitar costos innecesarios.

---

## ⭐ 1. Restauración desde Snapshot

- Un snapshot permite recuperar:
    
    - La base de datos
        
    - Las tablas
        
    - Los datos exactamente como estaban
        
- Al restaurar:
    
    - Se crea una **nueva instancia RDS**
        
    - Se obtiene un **nuevo endpoint**
        
    - Las credenciales permanecen iguales
        

Esto es clave para **recuperación ante desastres** y errores humanos.

---

## ⭐ 2. Verificación de datos restaurados

- Se valida la restauración conectándose a RDS
    
- Se confirma que:
    
    - La base de datos `mydb` existe
        
    - Los registros en la tabla `customer` siguen intactos
        

Esto demuestra que los snapshots son **copias completas y confiables**.

---

## ⭐ 3. Eliminación ordenada de recursos

Para evitar costos y conflictos, el orden correcto es:

1. Eliminar la instancia RDS restaurada
    
2. Eliminar snapshots manuales
    
3. Terminar la instancia EC2
    
4. Eliminar Security Groups (primero RDS, luego app SG)
    

---

## ⭐ 4. Buenas prácticas implícitas

- Los snapshots son ideales para:
    
    - Backups manuales
        
    - Recuperación ante fallos
        
    - Pruebas y validaciones
        
- La limpieza de recursos es tan importante como su creación
    
- Algunos recursos requieren **esperar unos minutos** antes de poder ser eliminados
    

---

# 🎯 **Idea principal**

Amazon RDS permite **recuperar bases de datos completas a partir de snapshots con mínima configuración**, y AWS ofrece mecanismos claros para eliminar todos los recursos asociados una vez finalizadas las pruebas o el entorno.

Dominar tanto la **restauración** como la **limpieza** es fundamental para trabajar en AWS de forma profesional y responsable en costos.

---
