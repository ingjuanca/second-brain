
---

Hagamos un resumen rápido de lo que hicimos en esta sección.

Como parte de **RDS**, estos son los motores de base de datos que tenemos disponibles.

**RDS es un servicio completamente administrado** y es altamente disponible.

Algunas de las tareas administrativas rutinarias son manejadas por AWS, como:

- Aplicación de parches de software
    
- Mantenimiento programado
    
- Backups periódicos
    

Usando estos backups, deberías poder **restaurar tu base de datos**.

También puedes tener los backups en **otra región**, si lo deseas, como medida adicional de seguridad.

---

### Multi-AZ

Esta es la opción que normalmente elegirás para tu **base de datos en producción**.

Como es habitual:

- Lanzarás una sola instancia de base de datos
    
- Se te proporcionará **un único endpoint**
    

Pero detrás de escena:

- Existe una **instancia primaria**
    
- Existe una **instancia standby**
    

Esto no es algo que veas directamente, pero está ahí.

Usando el endpoint, seguirás enviando tus solicitudes a la base de datos como siempre.

La instancia standby recibe todas las actualizaciones desde la instancia primaria.

Si algo falla, la instancia standby se convierte automáticamente en la primaria.

Perfecto.

---

### Read Replicas

Si tienes una aplicación con **muchas lecturas**, puedes crear **read replicas**.

De esta forma:

- Todas las operaciones de **escritura** se envían a la instancia principal
    
- Las operaciones de **lectura** se envían a las read replicas
    

Esto ayuda a **mejorar el rendimiento** de la aplicación.

En la imagen solo se muestra una read replica, pero puedes tener **muchas más** si lo deseas.

Las read replicas también pueden estar en **otra región**, lo que ayuda a reducir la **latencia de red**.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento resume los conceptos clave aprendidos sobre **Amazon RDS**, destacando su carácter administrado, las opciones de alta disponibilidad y las estrategias de escalabilidad para aplicaciones modernas.

---

## ⭐ 1. Amazon RDS como servicio administrado

- Soporta múltiples motores de base de datos.
    
- Es **altamente disponible**.
    
- AWS se encarga de:
    
    - Parches
        
    - Mantenimiento
        
    - Backups automáticos
        
- Permite restaurar bases de datos a partir de backups.
    
- Soporta **replicación de backups entre regiones**.
    

---

## ⭐ 2. Multi-AZ (Alta disponibilidad)

- Es la **opción recomendada para producción**.
    
- Arquitectura:
    
    - Un endpoint
        
    - Una instancia primaria
        
    - Una instancia standby
        
- Replicación síncrona entre primaria y standby.
    
- Failover automático ante fallos.
    
- La instancia standby **no se usa para lecturas**.
    

---

## ⭐ 3. Read Replicas (Escalabilidad)

- Diseñadas para aplicaciones **read-heavy**.
    
- Permiten:
    
    - Separar lecturas y escrituras
        
    - Mejorar el rendimiento
        
    - Escalar horizontalmente
        
- Pueden existir múltiples read replicas.
    
- Pueden desplegarse en **otras regiones** para reducir latencia.
    

---

# 🎯 **Idea principal**

Amazon RDS simplifica enormemente la operación de bases de datos en la nube al ofrecer **administración automática, alta disponibilidad y escalabilidad**, permitiendo a los equipos enfocarse en la aplicación y no en la infraestructura.

La combinación típica en producción es:

- **Multi-AZ** → resiliencia y disponibilidad
    
- **Read Replicas** → rendimiento y escalabilidad de lecturas
    

---

Si quieres, el siguiente paso ideal sería:

✅ Arquitectura final **RDS + Spring Boot**  
✅ Estrategia recomendada para **producción real**  
✅ Tabla comparativa definitiva: **Single DB vs Multi-AZ vs Read Replicas**  
✅ Checklist final de RDS para entrevistas y proyectos reales