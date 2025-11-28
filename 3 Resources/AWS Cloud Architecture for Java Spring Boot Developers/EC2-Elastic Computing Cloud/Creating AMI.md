
---

En esta clase, veamos cómo crear nuestra propia **AMI (Amazon Machine Image)**.

Vamos al panel de EC2.  
Asegúrate siempre de estar en tu región.  
Vamos a **Instances**; la instancia sigue ahí, todo bien.

Primero lancemos una nueva instancia. Dale algún nombre; usaré el mismo nombre, pero puedes usar cualquiera.  
Seleccionamos **Amazon Linux**, tipo **t2.micro**, y dejamos todo igual.  
Elige tu _key pair_ (no necesitas crear uno nuevo cada vez).  
Para el security group, podemos usar el que ya creamos previamente.

Lanzamos la instancia.  
Si vas a la lista, puede que no aparezca al principio; refresca.  
Ahora está en estado _pending_. En 1–2 minutos estará lista.

Incluso aunque el estado de los health checks esté inicializando, a veces puedes conectar. Probemos.  
En este caso, sí se puede conectar. Perfecto.

Esta máquina es totalmente nueva, con Amazon Linux.  
No tiene Docker ni Java instalados (como vimos antes).

En lugar de lanzar siempre máquinas nuevas e instalar manualmente todo, podemos hacer esto:

1. Lanzar una VM base.
    
2. Instalar todos los softwares necesarios.
    
3. Crear una **AMI personalizada** a partir de esa VM.
    

De ese modo, cada nueva instancia creada desde esta AMI ya tendrá todo instalado.

Es muy similar al concepto de crear una imagen Docker.

---

### Instalemos el software necesario

Primero:

```
sudo yum update
```

Luego instalamos Java, Docker y PostgreSQL 15:

```
sudo yum install <paquetes>  -y
```

(Los detalles de por qué usaremos PostgreSQL se explicarán después).

Veremos que Java se instaló correctamente.

Ahora iniciamos Docker:

```
sudo systemctl start docker
sudo docker version
```

Docker funciona, pero por defecto no inicia automáticamente al arrancar la instancia.  
Queremos que _sí_ lo haga:

```
sudo systemctl enable docker
```

Luego permitimos ejecutar Docker sin `sudo` agregando permisos al usuario actual:

```
sudo usermod -aG docker $USER
```

Ahora podemos salir:

```
exit
```

---

### Crear la AMI personalizada

Vamos a **Actions → Image and templates → Create image**.

Aquí le damos un nombre a la AMI.  
La opción “No reboot” es mejor dejarla vacía; AWS podría reiniciar la instancia al crear la AMI para garantizar consistencia.

Creamos la imagen.  
Tardará unos minutos.  
Vamos a la sección **AMIs**; la veremos en estado _pending_, luego pasará a _available_.

---

### Crear una nueva instancia desde la AMI

Terminamos y borramos la instancia anterior.

Ahora lanzamos una nueva instancia, pero esta vez desde _My AMIs_, seleccionando la AMI que creamos.

Le damos nombre, dejamos la arquitectura por defecto, usamos el tipo t2.micro, mismo key pair y security group.

Lanzamos.

Cuando la instancia esté corriendo, conectamos:

Notarás que el conector muestra `root`, así que cambiamos a:

```
ec2-user
```

Probamos Java:

```
java -version
```

Funciona.  
Probamos Docker:

```
docker run nginx
```

Funciona sin `sudo`.  
La AMI ya incluye todos los softwares preinstalados.

Esto demuestra cómo las AMIs permiten crear máquinas con configuraciones predefinidas listas para usar.

Finalmente, terminamos la instancia creada.

---

# 📝 **RESUMEN COMPLETO DEL DOCUMENTO (VERSIÓN PROFESIONAL)**

Este documento explica paso a paso cómo crear una **AMI personalizada** en AWS para lanzar instancias EC2 con software preinstalado.

---

## ⭐ **1. ¿Qué es una AMI personalizada?**

Una AMI es una imagen que contiene:

- Sistema operativo
    
- Configuraciones
    
- Paquetes instalados
    
- Servicios preconfigurados
    

Permite lanzar nuevas instancias EC2 listas para usar sin repetir instalaciones manuales.

---

## ⭐ **2. Proceso completo**

### **A) Crear una instancia base**

- Lanzar una EC2 con Amazon Linux.
    
- Usar el key pair y security group existentes.
    
- Conectar por EC2 Instance Connect.
    

### **B) Instalar software necesario**

Se instalan:

- Java 22
    
- Docker
    
- PostgreSQL 15
    

Luego se:

- Inicia Docker manualmente.
    
- Configura Docker para arrancar automáticamente.
    
- Otorgan permisos al usuario para usar Docker sin sudo.
    

### **C) Crear la AMI**

Desde la consola:

**Actions → Image and templates → Create Image**

- Asignar nombre.
    
- No modificar “No reboot”.
    
- Esperar a que la AMI pase a _available_.
    

### **D) Lanzar nuevas instancias desde la AMI**

- Ir a EC2 → Launch Instance → My AMIs.
    
- Seleccionar la AMI recién creada.
    
- Configurar los parámetros habituales (t2.micro, key pair, SG).
    
- Lanzar la instancia.
    

La nueva instancia ya tendrá Java, Docker y PostgreSQL instalados y configurados.

---

## ⭐ **3. Beneficios de usar una AMI personalizada**

- Ahorra tiempo en despliegues repetitivos.
    
- Facilita crear ambientes idénticos para desarrollo o pruebas.
    
- Ideal para equipos grandes o infraestructura automatizada.
    
- Similar al uso de imágenes Docker para contenedores.
    

---

Si quieres, puedo también crear:  
✅ un **diagrama visual del flujo AMI**,  
✅ un **cheat sheet de comandos**,  
✅ una versión **más corta para documentación**,  
o  
✅ una **explicación orientada a microservicios con Spring Boot**.