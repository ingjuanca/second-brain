
---

Perfecto. A continuación tienes **la traducción completa al español** del documento que compartiste y luego un **resumen completo, claro y profesional**, centrado en **IAM Roles, EC2 y acceso seguro a S3 sin credenciales**.  

---

# ✅ **TRADUCCIÓN COMPLETA AL ESPAÑOL**

## IAM Role

En esta lección vamos a hablar sobre **roles**.

El escenario es el siguiente:  
tenemos una **instancia EC2** y queremos que esa instancia **se comunique con S3**.

---

### Escenario inicial

Volvemos a la consola de AWS.  
Estamos usando la ventana del **desarrollador (Dev)**.

Lanzamos una instancia EC2:

- En la región correcta
    
- Tipo `t2.micro`
    
- Sin key pair
    
- Usando el security group existente con el puerto 22 abierto
    

Esperamos a que la instancia esté _running_ y nos conectamos usando **EC2 Instance Connect**.

---

### Uso de AWS CLI desde EC2

La instancia corre Linux y ya trae instalado **AWS CLI**.

Probamos ejecutar:

```bash
aws s3 ls
```

El resultado es un error:

> _Unable to locate credentials_

Esto significa que **la instancia EC2 no tiene permisos para acceder a S3**.

No es un problema del usuario humano, sino de la **instancia EC2**.

---

### Breve explicación del AWS CLI

El formato general es:

```bash
aws <service> <command>
```

Ejemplos:

- `aws s3 ls`
    
- `aws s3 cp`
    
- `aws ec2 describe-instances`
    

Cada servicio tiene múltiples comandos disponibles.

---

### Crear un IAM Role para EC2

Cambiamos a la ventana del **administrador**.

Vamos a **IAM → Roles → Create role**.

Seleccionamos:

- **AWS service**
    
- Caso de uso: **EC2**
    

Esto indica que:

> _Vamos a permitir que EC2 acceda a otros servicios de AWS_

---

### Permisos del rol

Buscamos **S3** y seleccionamos:

- **AmazonS3ReadOnlyAccess**
    

Esto permite:

- Listar buckets
    
- Descargar archivos
    
- ❌ No permite subir archivos
    

Asignamos un nombre al rol, por ejemplo:

- `EC2-S3`
    

Creamos el rol.

---

### Problema: el desarrollador no puede asignar el rol

Volvemos a la ventana del **desarrollador**.

Intentamos:

- EC2 → Actions → Security → Modify IAM role
    

Resultado:

- **Access Denied**
    

Esto ocurre porque:

- El desarrollador **no tiene permisos IAM** para ver o asignar roles
    

---

### Solución: permisos IAM para el desarrollador

Volvemos a la ventana del **administrador**.

Entramos al grupo **developers** y creamos una **inline policy** que permita:

- `iam:ListInstanceProfiles`
    
- `iam:PassRole`
    

Estas acciones permiten:

- Ver roles disponibles
    
- Asignar roles a instancias EC2
    

Guardamos la policy.

---

### Asignar el rol a la instancia EC2

Volvemos a la ventana del desarrollador y refrescamos.

Ahora:

- El rol `EC2-S3` aparece
    
- Podemos asignarlo a la instancia EC2
    
- El cambio se aplica **sin reiniciar la instancia**
    

---

### Acceso a S3 desde EC2 (lectura)

Volvemos a la consola de la instancia EC2.

Ejecutamos:

```bash
aws s3 ls
```

Ahora funciona.

Listamos contenido del bucket:

```bash
aws s3 ls vince-demo/
aws s3 ls vince-demo/public/
```

Descargamos un archivo:

```bash
aws s3 cp s3://vince-demo/public/01aws.png .
```

El archivo se descarga correctamente en la instancia EC2.

---

### Intento de escritura (falla esperada)

Intentamos subir el archivo de vuelta a S3:

```bash
aws s3 cp 01aws.png s3://vince-demo/public/
```

Resultado:

- **Upload failed**
    

Esto es correcto, porque:

- El rol solo tiene **acceso de lectura**
    
- No tiene permisos de escritura
    

---

### Concepto clave demostrado

La instancia EC2:

- ❌ No tiene credenciales (username/password)
    
- ✅ Usa un **IAM Role**
    
- AWS gestiona credenciales temporales automáticamente
    
- Todo ocurre **detrás de escena**
    

Este es el **mecanismo recomendado por AWS**.

---

### Limpieza final

- Se termina la instancia EC2
    
- Se elimina el rol IAM (opcional)
    
- Crear roles **no tiene costo**
    

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento demuestra cómo usar **IAM Roles** para permitir que una **instancia EC2 acceda a S3 de forma segura**, sin manejar credenciales manuales.

---

## ⭐ 1. Problema que resuelve IAM Roles

- EC2 necesita acceder a S3
    
- No se deben usar:
    
    - Access Keys
        
    - Secret Keys
        
    - Credenciales hardcodeadas
        

IAM Roles resuelven esto de forma segura.

---

## ⭐ 2. Rol IAM para EC2

- Se crea un rol con permisos específicos (S3 Read Only)
    
- El rol se asigna directamente a la instancia EC2
    
- AWS inyecta credenciales temporales automáticamente
    

---

## ⭐ 3. Seguridad y principio de menor privilegio

- El rol solo permite **lectura**
    
- Las operaciones de escritura fallan
    
- El acceso está estrictamente controlado
    

---

## ⭐ 4. Permisos IAM para humanos

Para que un desarrollador pueda asignar roles a EC2, necesita permisos como:

- `iam:ListInstanceProfiles`
    
- `iam:PassRole`
    

IAM es **extremadamente granular**.

---

## ⭐ 5. Buenas prácticas demostradas

- ✅ Usar IAM Roles, nunca credenciales en EC2
    
- ✅ Separar permisos humanos vs permisos de servicios
    
- ✅ Aplicar menor privilegio
    
- ✅ Usar políticas administradas + inline policies
    
- ✅ Seguridad sin fricción operativa
    

---

# 🎯 **Idea principal**

**IAM Roles son la forma correcta y segura de permitir comunicación entre servicios AWS.**  
EC2 obtiene permisos dinámicos y temporales para acceder a S3 sin exponer credenciales, manteniendo la seguridad y escalabilidad del sistema.

---

Si quieres, el siguiente paso ideal sería:

✅ Comparar **IAM Roles vs Access Keys**  
✅ Usar **IAM Roles con Spring Boot en EC2**  
✅ Diseñar un modelo IAM realista para microservicios  
✅ Prepararte para preguntas avanzadas de entrevista sobre IAM