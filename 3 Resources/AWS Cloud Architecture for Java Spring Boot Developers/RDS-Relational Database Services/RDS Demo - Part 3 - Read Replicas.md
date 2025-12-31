
---

Finalmente, mi **read replica** ya está lista.

Como puedes ver, esta instancia se encuentra en una **Availability Zone diferente**, pero dentro de la **misma región**.

Perfecto.

Si hago clic sobre ella, verás que efectivamente es una **read replica**.

Ahora, observa que tiene un **endpoint diferente** al de la base de datos principal.

Perfecto.

Lo que vamos a hacer ahora es lo siguiente:

Tengo una shell abierta que ya está conectada a la **instancia principal de la base de datos (writer)**.  
La dejaremos así.

Abramos una segunda shell en la **misma instancia EC2**.

Seguimos usando el mismo usuario (`ec2-user`).

Ahora, en esta nueva shell, nos conectaremos a la base de datos usando el **endpoint de la read replica**.

El usuario es el mismo y la contraseña también.

Ahora estamos conectados a la **read replica**.

Si ejecutamos las mismas consultas que antes, por ejemplo listar las bases de datos, veremos que **mydb** también existe aquí.

Nos conectamos a `mydb`.

Ejecutamos:

```sql
select * from customer;
```

Podemos ver los dos registros existentes en la read replica.

Ahora intentemos insertar un nuevo registro:

```sql
insert into customer(name) values ('Jake');
```

Al presionar Enter, obtenemos un error indicando que **no se puede ejecutar un INSERT en una transacción de solo lectura**.

Esto es lo esperado.

Ahora copiamos el mismo INSERT y lo ejecutamos en la conexión a la **base de datos principal**.

Aquí sí se ejecuta correctamente.

Si ahora consultamos nuevamente desde la read replica, veremos que el nuevo registro (**Jake**) ya aparece.

Esto es interesante:

Tenemos **dos instancias de base de datos distintas**, en **Availability Zones diferentes**, pero tan pronto como insertamos datos en la instancia principal, **los cambios se reflejan inmediatamente en la read replica**.

En este punto, ya entendimos cómo funciona.

---

### Snapshots

Ahora vamos a tomar un **snapshot manual**.

Vamos a la sección de mantenimiento y backups de la base de datos principal.

Creamos un snapshot y le damos cualquier nombre.

Este proceso toma un par de minutos.

Una vez finalizado, el snapshot aparece como disponible.

---

### Eliminación de recursos

Ahora vamos a eliminar las bases de datos.

Primero, eliminamos la base de datos principal.

AWS pregunta:

- Si queremos tomar un snapshot final → decimos que no.
    
- Si queremos conservar los backups automáticos → decimos que no.
    
- Confirmamos la eliminación.
    

Luego eliminamos la **read replica**.

No tomamos snapshot adicional y confirmamos la eliminación.

Ambas eliminaciones toman algunos minutos, así que debemos esperar.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento demuestra el funcionamiento real de **Read Replicas en Amazon RDS**, mostrando cómo se conectan, qué operaciones permiten y cómo se gestionan snapshots y limpieza de recursos.

---

## ⭐ 1. Read Replicas en RDS

- Una read replica:
    
    - Tiene su **propio endpoint**
        
    - Puede estar en otra Availability Zone (o región)
        
    - Está sincronizada con la base de datos principal
        
- Las read replicas:
    
    - **Permiten solo operaciones de lectura**
        
    - No aceptan `INSERT`, `UPDATE` ni `DELETE`
        

---

## ⭐ 2. Validación práctica de solo lectura

- Desde la read replica:
    
    - `SELECT` → funciona
        
    - `INSERT` → falla (solo lectura)
        
- Desde la base de datos principal:
    
    - `INSERT` → funciona
        
    - Los cambios se propagan automáticamente a la read replica
        

Esto confirma la **replicación asíncrona administrada por AWS**.

---

## ⭐ 3. Sincronización entre instancias

- Aunque las instancias están en AZs distintas:
    
    - Los datos se reflejan casi de inmediato
        
    - La aplicación puede usar read replicas para escalar lecturas sin afectar escrituras
        

---

## ⭐ 4. Snapshots

- Se pueden crear **snapshots manuales**
    
- Los snapshots permiten:
    
    - Restaurar la base de datos completa
        
    - Recuperar datos ante errores graves
        

---

## ⭐ 5. Limpieza de recursos

- Eliminación controlada de:
    
    - Base de datos principal
        
    - Read replicas
        
- Opción de:
    
    - Tomar snapshot final
        
    - Conservar o eliminar backups automáticos
        

Esto es importante para **evitar costos innecesarios** en AWS.

---

# 🎯 **Idea principal**

Las **Read Replicas en RDS** permiten escalar aplicaciones con alta carga de lectura de forma segura y sencilla.  
Separan claramente las responsabilidades:

- **Writer** → escrituras
    
- **Readers** → lecturas
    

Además, AWS facilita backups, snapshots y eliminación de recursos, haciendo que la operación sea confiable y manejable incluso en entornos productivos.

---

Si quieres, el siguiente paso natural sería:

✅ Cómo usar **Spring Boot con writer + read replicas**  
✅ Estrategias de routing de lecturas y escrituras  
✅ Comparación clara entre **Multi-AZ vs Read Replicas**  
✅ Checklist final de RDS para producción