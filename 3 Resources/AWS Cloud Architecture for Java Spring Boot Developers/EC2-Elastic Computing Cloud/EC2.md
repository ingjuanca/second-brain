
---

EC2 significa _Elastic Compute Cloud_. Es un servicio que proporciona capacidad de cómputo “redimensionable” en la nube. Básicamente, puedes lanzar una máquina virtual en la nube, y en menos de un minuto puedes elegir tu sistema operativo como Ubuntu, Linux, etc. A esto lo llamamos **AMI (Amazon Machine Image)**. También deberás especificar tu CPU, memoria, etc. Por ejemplo: quiero 100 CPU y 100 GB de RAM. Sí, todas esas configuraciones son posibles. Definas lo que definas, puedes crear una VM así de simple y escalarla hacia arriba o hacia abajo según tus necesidades.

Hablemos del modelo de precios de EC2, algo importante de conocer.

Existen cuatro modelos de precios:

### 1. **On-Demand (bajo demanda)**

Lanzas una máquina virtual y la usas el tiempo que quieras. Pagas por uso.  
Es similar a alquilar una habitación de hotel en una ciudad que visitas: si te quedas 10 días, pagas esos 10 días.

### 2. **Reserved Instances (instancias reservadas)**

Reservas una VM por 1 o 3 años. Como te comprometes, recibes un descuento.  
Es similar a arrendar una casa: hay un compromiso a largo plazo, pero obtienes un mejor precio.

### 3. **Dedicated Instances (instancias dedicadas)**

Es como ser propietario de una casa. Obtienes hardware físico completamente dedicado para ti.

### 4. **Spot Instances (instancias spot)**

Puedes obtener hasta un 90% de descuento, lo cual significa mucho ahorro.  
¿Cómo funciona? Imagina que el centro de datos de Virginia tiene miles de máquinas. AWS gana dinero alquilando estas máquinas. Pero a veces muchas quedan sin uso. En esos casos, AWS básicamente dice: “alguien, por favor, use estas máquinas; te doy 90% de descuento”.

Si tú obtienes una VM con 90% de descuento y empiezas a usarla, pero luego llega alguien dispuesto a pagar el precio on-demand completo, AWS te dará un aviso de 2 minutos, quitará la máquina que te prestó con descuento y se la entregará al nuevo usuario que paga más. Ese es el “riesgo” de las spot.

Las instancias spot son útiles si aceptas interrupciones. Por ejemplo, el instructor las usa para ejecutar _test automation scripts_, pero nunca para aplicaciones críticas o de producción orientadas al cliente.

Después de esta teoría, el siguiente paso es ir a la consola y trabajar con una instancia EC2. La opción por defecto será la que usaremos en este curso.

---

# 📝 **Resumen completo del documento (versión experta y clara)**

Este documento introduce **Amazon EC2**, uno de los servicios fundamentales de AWS, explicando qué es, cómo funciona y cuáles son sus modelos de precios.

---

## 🚀 **¿Qué es EC2?**

EC2 (_Elastic Compute Cloud_) es el servicio que permite lanzar **máquinas virtuales escalables** en la nube de AWS.  
El usuario puede elegir:

- Sistema operativo (_AMI_: Amazon Machine Image)
    
- CPU
    
- Memoria
    
- Capacidad y tipo de almacenamiento
    
- Opciones de desempeño
    

Las instancias pueden escalarse hacia arriba o hacia abajo según la demanda.

---

## 💰 **Modelos de precios de EC2 (4 tipos)**

### **1. On-Demand**

- Pagas solo por el tiempo usado.
    
- Sin compromisos.
    
- Ideal para cargas impredecibles.
    
- Analogía: alquilar una habitación de hotel por días.
    

### **2. Reserved Instances**

- Compromiso de 1 o 3 años.
    
- Precios más bajos a cambio de la permanencia.
    
- Analogía: arrendar una casa.
    

### **3. Dedicated Instances**

- Hardware físico exclusivo para ti.
    
- Más costoso, pero con aislamiento total.
    
- Analogía: tener tu propia casa.
    

### **4. Spot Instances**

- Hasta **90% de descuento** si AWS tiene capacidad ociosa.
    
- Pueden ser interrumpidas con un aviso de 2 minutos si alguien paga tarifa on-demand.
    
- Recomendadas para tareas tolerantes a fallos (ej. pruebas automáticas, procesamiento batch).
    
- No recomendadas para producción orientada al cliente.
    

---

## ⭐ **Idea clave**

EC2 ofrece flexibilidad extrema: puedes elegir el hardware virtual, cambiarlo cuando sea necesario y pagar según un modelo que se adapte a tus necesidades.  
AWS ofrece opciones desde máquinas muy baratas pero interrumpibles (Spot), hasta hardware exclusivo (Dedicated), pasando por modelos equilibrados como On-Demand y Reserved.

---