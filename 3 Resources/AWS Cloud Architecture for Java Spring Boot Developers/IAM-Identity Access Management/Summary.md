
---

Hagamos un resumen rápido de todo lo que hemos discutido hasta ahora en esta sección.

**IAM** nos ayuda a administrar usuarios y sus niveles de acceso.  
También nos ayuda a controlar el acceso **entre servicios** (service-to-service).

Existen **cuatro componentes principales**:

- **Policies**
    
- **Users**
    
- **User Groups**
    
- **Roles**
    

Si estás planeando tomar una **certificación de AWS**, esto es **muy importante**.

---

### Policies (Políticas)

Una policy es un **conjunto de permisos**, definido en formato **JSON**.

Como vimos, existen:

- Reglas **ALLOW**
    
- Reglas **DENY**
    

Con estas reglas podemos proporcionar **accesos granulares**.

---

### Users (Usuarios)

Los usuarios representan a:

- Empleados
    
- Compañeros de trabajo
    
- Personas reales
    

Es posible adjuntar policies directamente a los usuarios,  
pero **no es una práctica recomendada**.

---

### User Groups (Grupos de usuarios)

La forma ideal es:

1. Crear **grupos**
    
2. Adjuntar las **policies al grupo**
    
3. Agregar usuarios al grupo
    

De esta manera, los usuarios **heredan automáticamente** los permisos del grupo.

---

### Roles (Roles)

Los roles se utilizan principalmente para la **comunicación entre servicios**.

También es posible asignar un rol a alguien de **otra cuenta de AWS**,  
pero ese escenario no se cubre en esta sección.

En este curso, usaremos los roles **principalmente para service-to-service communication**, lo cual es un punto clave que debes recordar.

---

### Principio de menor privilegio

Siempre debes seguir el **principio de menor privilegio**:

- No otorgar más permisos de los necesarios
    
- Dar exactamente los permisos requeridos
    
- Nada más que eso
    

Esto aplica tanto para:

- User Groups
    
- IAM Roles
    
- Policies
    

---

### Cuenta Root

La cuenta **root**:

- ❌ No debe usarse para tareas diarias
    
- ✅ Debe tener **Multi-Factor Authentication (MFA)** habilitado
    
- ✅ Debe mantenerse segura
    

Se debe crear **otro usuario IAM** con permisos de administrador y usar ese usuario para el trabajo diario.

---

### Access Keys

Las access keys:

- Se pueden usar para desarrollo local
    
- **Nunca deben estar en el código**
    
- No deben subirse a repositorios
    

Es muy importante:

- **Rotar las access keys con frecuencia**
    
- **Eliminar las keys que no se usan**
    

Siempre puedes crear nuevas access keys cuando las necesites.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento resume los **conceptos fundamentales de AWS IAM**, destacando las mejores prácticas de seguridad y administración de accesos.

---

## ⭐ 1. Qué resuelve IAM

- Controla **quién puede hacer qué** en AWS
    
- Gestiona accesos de usuarios humanos y servicios
    
- Permite comunicación segura entre servicios AWS
    

---

## ⭐ 2. Componentes clave

- **Policies** → reglas de permisos (ALLOW / DENY)
    
- **Users** → personas reales
    
- **User Groups** → agrupación de usuarios con permisos comunes
    
- **Roles** → permisos temporales, principalmente para servicios
    

---

## ⭐ 3. Buenas prácticas esenciales

- Asignar permisos a **grupos**, no a usuarios individuales
    
- Usar **roles** para service-to-service communication
    
- Aplicar siempre el **principio de menor privilegio**
    
- Usar **DENY explícitos** cuando sea necesario
    

---

## ⭐ 4. Seguridad de la cuenta

- No usar la cuenta **root** para tareas diarias
    
- Proteger root con **MFA**
    
- Usar un usuario administrador IAM para el día a día
    

---

## ⭐ 5. Manejo seguro de Access Keys

- Solo para desarrollo local
    
- Nunca en el código
    
- Rotarlas periódicamente
    
- Eliminar las que no se usan
    

---

# 🎯 **Idea principal**

**IAM es la base de la seguridad en AWS.**  
Un uso correcto de users, groups, roles y policies —aplicando menor privilegio y buenas prácticas— permite operar AWS de forma segura, escalable y alineada con estándares profesionales y de certificación.

---

Si quieres, puedo ayudarte a:

✅ Convertir este resumen en **notas de estudio para certificación AWS**  
✅ Crear un **modelo IAM real para una empresa**  
✅ Preparar **preguntas de entrevista sobre IAM**  
✅ Diseñar una **arquitectura IAM para microservicios**