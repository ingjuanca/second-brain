
---

Primero, familiaricémonos con algunos términos de la nube. Aprenderemos mucho durante el curso, pero estos son conceptos muy básicos que debemos conocer: **región**, **zona de disponibilidad** (a veces llamada _AZ_) y **ubicación de borde** (_edge location_).

Una _región_ es literalmente un área geográfica donde crearemos nuestros recursos en la nube, como máquinas virtuales, balanceadores de carga, etc. Puedes buscar “AWS Global Infrastructure” y encontrarás esta página.

Al momento de grabar esta clase, hay 33 regiones lanzadas y algunas más próximas a lanzarse (las que aparecen en rojo). AWS tiene regiones en muchas partes del mundo. Una región es simplemente un área geográfica.

Si revisas alguna región (por ejemplo, Norte de Virginia), verás que tiene seis zonas de disponibilidad. Cuando haces zoom dentro de una región, esta puede tener dos o más zonas de disponibilidad; el mínimo es dos. Una zona de disponibilidad es un centro de datos: un edificio con muchos servidores.

Las zonas de disponibilidad están físicamente separadas entre sí (por ejemplo, 30 o 50 millas). Tienen su propio suministro eléctrico, etc.  
¿Para qué?  
Imagina que desplegaste tu aplicación en un centro de datos de Northern Virginia y ocurre un desastre natural que afecta a esa instalación. Es probable que la otra zona de disponibilidad no se vea afectada, por lo que tu aplicación seguiría funcionando. Esa es la idea.

Ahora hablemos de las **edge locations** (ubicaciones de borde).

Aquí tenemos EE. UU. y las regiones actuales. Imagina que desplegaste tu aplicación en Virginia y tienes miles de clientes ubicados en esta zona. Tu aplicación tiene contenido estático como HTML, JavaScript, imágenes y CSS.

Cuando los clientes intentan acceder a tu aplicación desde esta zona, debido a la distancia, todo el contenido debe servirse desde Virginia. Esto incrementa la **latencia**, afectando el **tiempo de carga** y el **tiempo de respuesta**.

Aquí es donde entran las _edge locations_.  
Al momento de esta grabación, existen más de 600 edge locations en el mundo (seguramente más cuando veas esta clase). Piensa en ellas como mini centros de datos.

¿Qué hacen?  
Si un cliente intenta acceder a tu aplicación, la edge location contactará al centro de datos de Virginia, obtendrá los archivos HTML, CSS, etc., y los **cacheará**.

A partir de ese momento, cuando otro cliente cercano acceda a la aplicación, recibirá el contenido cacheado desde la edge location más cercana. Esto mejora el tiempo de respuesta y la velocidad de carga de la aplicación.

Ese es el concepto.

---

# 📝 **Resumen completo del documento (versión clara y profesional)**

El documento introduce tres conceptos fundamentales de la infraestructura global de AWS:

---

## 🔹 **1. Región (Region)**

Una **región** es un área geográfica donde AWS ofrece sus servicios. Cada región contiene múltiples centros de datos independientes llamados Zonas de Disponibilidad.

- Actualmente AWS tiene **33 regiones** (al momento del documento).
    
- Usamos regiones para desplegar recursos como EC2, balanceadores, bases de datos, etc.
    

---

## 🔹 **2. Zona de Disponibilidad (Availability Zone – AZ)**

Una zona de disponibilidad es un **centro de datos físico**, con energía, red y hardware independientes.

- Cada región tiene **al menos 2 AZs**, algunas tienen 6 o más.
    
- Las AZs están separadas físicamente (30–50 millas).
    
- Permiten **alta disponibilidad**: si una falla, las otras pueden mantener tu aplicación activa.
    

Ejemplo:  
Si tu app corre en una AZ y ocurre un desastre, otra AZ de la misma región puede seguir operando tu sistema sin interrupciones.

---

## 🔹 **3. Ubicación de Borde (Edge Location)**

Son **mini centros de datos pequeños distribuidos globalmente** (más de 600).  
Su función principal es **cachear contenido** estático y entregarlo desde el sitio más cercano al usuario.

- Se usan principalmente con **CloudFront (CDN de AWS)**.
    
- Reducen la latencia y aceleran los tiempos de carga.
    
- Mejoran la experiencia del usuario final en cualquier parte del mundo.
    

Ejemplo:  
Si tu app está en Virginia y un usuario en otro país accede, el contenido estático puede servirse desde la edge location local, no desde Virginia.

---

## ⭐ **Idea central del documento**

Explica cómo AWS organiza su infraestructura global para garantizar:

- **Disponibilidad** (regiones + AZs)
    
- **Baja latencia y alto rendimiento** (edge locations + caching)
    
- **Resiliencia ante fallos** (separación física de AZs)
    

Estos conceptos son la base de todo lo que se construirá en el curso.
