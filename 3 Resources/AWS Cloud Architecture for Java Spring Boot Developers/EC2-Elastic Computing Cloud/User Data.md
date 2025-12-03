
---

En esta lección hablemos sobre una instancia EC2 con **user data**.

Esta no es una funcionalidad muy importante, pero es útil en ciertos casos, y por eso la estamos viendo.

Primero, lancemos una instancia.

Como siempre, dale algún nombre; no importa mucho.  
Selecciono mi AMI, tipo **t2.micro**.  
El _key pair_ podemos ignorarlo porque no lo usaremos; no vamos a hacer nada especial.  
La subred, VPC, etc., dejamos todo por defecto.

Esto es importante:  
No usaremos SSH.  
Sin embargo, voy a permitir solicitudes HTTP desde internet, lo cual abrirá el puerto 80 automáticamente.

El almacenamiento puede ser 8 GB.  
Ahora expandimos **Additional details**.

Al bajar verás muchas opciones.  
Si tienes dudas sobre alguna, puedes hacer clic en el ícono de información para leer más.  
Hay demasiadas opciones, y si habláramos de todas, este curso nunca terminaría.  
Por eso nos enfocamos solo en los conceptos más importantes.

Seguimos bajando hasta llegar a **User data**.

¿Qué es esto?  
“User data” no es un buen nombre, es un poco confuso.  
User data simplemente permite ejecutar **scripts de inicialización**, es decir, comandos que se ejecutan automáticamente cuando la instancia arranca por primera vez.

La idea es:  
Si quieres ejecutar algún comando al iniciar el sistema, como iniciar Docker o correr nginx, puedes escribir ese script aquí, sin necesidad de acceder por SSH ni ejecutar nada manualmente.

Aquí pegamos este script:

```
#!/bin/bash
docker run -d -p 80:80 nginx
```

- La línea `#!/bin/bash` indica que usaremos bash para ejecutar estos comandos.
    
- `docker run -d` inicia el contenedor en modo _daemon_, es decir, en segundo plano.
    
- `-p 80:80` mapea el puerto 80 del contenedor al 80 de la instancia.
    
- `nginx` es la imagen de Docker que queremos ejecutar.
    

Esto significa que al iniciar la EC2:

1. Se iniciará automáticamente Docker.
    
2. Se descargará la imagen nginx.
    
3. Se levantará un servidor nginx escuchando en el puerto 80.
    

Lanzamos la instancia.

Esperamos un minuto.  
Refrescamos la página.  
Ya está en estado _running_.

Copiamos la IP pública.

En la consola aparece una opción de “open address”, pero a veces no funciona porque usa HTTPS automáticamente, y nginx está corriendo en HTTP.

Pegamos la IP en el navegador con **http://**, y veremos la página por defecto de nginx funcionando correctamente.

Todo esto sin necesidad de usar SSH.

Así es como funciona **user data**.  
Es útil en varios escenarios, por eso lo estamos viendo.

Podemos terminar la instancia ahora.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento explica el uso de **EC2 User Data**, una función que permite inicializar automáticamente una instancia EC2 ejecutando scripts durante su primer arranque.

---

## ⭐ 1. ¿Qué es User Data?

**User Data** permite ejecutar scripts de inicio cuando la instancia EC2 arranca por primera vez.  
Sirve para automatizar tareas como:

- Instalar software
    
- Descargar paquetes
    
- Ejecutar contenedores
    
- Configurar servicios
    
- Realizar tareas de bootstrapping sin usar SSH
    

---

## ⭐ 2. Escenario práctico

Se lanza una instancia EC2 usando:

- AMI personalizada
    
- Tipo t2.micro
    
- Security group que permite HTTP (puerto 80)
    
- Sin necesidad de key pair ni SSH
    

Durante la creación, en **Additional details → User data**, se agrega un script:

```bash
#!/bin/bash
docker run -d -p 80:80 nginx
```

Este script:

- Arranca Docker en segundo plano
    
- Descarga la imagen nginx
    
- Ejecuta un contenedor mapeado al puerto 80
    

---

## ⭐ 3. Resultado

- La instancia se inicia.
    
- El script se ejecuta automáticamente.
    
- Nginx queda funcionando sin intervención manual.
    
- Al abrir la IP pública en el navegador usando **HTTP**, se ve la página por defecto de nginx.
    
- No fue necesario conectarse por SSH en ningún momento.
    

---

## ⭐ 4. ¿Por qué es útil?

User Data es útil para:

- Automatizar despliegues simples
    
- Configurar servidores al inicio
    
- Preparar ambientes automáticamente
    
- Realizar acciones una sola vez sin scripts externos
    
- Crear imágenes configuradas sin necesidad de AMIs personalizadas (en algunos casos)
    

---

Si quieres, puedo también:

✅ mostrarte una versión avanzada del script user-data (con instalación de Docker, Java, etc.)  
✅ comparar AMI personalizada vs. user-data  
✅ generar un resumen ultra corto  
✅ explicarlo en términos de DevOps, autoscaling o microservicios