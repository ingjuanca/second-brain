
---

Iniciemos sesión en la consola de AWS.

Lo primero que quiero que hagas es elegir una región.  
Durante todo el curso yo usaré **us-east-1 (Northern Virginia)**.  
Si estás en Singapur u otra zona y quieres usar esa región, está bien. Pero lo importante es que uses **siempre la misma región durante todo el curso**, para evitar confusiones, porque la mayoría de los servicios de AWS son específicos por región.

Por ejemplo, estoy en esta región y lanzo una máquina virtual. Si luego cambio la región por accidente o sin darme cuenta, y verifico EC2, no veré la VM. Esto causa pánico en mucha gente, que cree que su instancia “desapareció”, cuando en realidad solo cambiaron de región. Por eso digo que casi todos los recursos son específicos de una región.

Ahora, hablemos de la consola. Puede que veas la interfaz diferente a mi pantalla, y eso está bien. AWS cambia su UI de vez en cuando. No te preocupes si tu pantalla no luce igual a la del video; siempre deberías poder encontrar las opciones. Si algo cambia mucho, házmelo saber y te ayudaré.

Vamos a lanzar una instancia EC2.  
Puedes hacer clic en el acceso directo o buscar “EC2” en la barra de búsqueda. Incluso puedes marcarlo como favorito para accederlo rápidamente.

Ya estando en EC2, ve a **Instances**. Aquí verás todas tus instancias. Si alguna está corriendo y quieres detenerla, lo harás aquí. Por ahora no tenemos ninguna, así que haremos clic en **Launch Instances**.

Aquí especificarás todos los detalles: sistema operativo, RAM, CPU, etc.

1. **Nombre de la máquina virtual**  
    Escribe el nombre que quieras. Yo usaré “Vince1”.
    
2. **AMI (Amazon Machine Image)**  
    Aquí eliges Linux, Ubuntu, Windows, macOS, etc. Incluso puedes crear tu propia AMI (lo veremos después).  
    Para ahora, elige **Linux**.
    
3. **Arquitectura de CPU**  
    Podrías usar ARM, pero dejaremos la opción por defecto.
    
4. **Tipo de instancia**  
    Aquí defines CPU y memoria. La instancia free tier da 1 CPU y 1 GB de RAM, suficiente para el curso.  
    Puedes ver comparaciones: AWS ofrece hasta instancias con **896 CPU y varios terabytes de RAM** (muy costosas).
    
5. **Key Pair**  
    Necesitamos un par de llaves para conectarnos por SSH.
    
    - Mac/Linux: usa `.pem`
        
    - Windows: usa PuTTY  
        Nombra tu key pair (por ejemplo “Vince”), créalo y descárgalo. **Guárdalo en un lugar seguro**.
        
6. **Network Settings**  
    No explicaré esto ahora porque es un tema avanzado, lo veremos después.  
    Pero asegúrate de que **Auto-assign Public IP = Enabled**. Es crítico.
    
7. **Security Group**  
    Es una lista de reglas de firewall.  
    Edítalo, dale un nombre (por ejemplo “CSG”), y abre el puerto **22** para SSH.  
    El origen puede ser “Anywhere”, o puedes restringirlo a tu IP.
    
8. **Disco (Storage)**  
    La instancia viene con 8 GB por defecto, suficiente.  
    Free tier permite hasta 30 GB si quisieras cambiarlo.
    
9. **Detalles adicionales**  
    Opcionales. Ignóralos por ahora.
    

Una vez listo, haz clic en **Launch Instance**.

En la lista de instancias, puede que no veas nada inicialmente.  
Usa **refresh**.  
Si no funciona, usa **hard refresh del navegador** (F5).

Eventualmente verás la instancia en estado **running** (puede tardar 1–2 min). Si haces clic, verás:

- **Public IP**: para acceder desde internet.
    
- **Private IP**: para comunicación interna.
    
- **VPC y Subnet**: conceptos de red que veremos más adelante.
    
- **ARN**: identificador único del recurso.
    

En **Status Checks** verás:

- “System check passed”
    
- “Instance reachability check passed”
    

Significa que la VM está lista.

En **Monitoring** verás CPU y otros gráficos.

En **Security**, verás:

- **Inbound rules**: solo el puerto 22 está abierto.
    
- **Outbound rules**: todo el tráfico saliente está permitido.
    

En **Storage**, verás el disco de 8 GB.

Finalmente, en **Tags**, puedes agregar key-value pairs.  
Son muy importantes en organizaciones grandes para encontrar tus propios recursos. Ejemplos:

- Name = Vince1
    
- Department = dev
    
- CostCenter = 101
    

Si hubiera cientos de instancias, podrías filtrar por etiquetas como “dev”, “qa”, etc.

---

# 📝 **RESUMEN COMPLETO Y PROFESIONAL**

Este documento guía el proceso completo de lanzar una instancia EC2 por primera vez y explica conceptos esenciales de la consola de AWS.

---

## ⭐ **Puntos clave**

### **1. Elegir siempre la misma región**

Los servicios de AWS son _region-specific_.  
Si cambias la región, no verás tus recursos, lo que causa confusión.  
Para el curso se recomienda **us-east-1 (Northern Virginia)**.

---

### **2. Lanzar una instancia EC2**

El proceso incluye:

- Elegir un nombre.
    
- Seleccionar una AMI (Linux para el curso).
    
- Elegir tipo de instancia (t2.micro o t3.micro – 1 vCPU, 1GB RAM).
    
- Crear un Key Pair para acceso SSH.
    
- Revisar configuración de red (autogenerar IP pública).
    
- Configurar un Security Group (abrir puerto 22).
    
- Seleccionar el tamaño del disco (8 GB por defecto).
    
- Lanzar la instancia.
    

---

### **3. Verificación del estado**

Una vez creada la instancia:

- Puede tardar 1–2 minutos en pasar a “running”.
    
- AWS realiza dos tipos de health checks:
    
    - **Sistema (hypervisor)**
        
    - **Instancia (VM)**
        

Ambos deben decir “passed”.

---

### **4. Información importante de la instancia**

- **Public IP**: acceso remoto.
    
- **Private IP**: uso interno de la VPC.
    
- **Security Group**: firewall controlando puertos entrantes y salientes.
    
- **Storage**: volumen EBS asignado.
    
- **Tags**: organizan y clasifican recursos.
    

---

### **5. Importancia de los Tags**

Permiten filtrar recursos en cuentas grandes.  
Ejemplo: Department=dev, CostCenter=101.

---

## 🎯 **Idea principal**

El documento enseña paso a paso cómo crear correctamente una instancia EC2 y entender todos los elementos esenciales asociados, garantizando que los estudiantes puedan seguir el curso sin confusiones, especialmente respecto a región, seguridad, red y visibilidad de recursos.

