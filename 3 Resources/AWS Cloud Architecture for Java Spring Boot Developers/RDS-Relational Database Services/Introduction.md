
---

En esta sección vamos a hablar sobre **RDS**, donde RDS significa _Relational Database Service_ (Servicio de Base de Datos Relacional).

Amazon también ofrece otros servicios para bases de datos **NoSQL**, pero este servicio está orientado a **bases de datos relacionales**.

Actualmente, estos son los motores de base de datos que AWS soporta en este curso.

Como parte de esta sección y las siguientes, utilizaremos el motor **PostgreSQL**.

Sin embargo, si estás usando **MySQL u otro motor**, no debes preocuparte.  
Las funcionalidades que vamos a discutir son prácticamente las mismas.

RDS ofrece características muy similares tanto para PostgreSQL como para MySQL.

RDS es un servicio **completamente administrado por AWS**.  
Básicamente, AWS actúa como tu **administrador de base de datos (DBA)**.

Tu base de datos será **altamente disponible**.

Algunas tareas rutinarias que normalmente realiza un DBA, como:

- Parches (patching)
    
- Actualizaciones de versión
    
- Mantenimiento programado
    
- Backups periódicos
    

todo esto será manejado automáticamente por AWS.

Gracias a los backups, podrás **restaurar tu base de datos** cuando sea necesario.

Con RDS, en cuestión de minutos, tendrás tu **base de datos de producción** funcionando.

---

## Características destacadas de RDS

### **Multi-AZ (Alta disponibilidad)**

AZ significa **Availability Zone** (Zona de Disponibilidad).

Esta funcionalidad se recomienda **siempre para bases de datos de producción**.

Cuando creas una base de datos con Multi-AZ:

- AWS te entrega **un solo endpoint**, por ejemplo:  
    `xyz.rds.amazonaws.com`
    
- Pero detrás de escena existen **dos instancias de base de datos**
    

Una instancia se ejecuta en una zona de disponibilidad y la otra en una AZ distinta, físicamente separada (incluso a decenas de kilómetros).

Tu aplicación usa siempre el **mismo endpoint** para:

- Lecturas
    
- Escrituras
    
- Inserts
    
- Updates
    
- Deletes
    

Todas las operaciones llegan a la instancia principal.

Cada cambio se **sincroniza inmediatamente** con la instancia secundaria.

Si ocurre una falla grave, AWS realiza automáticamente un **failover**, y la instancia secundaria pasa a ser la principal, sin que la aplicación tenga que cambiar nada.

Esta instancia secundaria se conoce como **standby instance**.

El objetivo es evitar caídas prolongadas del sistema por fallas de hardware, como ocurrió históricamente en grandes compañías antes de migrar a la nube.

---

### **Read Replicas (Escalabilidad de lectura)**

La mayoría de las aplicaciones son **read-heavy** (más lecturas que escrituras).

En general:

- 70% lecturas / 30% escrituras
    
- En algunos casos, hasta 90% lecturas (por ejemplo, Twitter)
    

Con **Read Replicas**:

- Existe un **nodo escritor (writer)**
    
- Existen uno o más **nodos de lectura (read replicas)**
    
- La replicación es **asíncrona**
    

Todas las operaciones de escritura (insert, update, delete) van al nodo principal.

Los nodos de lectura reciben los cambios y se usan únicamente para consultas (`SELECT`).

Ventajas:

- Mejor rendimiento
    
- Menor carga en el nodo principal
    
- Escalabilidad horizontal
    

Además, los nodos de lectura pueden estar en **otras regiones**, lo que reduce la latencia para usuarios distribuidos globalmente.

---

Ahora, vamos a empezar a trabajar con RDS.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

## 🔹 ¿Qué es Amazon RDS?

Amazon RDS es un servicio de bases de datos relacionales **totalmente administrado** que soporta motores como PostgreSQL y MySQL, permitiendo crear bases de datos de producción en minutos sin preocuparse por la administración operativa.

---

## 🔹 Beneficios principales

- Alta disponibilidad
    
- Backups automáticos y restauración
    
- Patching y upgrades administrados
    
- Escalabilidad
    
- Menor carga operativa para el equipo técnico
    

---

## 🔹 Multi-AZ (Alta disponibilidad)

- Recomendado para **ambientes productivos**
    
- Un solo endpoint para la aplicación
    
- Replicación **síncrona**
    
- Failover automático
    
- La instancia standby **no se usa para lecturas**
    

Ideal para garantizar continuidad del servicio ante fallas de infraestructura.

---

## 🔹 Read Replicas (Performance y escalabilidad)

- Diseñado para aplicaciones con muchas lecturas
    
- Separación de responsabilidades:
    
    - Writes → nodo principal
        
    - Reads → réplicas
        
- Replicación asíncrona
    
- Réplicas en otras regiones para reducir latencia
    
- Mejora significativa del rendimiento
    

---

# 🎯 **Idea principal**

Amazon RDS simplifica drásticamente la gestión de bases de datos relacionales al ofrecer alta disponibilidad, escalabilidad y mantenimiento automático.  
Multi-AZ protege contra fallas, mientras que Read Replicas permiten escalar aplicaciones modernas con alta carga de lectura, como redes sociales o plataformas globales.
