
---

Vamos a las instancias.

Vamos a lanzar **tres instancias EC2**.

Hacemos clic en _Launch instances_.  
El nombre no importa mucho; puedes cambiarlo después.  
Selecciono mi AMI personalizada, tipo **t2.micro**, el _key pair_ que ya tengo, misma VPC y subred.  
Esto es muy importante: **asegúrate de que la IP pública esté habilitada**.  
En el _security group_, vamos a usar **SSH-22**.  
Todo lo demás queda igual.

Seleccionamos **3 instancias** y lanzamos.

Vamos al listado de instancias.  
Puede que no aparezcan de inmediato; refrescamos.  
Ahora están en estado _pending_.  
Esperamos a que estén _running_.

Cambio los nombres:

- Una se llamará **db** (base de datos)
    
- Otra **app**
    
- Otra **client**
    

Ahora ingreso a la instancia **db**.

Ejecutamos el comando Docker para iniciar un contenedor de Postgres:

```
docker run -p 5432:5432 -e POSTGRES_PASSWORD=password postgres
```

Esto descarga la imagen y levanta Postgres.  
El usuario por defecto es `postgres` y la contraseña `password`.

El DB ya está listo para recibir conexiones.

Ahora vamos a la instancia **app** y nos conectamos.  
Como en nuestra AMI ya instalamos el cliente de Postgres, usaremos:

```
psql -h <PRIVATE_IP_DEL_DB> -U postgres
```

Pero no funcionará todavía porque **el puerto 5432 no está permitido en el security group**.

---

## Error común

A veces la gente edita _el mismo security group SSH-22_ y le agrega el puerto 5432.  
Pero ese SG está adjunto a **las tres instancias** (db, app, client).  
Si abres 5432 ahí, **lo abres para los tres**, lo cual es un gran riesgo de seguridad.

---

## Solución correcta: crear SG separados

Vamos a crear **dos nuevos security groups**:

1. **app-SG** → para la instancia app
    
2. **db-SG** → para la instancia db
    

Regla importante en AWS:  
Un security group puede permitir tráfico **proveniente de otro security group**, en lugar de usar IPs.

Esto soluciona el problema de no conocer el rango de IP exacto.

### Configuración:

- En **db-SG**, abrimos el puerto **5432**.
    
- Como origen, seleccionamos **app-SG** (no usamos IPs).
    

Esto significa:

> “Permito conexiones al DB siempre que provengan de una instancia con el SG app-SG”.

Ahora adjuntamos:

- **db-SG** a la instancia DB
    
- **app-SG** a la instancia APP
    

Probamos de nuevo el comando `psql` desde la app → ahora funciona.

---

## Evitar acceso desde el cliente

Vamos al **client** e intentamos conectarnos a la DB:

```
psql -h <PRIVATE_IP_DEL_DB> -U postgres
```

No funciona, como debe ser.

---

## Comunicación cliente → app (puerto 80)

Ahora en la instancia **app** desplegamos nginx:

```
docker run -p 80:80 nginx
```

Queremos que el cliente pueda acceder a nginx usando:

```
curl <PRIVATE_IP_DE_APP>
```

Pero no funciona porque el puerto 80 tampoco está permitido.

Creamos otro security group:

- **client-SG**
    

Luego modificamos **app-SG**:

- Abrimos el puerto 80
    
- Como origen seleccionamos **client-SG**
    

Adjuntamos **client-SG** a la instancia client.

Ahora desde client:

```
curl <PRIVATE_IP_DE_APP>
```

→ funciona correctamente.

---

## Limpieza final

- Cerramos sesiones.
    
- Terminamos todas las instancias.
    
- Eliminamos los security groups adicionales en orden inverso (porque algunos SG referencian a otros).
    
    - Primero **db-SG**
        
    - Luego **app-SG**
        
    - Luego **client-SG**
        
    - Finalmente **SSH-22** si no se usa
        

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento enseña a crear una arquitectura segura usando múltiples **security groups** en AWS para controlar la comunicación entre instancias EC2.

---

## ⭐ 1. Configuración inicial

Se lanzan tres instancias EC2 usando una AMI personalizada:

- **db** (base de datos)
    
- **app** (aplicación backend)
    
- **client** (cliente que consume la app)
    

Todas inician con el SG básico **SSH-22**.

---

## ⭐ 2. Problema principal

El _app_ necesita conectarse al _db_ en el puerto 5432, pero:

- Las reglas del SG actual no lo permiten.
    
- No se debe agregar el puerto 5432 al SG compartido, porque abriría ese puerto para **todas** las instancias, lo cual es inseguro.
    

---

## ⭐ 3. Solución: Security Groups basados en otros SG

Se crean:

- **app-SG**
    
- **db-SG**
    

Configuración:

- **db-SG** abre puerto 5432.
    
- El origen permitido **no es una IP**, sino el SG **app-SG**.
    

Esto permite:

✔ La instancia app puede hablar con db  
✘ La instancia client **no** puede hablar con db

---

## ⭐ 4. Comunicación cliente → app

Para permitir que el cliente acceda a nginx en la app (puerto 80):

- Se crea **client-SG**.
    
- En **app-SG**, se abre el puerto 80 con origen = client-SG.
    

Esto permite:

✔ client → app (HTTP)  
✘ otros orígenes → app

---

## ⭐ 5. Conclusión y buenas prácticas

- **Nunca abras puertos en SG compartidos** entre varias instancias.
    
- **Usa SGs basados en otros SGs**, no en IPs.
    
- Esto garantiza seguridad aunque las IP cambien.
    
- Permite arquitecturas seguras app → db → client.
    
- Es la forma recomendada por AWS para microservicios y aplicaciones multi-tier.
    

---

Si quieres, puedo también prepararte:

✅ un diagrama ASCII de toda la arquitectura  
✅ un resumen ultra corto (10 líneas)  
✅ una explicación orientada a Spring Boot (app ↔ db ↔ client)  
✅ una versión lista para documentación de tu equipo