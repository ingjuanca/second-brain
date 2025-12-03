
---

Resumamos rápidamente todo lo que hicimos en esta sección.

Primero nos familiarizamos con algunas terminologías de nube.  
Estos son conceptos muy básicos que debemos conocer:

- **Región**: una zona geográfica donde desplegaremos nuestras instancias y aplicaciones.
    
- **AZ (Availability Zone)**: es una zona de disponibilidad, esencialmente un centro de datos dentro de una región.  
    AWS tiene al menos dos AZ por región para proporcionar alta disponibilidad.  
    Si algo falla en una AZ, la otra puede seguir operativa.
    
- **Edge Location**: se usa para CDN (Content Delivery Network).  
    Más adelante, al desplegar nuestras aplicaciones usando AZs y edge locations, lo entenderás aún mejor.  
    Al final del curso todo quedará claro.
    

---

### EC2

EC2 significa _Elastic Compute Cloud_.  
Es un servicio que proporciona capacidad de cómputo.  
Como viste, podemos lanzar una máquina virtual en menos de un minuto.  
Hay opciones como Linux, Ubuntu, Mac, Windows, etc.  
Podemos crear nuestras propias AMIs, elegir CPU, memoria, lo que queramos. AWS nos lo provee.

También podemos usar **Auto Scaling**, es decir, escalar automáticamente instancias según la demanda.  
Eso lo cubriremos más adelante cuando despleguemos la aplicación, generemos carga y veas cómo escala automáticamente.

---

### Security Groups

El concepto más importante de esta sección es **Security Groups**.

Los security groups son reglas de firewall que funcionan **a nivel de instancia**.

AWS nos da todas las herramientas, pero nosotros somos responsables de asegurar nuestra aplicación usando esas herramientas.

Por defecto, **todas las solicitudes inbound están denegadas**.  
Cuando creamos un security group, las reglas inbound estarán vacías.  
Debemos agregar **reglas allow una por una**.

Al crear una regla, debemos definir:

- el **puerto** (80, 22, 5432, etc.)
    
- el **origen** (CIDR, una IP de confianza, una prefix list o un security group)
    

No existen reglas de “deny” en los security groups.  
Si quieres bloquear explícitamente una IP, eso se hace con un **Network ACL** (lo veremos en la sección de networking).

Un security group puede estar adjunto a múltiples instancias.  
Y una instancia puede usar múltiples security groups.

---

### Ejemplos prácticos

Si tienes una aplicación y una base de datos, y quieres que **solo la app pueda hablar con la DB**:

- Creas dos SG:
    
    - **db-SG**
        
    - **app-SG**
        
- db-SG permite tráfico desde app-SG
    
- db-SG se adjunta a la instancia DB
    
- app-SG se adjunta a la instancia de la aplicación
    

Así evitamos que otras aplicaciones, como el frontend, puedan conectarse a la DB.

Si tienes múltiples microservicios backend que necesitan comunicarse entre sí:

- Puedes crear **un único security group** con una **regla self-referencing**.
    
- Luego lo adjuntas a todos los microservicios.
    
- Todos podrán comunicarse entre ellos fácilmente.
    

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento resume todos los conceptos fundamentales vistos en esta sección del curso: regiones, AZs, edge locations, EC2 y security groups.

---

## ⭐ 1. Conceptos básicos de nube

- **Región**: área geográfica donde se despliegan recursos.
    
- **Availability Zone (AZ)**: centro de datos independiente dentro de una región. Garantiza alta disponibilidad.
    
- **Edge Location**: puntos distribuidos globalmente usados para CDN y reducción de latencia.
    

---

## ⭐ 2. EC2 (Elastic Compute Cloud)

- Servicio que permite lanzar máquinas virtuales en minutos.
    
- Permite elegir SO, CPU, RAM, almacenamiento, AMIs personalizadas.
    
- Puede integrarse con **Auto Scaling** para ajustar recursos según demanda.
    

---

## ⭐ 3. Security Groups (el tema más importante)

- Actúan como **firewall a nivel de instancia**.
    
- Por defecto **todo inbound está bloqueado**.
    
- Solo existen reglas **ALLOW**, nunca DENY.
    
- Las reglas deben especificar:
    
    - Puerto
        
    - Origen (CIDR, IP, prefix list o security group)
        

Para bloquear IPs se debe usar **Network ACL**, no SGs.

---

## ⭐ 4. Asociación flexible

- Un SG puede aplicarse a varias instancias.
    
- Una instancia puede tener varios SG.
    

Esto permite construir arquitecturas seguras.

---

## ⭐ 5. Patrones comunes de seguridad

### ✔ App → DB

- db-SG permite tráfico desde app-SG
    
- Garantiza que solo la aplicación pueda conectarse a la base de datos
    

### ✔ Microservicios entre sí

- Un solo SG con **self-referencing**
    
- Todos los microservicios pueden comunicarse entre ellos fácilmente
    

---

## 🎯 **Idea clave del documento**

Esta sección enseña los fundamentos esenciales de infraestructura y seguridad en AWS:  
cómo funcionan las regiones, las AZs, EC2 y especialmente cómo controlar el tráfico entre instancias usando security groups y mejores prácticas de arquitectura.

---

Si quieres, también puedo darte:

✅ un resumen ultra corto en 5–7 líneas  
✅ un mapa conceptual de toda esta sección  
✅ explicaciones orientadas a microservicios con Spring Boot  
✅ preguntas tipo entrevista sobre estos conceptos