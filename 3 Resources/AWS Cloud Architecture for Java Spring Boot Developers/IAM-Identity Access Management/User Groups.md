
---

## User Groups (Grupos de Usuarios)

Vamos a trabajar con **user groups**.

En esta lección tengo **dos ventanas del navegador abiertas**, lo cual puede ser un poco confuso, así que presta atención.

Una ventana corresponde a la **cuenta root** y la otra a la **cuenta del desarrollador**.  
Verifica también en tu caso en qué cuenta estás trabajando.

El usuario nuevo (el desarrollador) **no tiene acceso a nada todavía**.  
Queremos agregarlo a un grupo llamado **developers**, pero ese grupo aún no existe, así que vamos a crearlo.

---

### Crear el grupo Developers

Vamos a **User Groups → Create group**.

- Nombre del grupo: **developers**
    

Podríamos agregar el usuario ahora mismo, pero lo haremos después.

Ahora debemos decidir **qué permisos** tendrá este grupo.

Podríamos darle permisos de administrador para probar, pero **recordemos el principio de menor privilegio**, así que no haremos eso.

AWS ofrece muchas políticas administradas (más de 40).  
También podríamos crear políticas personalizadas más finas, si lo necesitamos.

---

### Permisos del grupo Developers

Queremos que el desarrollador **administre instancias EC2**.

Buscamos **EC2** y seleccionamos **EC2 Full Access**.

Con esto, el desarrollador podrá:

- Iniciar instancias
    
- Detener instancias
    
- Administrar EC2
    

Creamos el grupo.

---

### Agregar el usuario al grupo

Entramos al grupo **developers** → **Add users**.

Seleccionamos el usuario **VinceDev** y lo agregamos al grupo.

---

### Probar permisos desde la cuenta del desarrollador

Ahora cambiamos a la ventana del **usuario desarrollador**.

Refrescamos la página.

Entramos a **EC2**:

- Ya no hay errores
    
- Puede ver instancias
    
- Puede lanzar nuevas instancias
    

Creamos una instancia EC2 (dev):

- Nombre: _dev-one_
    
- Tipo: t2.micro
    
- Sin key pair
    
- IP pública habilitada
    
- Security Group por defecto
    
- Todo lo demás por defecto
    

La instancia se lanza correctamente.

El desarrollador puede:

- Ver la IP pública
    
- Administrar la instancia
    

---

### Problema: no puede hacer SSH

Cuando el desarrollador intenta conectarse por **SSH** usando _EC2 Instance Connect_, recibe un error de permisos.

Esto es correcto:

- Tiene permisos de EC2
    
- **Pero no tiene permisos para EC2 Instance Connect**
    

---

### Agregar permiso EC2 Instance Connect

Volvemos a la **cuenta root**.

Entramos al grupo **developers** y **agregamos una nueva policy**:

- **EC2 Instance Connect**
    

Ahora el grupo developers puede:

- Lanzar EC2
    
- Conectarse por SSH vía Instance Connect
    

Volvemos a la cuenta del desarrollador, refrescamos, y ahora sí puede:

- Conectarse por SSH
    
- Ejecutar comandos
    
- Levantar nginx con Docker
    
- Exponer la app (abriendo puerto 80 en el security group)
    

---

### Problema de seguridad: acceso a producción

Desde la cuenta root se lanza otra instancia EC2, simulando **producción** (_prod-one_).

El desarrollador:

- Puede verla
    
- Puede conectarse por SSH
    

Esto **no es deseable**.

Queremos que el desarrollador:

- ✔ Administre instancias
    
- ✔ Haga SSH a **dev**
    
- ❌ NO haga SSH a **prod**
    

---

### Solución: política DENY por recurso específico

Desde la cuenta root:

1. Copiamos el **ARN de la instancia de producción**.
    
2. Vamos a **IAM → User Groups → developers**.
    
3. Creamos una **inline policy personalizada** usando el editor visual.
    
4. Servicio: **EC2 Instance Connect**
    
5. Efecto: **DENY**
    
6. Acciones: todas las acciones de Instance Connect
    
7. Recurso: **ARN de la instancia prod**
    
8. Nombre de la policy: `deny-prod-ssh`
    

Importante:

- Aunque el grupo tenga políticas **ALLOW**,
    
- **DENY siempre tiene prioridad**.
    

---

### Verificación final

Desde la cuenta del desarrollador:

- SSH a instancia **dev** → ✅ funciona
    
- SSH a instancia **prod** → ❌ acceso denegado
    
- Puede detener instancias → ✅ permitido
    

Comportamiento exactamente como lo deseamos.

Finalmente, se detienen todas las instancias.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento demuestra un uso **realista y avanzado de AWS IAM**, combinando **políticas administradas**, **políticas personalizadas** y el uso estratégico de **DENY** para controlar accesos sensibles.

---

## ⭐ 1. User Groups como base de permisos

- Los usuarios **no deben recibir permisos directos**
    
- Los permisos se asignan a **grupos**
    
- Los usuarios heredan permisos al pertenecer a un grupo
    

---

## ⭐ 2. Uso de políticas administradas

- `EC2FullAccess` → administrar instancias
    
- `EC2InstanceConnect` → permitir SSH vía consola
    
- Rápido, seguro y mantenido por AWS
    

---

## ⭐ 3. Problema real: acceso a producción

Un desarrollador:

- Necesita acceso a dev
    
- No debe tener acceso SSH a prod
    

---

## ⭐ 4. Solución profesional: DENY por recurso

- Se crea una **policy personalizada**
    
- Se usa el **ARN del recurso específico**
    
- Se aplica un **DENY explícito**
    
- DENY tiene prioridad sobre cualquier ALLOW
    

Este patrón es **muy común en entornos reales**.

---

## ⭐ 5. Buenas prácticas demostradas

- Principio de menor privilegio
    
- Separación dev / prod
    
- Uso de grupos
    
- Uso de políticas administradas + personalizadas
    
- Control fino por recurso
    
- Seguridad sin bloquear productividad
    

---

# 🎯 **Idea principal**

**IAM no solo sirve para “dar permisos”, sino para diseñar reglas de seguridad inteligentes.**  
Combinando ALLOW generales con DENY específicos, puedes permitir que los equipos trabajen libremente **sin poner en riesgo producción**.

---
