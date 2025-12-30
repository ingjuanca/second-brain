
---

Volvamos a nuestra consola de AWS.

Iniciamos sesión como administrador.

Buscamos **RDS** y lo agregamos a favoritos.

Entramos al **dashboard de RDS** y luego a **DB instances** (instancias de base de datos).

Ahora vamos a **Create database**.

Existen dos formas de crear una base de datos.  
Elegimos la opción **Standard create**, ya que nos permite explorar todas las configuraciones disponibles (la opción “Easy create” simplifica el proceso, pero oculta muchas opciones).

---

### Selección del motor

Actualmente estos son los motores disponibles (podrían ser más en el futuro).

**Aurora** es el motor relacional de Amazon:

- Compatible con PostgreSQL
    
- Compatible con MySQL
    

Para este ejemplo, usamos **PostgreSQL** y seleccionamos la versión más reciente disponible.

---

### Templates y disponibilidad

- **Single DB instance**:  
    Se crea una sola instancia, sin standby.
    
- **Multi-AZ** (alta disponibilidad):  
    Se crea:
    
    - Una instancia primaria
        
    - Una instancia standby en otra Availability Zone
        
    
    AWS entrega **un solo endpoint**.  
    Todas las lecturas y escrituras van al primario.  
    La instancia standby se sincroniza automáticamente.
    
    Si ocurre una falla, AWS realiza **failover automático** y la standby se convierte en primaria.
    
    👉 Importante:
    
    - La instancia standby **no se puede usar para lecturas**
        
    - No se expone un endpoint separado
        
    - Su único objetivo es alta disponibilidad
        
- **Multi-AZ DB Cluster (opción más nueva)**:  
    Permite:
    
    - Un nodo primario
        
    - Dos instancias standby **legibles**  
        Combina alta disponibilidad + capacidad de lectura.
        

Para el **Free Tier**, se usa **Single DB instance**, aunque luego se pueden crear read replicas.

---

### Configuración básica

- **DB Identifier**: nombre de la instancia.
    
- **Master username**: por ejemplo `postgres`.
    
- **Password**:
    
    - Puede generarse automáticamente
        
    - Para la demo se usa una contraseña simple (no recomendado en producción).
        

---

### Configuración de instancia

- Se define CPU y RAM.
    
- En Free Tier se usa la instancia mínima.
    
- En producción se pueden usar instancias con cientos de GB de RAM si es necesario.
    

---

### Almacenamiento

Las operaciones de base de datos generan muchas operaciones de entrada/salida (I/O).

Opciones disponibles:

- Tipos de almacenamiento con diferentes IOPS
    
- Desde ~3.000 IOPS hasta ~256.000 IOPS
    
- El costo varía según la opción elegida
    
- **Allocated storage**: mínimo 20 GB (puede crecer hasta varios TB).
    
- **Auto scaling**:
    
    - Empiezas con 20 GB
        
    - El almacenamiento crece automáticamente a medida que el negocio crece
        

---

### Conectividad y red

- Se selecciona la **VPC por defecto**.
    
- Subnet group por defecto.
    
- **Public access: NO**  
    Nunca se habilita acceso público en producción.
    
- **Security Group**:
    
    - Se crea uno nuevo
        
    - No se usa el default
        
    - Permitirá acceso solo desde recursos autorizados (por ejemplo EC2)
        

---

### Configuración adicional

- **Puerto**: 5432 (PostgreSQL).
    
- **Tags**: opcionales (costos, organización).
    
- **Autenticación**: password.
    
- **Monitoring / Performance Insights**:
    
    - Permite analizar queries lentas y rendimiento.
        
    - Recomendado para producción.
        
- **Initial database name**:
    
    - Opcional
        
    - Si se define, AWS crea la base automáticamente.
        

---

### Backups y mantenimiento

- **Backups automáticos**:
    
    - Retención de 1 a 35 días.
        
    - También se pueden hacer snapshots manuales.
        
- **Backup window**:
    
    - Se puede definir un horario específico
        
    - O dejar que AWS lo maneje automáticamente
        
- **Backup replication**:
    
    - Permite copiar backups a otra región (por ejemplo, producción en Virginia y backups en Singapur).
        
- **Encryption at rest**:
    
    - Se habilita cifrado de datos en reposo.
        
    - Recomendado siempre.
        
- **Maintenance window**:
    
    - AWS aplica parches y upgrades menores.
        
    - Puede definirse una ventana específica o dejar “no preference”.
        

---

Finalmente, se crea la base de datos.

El proceso puede tardar **hasta 10 minutos**.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento describe paso a paso cómo crear una instancia de Amazon RDS usando PostgreSQL, explicando las decisiones clave de arquitectura, disponibilidad, seguridad y costos.

---

## ⭐ 1. Creación controlada de RDS

- Se usa **Standard create** para tener control total.
    
- Se selecciona PostgreSQL como motor.
    
- Se elige Single DB (Free Tier) o Multi-AZ para producción.
    

---

## ⭐ 2. Alta disponibilidad

- **Multi-AZ** crea una instancia primaria y una standby.
    
- Un solo endpoint.
    
- Failover automático.
    
- La standby **no sirve para lecturas**.
    
- Existe una opción más moderna: **Multi-AZ DB Cluster** con nodos legibles.
    

---

## ⭐ 3. Performance y almacenamiento

- Diferentes tipos de storage según IOPS requeridos.
    
- Auto scaling de almacenamiento.
    
- Ideal para crecer sin reprovisionar manualmente.
    

---

## ⭐ 4. Seguridad y red

- Sin acceso público.
    
- Uso de Security Groups dedicados.
    
- Acceso controlado solo desde EC2 autorizadas.
    
- Cifrado de datos en reposo habilitado.
    

---

## ⭐ 5. Backups, monitoreo y mantenimiento

- Backups automáticos (hasta 35 días).
    
- Snapshots manuales.
    
- Replicación de backups entre regiones.
    
- Performance Insights para análisis de queries.
    
- Ventanas de mantenimiento configurables.
    

---

# 🎯 **Idea principal**

Amazon RDS permite crear bases de datos relacionales listas para producción en minutos, con alta disponibilidad, backups, seguridad y escalabilidad integradas.  
La clave está en **elegir correctamente Multi-AZ, almacenamiento, red privada y cifrado**, evitando siempre el acceso público.
