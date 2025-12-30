
---

La base de datos ya está lista.

Hagamos clic sobre ella.

Dentro de la sección de **conectividad y seguridad**, aquí podemos ver el **endpoint** que necesitaremos para conectarnos a la base de datos.

El **puerto es 5432**.

Puedes explorar la sección de **monitoring**.  
Aquí puedes monitorear la base de datos.

En este momento probablemente no veremos mucha información, ya que aún no hemos hecho nada.

También puedes ver:

- Logs
    
- Eventos de la base de datos (por ejemplo, backups automáticos)
    

Si quieres, puedes revisar los **logs de PostgreSQL** haciendo clic y visualizándolos.

En la sección de **configuration**, puedes ver:

- CPU
    
- RAM
    
- Otras características de la instancia
    

En nuestro caso, **Multi-AZ está en “No”**.  
Si hubiéramos seleccionado Multi-AZ, aquí aparecería como **“Yes”**.

Esa sería prácticamente la única diferencia visible.

---

### Backups y mantenimiento

Tenemos habilitados los **backups automáticos**.  
Parece que ya se ha tomado un snapshot automáticamente.

Si lo deseas, también puedes crear **snapshots manuales**.

---

## Conexión desde EC2 a RDS

Ahora vamos a crear una **instancia EC2** y conectarnos a esta base de datos.

Vamos a **EC2 → Security Groups**.

Vemos dos security groups:

- El **default security group** (no se puede eliminar)
    
- El security group que acabamos de crear para RDS
    

Si ves más security groups, probablemente son de demos anteriores y puedes eliminarlos.

Revisamos el **security group de RDS**:

- Eliminamos cualquier regla inbound existente
    
- No dejamos ninguna regla inbound
    

Esto es importante:  
👉 **RDS no debe tener acceso abierto por defecto**

---

### Crear Security Group para la aplicación

Ahora creamos un nuevo **Security Group**, que llamaremos **app SG** (Application Security Group).

Este security group será el que se asigne a la instancia EC2.

Configuración:

- Inbound rule: **SSH**
    
- Este SG será usado solo por la EC2
    

Creamos el security group.

---

### Permitir acceso de EC2 a RDS

Ahora volvemos al **security group de RDS**.

Editamos las reglas inbound y agregamos:

- Tipo: PostgreSQL
    
- Puerto: **5432**
    
- Origen: **app SG**
    

Esto significa:

- Solo las instancias EC2 que tengan asignado **app SG** podrán conectarse a PostgreSQL.
    

Guardamos las reglas.

---

## Lanzar la instancia EC2

Ahora lanzamos una nueva instancia EC2:

- Asignamos un nombre
    
- Seleccionamos la AMI
    
- Tipo de instancia: **t2.micro**
    
- Procedemos sin key pair (solo para la demo)
    
- Seleccionamos **existing security group**
    
- Elegimos **app SG**
    

Todo lo demás queda con valores por defecto.

Lanzamos la instancia.

Ahora esperamos a que la instancia EC2 esté **en estado running**.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento muestra cómo preparar la **conectividad segura entre una instancia EC2 y una base de datos RDS PostgreSQL**, usando correctamente **Security Groups**, sin exponer la base de datos a internet.

---

## ⭐ 1. Información clave de RDS

- El **endpoint** de RDS es el único punto de conexión para la aplicación.
    
- PostgreSQL usa el **puerto 5432**.
    
- Desde la consola se puede monitorear:
    
    - CPU
        
    - RAM
        
    - Logs
        
    - Eventos
        
    - Backups automáticos
        

---

## ⭐ 2. Principio de seguridad aplicado

- RDS **no debe tener reglas inbound abiertas por defecto**.
    
- El acceso se concede únicamente desde recursos autorizados.
    

---

## ⭐ 3. Uso correcto de Security Groups

Se utilizan **dos security groups separados**:

### 🔹 app SG

- Asociado a la instancia EC2
    
- Permite SSH para administración
    

### 🔹 RDS SG

- Asociado a la base de datos
    
- Permite tráfico **PostgreSQL (5432)** únicamente desde **app SG**
    
- No permite acceso desde IPs públicas ni desde internet
    

👉 Este patrón es una **mejor práctica estándar en AWS**.

---

## ⭐ 4. Flujo de conexión

1. EC2 tiene asignado **app SG**
    
2. RDS permite tráfico desde **app SG**
    
3. EC2 puede conectarse a PostgreSQL
    
4. Ningún otro recurso puede acceder a la base de datos
    

---

# 🎯 **Idea principal**

La conexión segura entre EC2 y RDS se logra **usando Security Groups como frontera de confianza**, no direcciones IP.  
Este enfoque evita exposición pública, mejora la seguridad y escala correctamente en entornos reales de producción.
