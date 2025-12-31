
---

En esta sección vamos a hablar sobre **IAM (Identity and Access Management)**.

Imaginemos que estás iniciando una empresa llamada _My ARG Comm_.

Todas tus aplicaciones están desplegadas bajo **una sola cuenta de AWS**.

Ahora tu negocio empieza a crecer y contratas a **4 o 5 personas** con diferentes roles.

Todos necesitan acceso a la cuenta de AWS para hacer su trabajo correctamente, pero **no todos necesitan el mismo nivel de acceso**.

Como dueño del negocio, podrías estar preocupado por los **costos de infraestructura**.  
Un **arquitecto** estará más interesado en infraestructura, seguridad, rendimiento y escalabilidad.  
Los **desarrolladores y DevOps engineers** trabajarán juntos en despliegues, monitoreo, uso de CPU, etc.  
El **product manager** querrá métricas de negocio como:

- productos en tendencia
    
- productos más clickeados
    
- tasa de conversión
    

Cada uno necesita **diferentes niveles de acceso**.

Aquí es donde **IAM** nos ayuda.

No solo eso.  
Si recuerdas el ejemplo anterior de Udemy, una **instancia EC2** puede necesitar acceso a un **bucket S3** para leer o escribir archivos.

En este caso, **un servicio de AWS necesita acceder a otro servicio de AWS**.

También aquí necesitamos limitar accesos, por ejemplo:

- permitir que una EC2 acceda a un bucket S3 específico
    
- impedir que acceda a buckets con información sensible
    

IAM también resuelve este problema.

---

### Componentes importantes de IAM

Hay **cuatro componentes clave** en IAM que debemos conocer:

- **Policies**
    
- **Users**
    
- **User Groups**
    
- **Roles**
    

---

### Policies (Políticas)

Una **policy** es un conjunto de permisos escrito en formato **JSON**.

Es el mismo concepto que una **S3 Bucket Policy**.

Si el formato parece confuso, no te preocupes:  
normalmente **no escribimos estas políticas a mano**.

Las policies definen cosas como:

- quién puede iniciar instancias EC2
    
- quién puede detenerlas
    
- quién no puede hacerlo, etc.
    

---

### Users (Usuarios)

Los **usuarios** son personas reales:

- compañeros de trabajo
    
- empleados
    
- miembros del equipo
    

---

### User Groups (Grupos de usuarios)

Los **user groups** representan equipos, por ejemplo:

- Developers
    
- DevOps Engineers
    

Aunque se pueden asignar permisos directamente a usuarios, **no es una buena práctica**.

La forma correcta es:

1. Crear un **user group** (por ejemplo, _Developers_)
    
2. Crear una **policy**
    
3. Asociar la policy al user group
    
4. Agregar usuarios al grupo
    

Todos los usuarios del grupo **heredan los permisos** definidos en la policy.

Por ejemplo:

- Si la policy permite iniciar instancias EC2 → todos los developers pueden hacerlo
    
- Si no permite reiniciar la base de datos de producción → no podrán hacerlo
    

Cada grupo puede tener su **propia policy**, según sus responsabilidades.

---

### Roles (Roles)

El concepto de **roles** es similar al de user groups, pero puede resultar confuso al inicio.

Un ejemplo en una empresa:

- Un CEO renuncia
    
- Se nombra un CEO interino temporal
    

Eso es un **rol**: algo que se asume por un tiempo.

En AWS:

- No solo los usuarios necesitan permisos
    
- **Los servicios también necesitan permisos**
    

Ejemplo:

- Una instancia **EC2** necesita escribir en **S3**
    

En este caso:

1. Se crea un **IAM Role** (por ejemplo, _S3WriterRole_)
    
2. Ese rol tiene permisos para acceder a S3
    
3. El rol se **asigna a la instancia EC2**
    

Mientras la EC2 tenga el rol:

- Puede acceder a S3
    

Si se elimina el rol:

- Ya no puede acceder
    

👉 Forma simple de entenderlo:

> **Un rol es la manera de permitir que un servicio de AWS acceda a otro servicio de AWS**

Los roles también pueden ser asumidos temporalmente por humanos, pero ese caso se deja fuera por ahora.

---

### Características importantes de IAM

- **Gestión centralizada de accesos**  
    Permite crear y administrar usuarios sin compartir credenciales del usuario root.
    
- **Permisos granulares**  
    Se puede controlar exactamente qué acciones puede realizar cada usuario o servicio.
    
- **Principio de menor privilegio**  
    AWS recomienda otorgar solo los permisos mínimos necesarios para realizar una tarea.
    
- **Integración con Active Directory / SSO**  
    Puedes usar el login corporativo para acceder a AWS.
    
- **Soporte para MFA (Multi-Factor Authentication)**  
    Agrega una capa adicional de seguridad.
    

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento introduce **AWS IAM**, el servicio encargado de controlar **quién puede hacer qué dentro de una cuenta de AWS**, tanto para usuarios humanos como para servicios.

---

## ⭐ 1. ¿Por qué existe IAM?

- En una empresa hay múltiples roles con distintas responsabilidades.
    
- No todos deben tener acceso total a la infraestructura.
    
- IAM permite **controlar costos, seguridad y operaciones**.
    

---

## ⭐ 2. Componentes clave de IAM

- **Users** → personas reales
    
- **User Groups** → equipos (developers, DevOps, etc.)
    
- **Policies** → reglas de permisos en formato JSON
    
- **Roles** → permisos temporales, especialmente para servicios AWS
    

---

## ⭐ 3. Buenas prácticas destacadas

- Nunca compartir credenciales del usuario root
    
- Asignar permisos a **grupos**, no a usuarios individuales
    
- Usar **roles** para comunicación entre servicios (EC2 → S3)
    
- Aplicar siempre el **principio de menor privilegio**
    
- Habilitar **MFA** para mayor seguridad
    

---

## 🎯 **Idea principal**

**IAM es la base de la seguridad en AWS**.  
Permite otorgar accesos controlados, seguros y auditables tanto a personas como a servicios, evitando accesos innecesarios y reduciendo riesgos operativos y de seguridad.

---

Si quieres, el siguiente paso ideal sería:

✅ Demo paso a paso creando **Users, Groups y Policies**  
✅ Ejemplos reales de **IAM Roles para EC2 + S3**  
✅ Cómo usar IAM con **Spring Boot en AWS**  
✅ Preguntas típicas de entrevista sobre **IAM**