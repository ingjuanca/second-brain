
---

Hablemos de los **Security Groups** en esta lección.  
Esto es **muy importante**.  
Habrá algunas diapositivas con teoría después de esto.

Vamos al demo.

Ya mencionamos que los _security groups_ son simplemente **reglas de firewall**.  
Por defecto, **todas las solicitudes entrantes (inbound)** están **denegadas**.  
Si quieres abrir puertos como 22, 80, etc., debemos agregarlos explícitamente en el security group para permitir ese tráfico entrante.

Esto es lo que hicimos antes.  
Y **no existen reglas de “deny”**.  
Por ejemplo, no puedes decir: “Quiero abrir el puerto 80 para todos excepto para una IP”.  
Eso no es posible con security groups.

Si necesitas bloquear una IP específica, eso debe manejarse usando algo llamado **NACL (Network Access Control List)**.

Un security group puede estar asociado a múltiples instancias EC2.  
Una instancia EC2 también puede tener múltiples security groups.  
Estas dos afirmaciones pueden sonar confusas, pero en realidad es simple.

Ejemplo:

- Tienes dos instancias EC2.
    
- Tienes dos security groups: uno permite el puerto 22, otro permite el puerto 80.
    

Puedes asignarlos así:

- Un security group puede aplicarse a varias instancias.
    
- Una instancia puede tener varios security groups.
    

A veces la gente se confunde:  
**¿Qué pasa si asigno dos security groups a una instancia y tienen reglas “conflictivas”?**

No pueden existir conflictos porque los security groups **solo tienen reglas de ALLOW**, nunca reglas de deny.

Ejemplos:

- SG1 dice: permitir 22
    
- SG2 dice: permitir 80  
    → Resultado: la instancia permite **22 y 80**.
    

Si ambos dicen: permitir 80  
→ Resultado: simplemente permitir 80, nada conflictivo.

Si uno dijera “permitir 22” y otro “denegar 22”, entonces sí habría conflicto,  
**pero como los security groups no tienen deny, eso nunca ocurre**.

Si aún resulta confuso, en el demo se aclara más; si después sigue siendo confuso, puedes preguntar en la sección Q&A.

---

## Reglas inbound

Las reglas “allow” requieren:

- Un **puerto**
    
- Un **origen** (source IP o rango CIDR)
    

¿Qué pasa si no conozco el CIDR o la IP puede cambiar?  
Las IPs cambian, no son confiables.  
Necesitamos un mejor mecanismo para tener reglas de firewall más seguras para nuestra aplicación.

Eso es lo que veremos.

---

## Demostración: creación de un SG y arquitectura básica

Vamos a crear un security group llamado **SSH-22** que permitirá el puerto 22.  
Lo usaremos en múltiples máquinas EC2.

Crearemos **tres instancias EC2** utilizando la AMI que generamos previamente:

- **client**
    
- **app**
    
- **db**
    

Adjuntaremos el security group SSH-22 a todas, para poder hacer SSH.

La idea es:

- En la instancia **db** ejecutaremos PostgreSQL en el puerto 5432.
    
- Queremos que SOLO las instancias **app** puedan conectarse al DB.
    
- NO queremos que el cliente se conecte directamente a la base de datos.
    

Eso es lo que aprenderemos a configurar.

---

## Demo en la consola AWS

Ir a **EC2 → Security Groups**.

Primero eliminar un SG existente.  
Vemos que el “default” no puede eliminarse (se explicará cuando veamos VPCs).

Creamos un nuevo SG:

- Name: **SSH 22**
    
- Descripción opcional
    
- Reglas inbound:
    
    - Tipo: SSH
        
    - Puerto: 22
        
    - Source: Anywhere (0.0.0.0/0)
        

Si quisiéramos abrir un puerto personalizado, como 8086, usaríamos:

- Custom TCP → 8086
    

Creamos el security group.

---

# 📝 **RESUMEN COMPLETO DEL DOCUMENTO (VERSIÓN PROFESIONAL)**

El documento explica el funcionamiento esencial de **Security Groups (SG)** en AWS y cómo crear reglas adecuadas para controlar el tráfico hacia instancias EC2.

---

## ⭐ 1. ¿Qué es un Security Group?

Un Security Group es un **firewall virtual** que controla el tráfico **entrante y saliente** de una instancia EC2.

Principios fundamentales:

- Por defecto, **todo el inbound está bloqueado**.
    
- Solo existen reglas **ALLOW**, no existen reglas DENY.
    
- Para habilitar un puerto se debe agregar explícitamente una regla.
    
- No puedes permitir “todos excepto X IP”.
    
- Para reglas con deny se usa un NACL (Network ACL).
    

---

## ⭐ 2. Asociación de Security Groups

- Un SG puede estar asociado a **varias instancias**.
    
- Una instancia EC2 puede tener **varios SG** asociados.
    
- No puede haber conflictos porque solo existen reglas “allow”.
    

Ejemplos:

|SG1|SG2|Resultado|
|---|---|---|
|allow 22|allow 80|permite 22 y 80|
|allow 80|allow 80|permite 80|
|allow 22|deny 22|imposible (no existen deny)|

---

## ⭐ 3. Concepto de reglas inbound

Una regla inbound necesita:

- Un puerto (ej. 22, 80, 5432)
    
- Un origen (IP, rango CIDR, otro SG)
    

Problema: las IP cambian.  
Solución: usar mecanismos más avanzados (se verá en próximas clases).

---

## ⭐ 4. Demo práctico realizado

1. Crear SG llamado **SSH 22** que permite acceso SSH desde cualquier origen.
    
2. Crear tres instancias usando la AMI personalizada:
    
    - client
        
    - app
        
    - db
        
3. Adjuntar el SG a todas para permitir SSH.
    
4. Objetivo de arquitectura:
    
    - DB solo debe aceptar conexiones desde “app”, no desde “client”.
        
    - Esto se logrará usando reglas basadas en _security groups_.
        

---

## ⭐ 5. Importancia del Security Group “default”

- El SG default no puede eliminarse.
    
- Tiene un rol clave dentro de la VPC (se explicará más adelante).
    

---

## 🎯 **Idea principal del documento**

El documento enseña cómo funcionan los Security Groups, por qué solo tienen reglas allow, cómo se asocian a instancias y cómo construir reglas de acceso más seguras. Además prepara el escenario para una arquitectura donde distintas instancias (client, app, db) tienen control de comunicación estricta usando SGs en lugar de IPs.
