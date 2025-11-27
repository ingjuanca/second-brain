
---

En esta clase vamos a conectarnos por SSH a la instancia EC2 que acabamos de lanzar.

Si tienes algún problema con esta clase, no te preocupes: en la siguiente clase veremos una opción mejor que funcionará para todos. Pero por ahora intentaremos conectarnos desde nuestra máquina local.

Lo primero que necesitamos es la **dirección IP pública**, que vimos en la consola de AWS. Asegúrate de usar la IP pública, no la privada.

Primero hablemos de los usuarios Windows.  
Usuarios de Linux/Mac, por favor esperen un momento; terminamos rápido con Windows.

Para Windows necesitas el software **PuTTY**.  
Si no lo tienes, ve a _putty.org_ y descárgalo. Es muy simple. No requiere instalación; descargas el .exe y lo abres.

Una vez abierto, coloca tu **IP pública** en “Host Name”. Muy simple.

Luego necesitamos dos datos más: **usuario** y **clave privada**.

El usuario lo defines en la opción _Connection → Data_ como “Auto-login username”. El usuario para todos será:

```
ec2-user
```

Luego, en _Connection → SSH → Auth_, verás donde cargar un archivo. Ahí debes seleccionar tu archivo **.ppk**, el que descargaste cuando creaste tu key pair. Cárgalo.

Entonces en resumen:

1. IP pública
    
2. Usuario: `ec2-user`
    
3. Archivo `.ppk` en la sección Auth
    

Haz clic en "Open".  
Te aparecerá un mensaje preguntando si confías en este host. Acepta.

---

### Para usuarios Linux / Mac

Ahora conectémonos por SSH desde Linux/Mac.

Ve al directorio donde guardaste tu archivo `.pem`.

El primer paso obligatorio es:

```
chmod 400 nombre-del-archivo.pem
```

Esto hace la llave privada accesible solo para el dueño. Si no haces esto, no funcionará.

Luego ejecutas:

```
ssh -i nombre-del-archivo.pem ec2-user@IP_PUBLICA
```

Presiona Enter.  
Verás un mensaje preguntando si confías en el host. Acepta.

En ese momento ya estarás conectado a la instancia EC2 remota.

Esta clase solo muestra esta opción para quien quiera conectarse desde su máquina local. En la siguiente clase veremos la otra opción, que será la principal que usaremos en este curso.

Para salir de SSH, simplemente ejecuta:

```
exit
```

---

# 📝 **RESUMEN COMPLETO DEL DOCUMENTO (VERSIÓN PROFESIONAL)**

El documento explica paso a paso cómo conectarse por SSH a una instancia EC2 recién lanzada, tanto desde Windows como desde Linux/Mac.

---

## 🔹 **1. Requisito principal**

Debes usar la **IP pública** de la instancia EC2 (no la privada).

---

## 🔹 **2. Conexión desde Windows (PuTTY)**

Windows usa PuTTY porque no soporta archivos `.pem` directamente.

Pasos:

1. Abrir PuTTY.
    
2. Ingresar **IP pública** en _Host Name_.
    
3. Ir a _Connection → Data_ y configurar:
    
    - Usuario: `ec2-user`
        
4. Ir a _Connection → SSH → Auth_ y cargar el archivo **.ppk**.
    
5. Conectar y aceptar el mensaje de confianza del host.
    

---

## 🔹 **3. Conexión desde Linux/Mac (SSH nativo)**

1. Ubicarse en la carpeta donde esté el archivo `.pem`.
    
2. Darle permisos correctos:
    
    ```
    chmod 400 archivo.pem
    ```
    
3. Ejecutar el comando SSH:
    
    ```
    ssh -i archivo.pem ec2-user@IP_PUBLICA
    ```
    
4. Aceptar el mensaje de confianza.
    
5. Ya estás conectado.
    

---

## 🔹 **4. Nota importante**

Si SSH falla:

- No te preocupes, en la siguiente clase se usará **otro método más sencillo y universal**.
    
- Esta clase solo muestra el método tradicional desde tu equipo local.
    

---

## ⭐ **Idea central del documento**

El objetivo es enseñar cómo ingresar a una instancia EC2 usando la clave privada generada anteriormente, mostrando el procedimiento completo tanto para Windows como para Linux/Mac y resaltando la importancia de usar la IP pública y configurar correctamente los permisos del archivo `.pem`.

---
