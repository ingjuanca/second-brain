
---

Volvamos a la consola de AWS.

En lugar de conectarnos desde nuestra máquina local usando la terminal, existe otra opción relativamente nueva. Podemos hacer clic en **Connect** y usar **EC2 Instance Connect**. También podemos usar esta opción para conectarnos a la máquina. Tendrás un shell dentro del navegador, lo cual es genial.

Haz clic en **Connect** y ya estarás conectado a la máquina por SSH desde el navegador.

Puedes ejecutar cualquier comando. Por ejemplo:

```
hostname
```

Luego probamos si Docker está instalado. La máquina no sabe qué es Docker. Probamos con Java y tampoco existe.

Vamos a instalar Docker.  
Limpio la pantalla con **Ctrl + L**.

Ejecutamos:

```
sudo yum install docker
```

Aceptamos con “y” y docker se instalará.

Limpio de nuevo la pantalla.

Si ejecuto:

```
docker version
```

Muestra solo la versión del cliente porque el servicio Docker no está activo por defecto.

Lo iniciamos:

```
sudo systemctl start docker.service
```

Luego probamos:

```
sudo docker version
```

Ahora muestra tanto cliente como servidor.

Probamos correr un contenedor simple:

```
sudo docker run hello-world
```

Se descargará y ejecutará el contenedor rápidamente.

Limpiamos la pantalla nuevamente.

Ahora ejecutemos **Nginx**:

```
sudo docker run -p 80:80 nginx
```

Esto mapea el puerto 80 del contenedor al puerto 80 de la máquina, ya que Nginx escucha en el puerto 80.

La imagen se descargará y el contenedor se iniciará.

Como nuestra máquina tiene una IP pública, deberíamos poder acceder a Nginx desde el navegador usando esa IP. Copio la IP pública, voy al navegador, la pego, presiono enter… y no funciona.

¿Por qué?

Porque en el _security group_ solo permitimos tráfico entrante por el puerto **22**. No estamos permitiendo el puerto **80**, por eso nadie puede acceder al servidor Nginx, incluso teniendo la IP.

Para corregirlo:

1. Vamos a la instancia → pestaña **Security**.
    
2. Abrimos el security group correspondiente.
    
3. Editamos las **inbound rules**.
    
4. Agregamos una regla HTTP (puerto 80), con origen “Anywhere (0.0.0.0/0)”.
    

Guardamos los cambios.

Ahora volvemos al navegador, copiamos la IP nuevamente, presionamos enter… ¡y funciona!  
Lo increíble es que no hubo necesidad de reiniciar nada: el cambio se aplica inmediatamente.

Terminamos.

Podemos cerrar el panel y detener la instancia.  
Ve a **Instance state → Terminate instance**.  
Una vez terminada, AWS dejará de cobrar. Puede tomar unos minutos para que la instancia desaparezca por completo de la lista. Si quieres ver solo las instancias en estado “running”, filtra la vista.

---

# 📝 **RESUMEN COMPLETO DEL DOCUMENTO (VERSIÓN PROFESIONAL)**

El documento muestra cómo conectarse a una instancia EC2 usando **EC2 Instance Connect**, instalar Docker, ejecutar contenedores (hello-world y Nginx) y configurar reglas de firewall para permitir acceso externo.

---

## 🔹 **1. Conexión sin SSH local (EC2 Instance Connect)**

AWS permite conectarse a una instancia EC2 directamente desde el navegador.  
Ventajas:

- No necesitas terminal local.
    
- No necesitas archivo PEM.
    
- Es rápido y funciona en casi todos los ambientes.
    

---

## 🔹 **2. Instalación de Docker en EC2**

Comandos ejecutados:

1. Instalar Docker:
    
    ```
    sudo yum install docker
    ```
    
2. Iniciar el servicio:
    
    ```
    sudo systemctl start docker.service
    ```
    
3. Confirmar versión:
    
    ```
    sudo docker version
    ```
    

Luego se validó la instalación ejecutando:

```
sudo docker run hello-world
```

---

## 🔹 **3. Ejecutar un servidor Nginx con Docker**

Comando utilizado:

```
sudo docker run -p 80:80 nginx
```

Esto descarga la imagen y ejecuta un servidor web básico exponiendo el puerto 80.

---

## 🔹 **4. El problema del puerto 80**

Aunque el contenedor estaba funcionando, no se podía acceder desde el navegador debido a las reglas del **Security Group**.

Por defecto, solo estaba abierto el puerto 22 (SSH).  
Solución:

- Editar reglas de entrada (**Inbound Rules**).
    
- Agregar regla HTTP (puerto 80).
    
- Permitir tráfico desde Anywhere (0.0.0.0/0).
    

Una vez hecho, la página de Nginx se muestra inmediatamente.

---

## 🔹 **5. Finalización del ejercicio**

La instancia se termina para evitar cargos innecesarios:

**Instance State → Terminate**  
AWS dejará de cobrar en cuanto la instancia pase a estado terminado.

---

## ⭐ **Idea principal**

Este documento enseña cómo usar EC2 Instance Connect, instalar Docker, correr contenedores y administrar reglas de firewall en AWS. También refuerza la importancia del Security Group para habilitar el acceso a servicios expuestos por la instancia.

---

Si quieres, puedo también:  
✅ resumirlo en 10 líneas,  
✅ generar una guía tipo “cheat sheet”,  
✅ preparar un paso a paso para documentarlo,  
o  
✅ ayudarte a convertirlo en notas de estudio para tu curso.