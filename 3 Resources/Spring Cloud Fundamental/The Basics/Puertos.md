
---

En las siguientes secciones vas a crear dos microservicios: **el servicio de cupones (coupon service)** y **el servicio de productos (product service)**. Luego, utilizarás varios proyectos de Spring Cloud como **Eureka Server**, **Zuul API Gateway Server** y otros.

Cada uno de estos servidores necesita un **puerto** para ejecutarse en una máquina. En este caso, usaremos la máquina local para correr todos estos servidores, pero en **puertos diferentes**.

- El **coupon service** (servicio de cupones), el primero que desarrollaremos, se ejecutará en el **puerto 8080** (puerto por defecto de Tomcat).
- Otra instancia del mismo **coupon service** se ejecutará en el **puerto 8081** en la misma máquina.
- El **product service** (servicio de productos) se ejecutará en el **puerto 1990**.
- El **Eureka Server** (de Spring Cloud) se ejecutará en el **puerto 8761** (recomendado por Spring).
- El **Zuul API Gateway Server** se ejecutará en el **puerto 8765**.
- El **Config Server** se ejecutará en el **puerto 8888**.
- Finalmente, el servidor de **Zipkin (distributed tracing)** se ejecutará en el **puerto 9411**.

Estos son los puertos recomendados para Eureka y los demás proyectos. Más adelante, en las prácticas, se volverán a mencionar estos números de puerto.

En resumen, todo se ejecutará en la máquina local y se gestionará mediante puertos. El mismo **coupon service** podrá tener múltiples instancias en diferentes puertos Tomcat, y el **product service** también correrá en un puerto distinto (1990).

---

## 📝 Resumen estructurado

🔹 **Contexto:**  
Se definen los **puertos locales** en los que correrán los microservicios y servidores de Spring Cloud en las prácticas.

🔹 **Microservicios y sus puertos:**

- Coupon Service → 8080 (y otra instancia en 8081)
- Product Service → 1990

🔹 **Spring Cloud servidores y sus puertos:**

- Eureka Server → 8761
- Zuul API Gateway → 8765
- Config Server → 8888
- Zipkin (distributed tracing) → 9411

🔹 **Clave:**  
Todos se ejecutan en la misma máquina local pero en diferentes puertos, lo que permite correr múltiples instancias de un mismo servicio (ej. coupon service en 8080 y 8081).

---