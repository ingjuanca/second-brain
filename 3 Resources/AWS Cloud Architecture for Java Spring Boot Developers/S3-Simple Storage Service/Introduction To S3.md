
---

En esta sección, hablemos de **S3**.

S3 significa _Simple Storage Service_.

Es un almacenamiento de objetos **seguro**, **durable** y **altamente disponible**.

Aquí, “objeto” significa cualquier archivo: texto, CSV, película, mp3, archivo zip, ejecutable… literalmente cualquier archivo.

Entonces, podrías preguntar:  
_¿Por qué lo llamamos almacenamiento de objetos en vez de almacenamiento de archivos?_  
Hay una muy buena razón detrás de esto.  
Lo explicaré después de la demo.

Un objeto puede tener desde **0 bytes hasta 5 terabytes**, y puedes almacenar **n cantidad de objetos**.

Amazon nos cobrará según la cantidad de datos almacenados por mes.

Para precios exactos, te recomendaría buscar “AWS S3 pricing”.  
Ahí encontrarás la calculadora oficial.

Solo para darte una idea, en la región **US East (N. Virginia)**, almacenar **1 GB por mes** cuesta aproximadamente **$0.02 USD**.  
Es bastante económico.

Amazon normalmente lo anuncia como _almacenamiento ilimitado_.

Una vez tuve la oportunidad de hablar con un arquitecto de AWS hace tiempo.  
Le pregunté:  
“¿Qué significa almacenamiento ilimitado? ¿Cómo pueden tener almacenamiento ilimitado?”

La forma en que respondió fue algo así:

> “Tenemos suficiente almacenamiento.  
> Lo único es que probablemente el cliente no tenga suficiente dinero para pagarlo.  
> ¿Quién se quedará sin dinero primero: el cliente o AWS?  
> Si creemos que el cliente se quedará sin dinero primero, entonces para nosotros es ilimitado”.

En **febrero de 2017**, un día tuvimos problemas con nuestra aplicación.  
Resultó que había un problema con S3.  
S3 no estuvo accesible durante un par de horas en la región **US East 1**.

Ese día, muchas personas, incluyéndome, nos dimos cuenta de **cuántos sitios web en el mundo dependen de S3** detrás de escena.

Algunos sitios populares afectados ese día incluyeron:

- GitHub
    
- Docker
    
- Slack
    

Lo curioso fue que incluso el sitio _“IsItDownRightNow.com”_, que normalmente se usa para verificar si los sitios están funcionando, también estuvo caído… por culpa de S3.

No solo la mayoría de sitios web dependen de S3.  
Muchos **servicios de AWS** también dependen internamente de S3.

Cuando usamos S3, creamos algo similar a una carpeta o directorio.  
Esto se llama **bucket**.

Debe tener un **nombre globalmente único**.

Por ejemplo:  
Si yo creo un bucket llamado _events_, tú no puedes crear uno con ese mismo nombre en tu propia cuenta.

Dentro del bucket almacenamos nuestros archivos así:

- file1.txt
    
- file2.txt
    
- etc.
    

Así es cómo se almacenan y acceden los archivos.

Y dado que los nombres de bucket son globalmente únicos, AWS nos da direcciones web (URLs) como esta si queremos acceder a los archivos usando HTTPS.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento introduce Amazon S3, sus características esenciales, su modelo de almacenamiento y su importancia dentro del ecosistema AWS.

---

## ⭐ 1. ¿Qué es S3?

S3 es un servicio de almacenamiento de objetos que ofrece:

- **Seguridad**
    
- **Durabilidad extremadamente alta**
    
- **Alta disponibilidad**
    

Un _objeto_ es cualquier archivo: texto, imágenes, videos, binarios, etc.

Los objetos pueden tener entre **0 bytes y 5 TB**, y se pueden almacenar cantidades ilimitadas.

---

## ⭐ 2. Modelo de costos

- Se cobra por **GB almacenado por mes**.
    
- Precio referencial: ~**$0.02 por GB/mes** en US East (N. Virginia).
    
- Muy económico y escalable.
    
- AWS llama a S3 “almacenamiento ilimitado” porque, en la práctica, **el límite suele ser el presupuesto del cliente**.
    

---

## ⭐ 3. Alta dependencia global

S3 es la base de muchas aplicaciones y servicios, tanto dentro como fuera de AWS.

Un ejemplo histórico:

- En **febrero de 2017**, S3 sufrió una caída en US East-1.
    
- Esto afectó a grandes plataformas como Slack, GitHub y Docker.
    
- Incluso sitios dedicados a verificar fallos como _IsItDownRightNow.com_ quedaron fuera de servicio.
    
- Demostró cuán crítico es S3 para internet.
    

Además, **muchos servicios de AWS dependen internamente de S3**.

---

## ⭐ 4. Buckets y nombres únicos

- En S3 se almacenan los objetos dentro de **buckets**.
    
- Un bucket funciona como una carpeta lógica.
    
- El nombre del bucket debe ser **globalmente único**:  
    si un usuario crea _events_, ningún otro usuario puede usar ese nombre.
    

Los archivos dentro se organizan de manera jerárquica (simulada), por ejemplo:

- events/file1.txt
    
- events/file2.txt
    

Como los nombres de bucket son únicos, AWS permite acceder a ellos mediante URLs HTTPS globales.

---

# 🎯 **Idea principal**

S3 es un servicio central de almacenamiento en la nube: escalable, duradero, muy económico y utilizado por miles de aplicaciones y servicios críticos. Su arquitectura basada en buckets con nombres únicos permite almacenar y acceder a archivos en cualquier parte del mundo.

---

Si quieres, puedo también:

✅ darte un resumen ultra breve (5 líneas)  
✅ explicar por qué S3 es “object storage” y no “file storage”  
✅ compararlo con EFS y EBS  
✅ prepararte preguntas tipo entrevista sobre S3