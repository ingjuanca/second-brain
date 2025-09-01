
---

Cuando implementamos arquitecturas de microservicios, podemos crear cualquier número de microservicios usando los módulos que nos ofrece **Spring Boot**.

Por ejemplo, si estamos trabajando en un software de gestión hospitalaria, podríamos crear:

- Un microservicio de registro de pacientes,
- Un microservicio clínico de pacientes,
- Un microservicio de gestión de reclamos de pacientes,
- Un microservicio de gestión de camas de pacientes.

Pero habrá varios **requisitos no funcionales** para estos microservicios, comenzando por el **registro y descubrimiento de servicios**.

Cada microservicio debe registrarse en un servidor centralizado, y los demás microservicios deben poder descubrirlo dinámicamente y comunicarse con él. Sin esto, cada microservicio quedaría **fuertemente acoplado** a los otros.

1. **Registro y descubrimiento de servicios.**
2. **Balanceo de carga.** A medida que aumenta la carga, se deben ejecutar múltiples instancias del mismo microservicio en diferentes servidores, y la carga debe distribuirse equitativamente.
3. **Tolerancia a fallos.** Si un microservicio falla, el sistema completo no debe colapsar; los fallos deben manejarse de forma controlada.
4. **Integración sencilla.** Los microservicios deben comunicarse fácilmente mediante APIs REST.
5. **Preocupaciones transversales (cross-cutting concerns).** Aspectos comunes como autenticación, autorización, seguridad y logging no deben repetirse en cada microservicio, sino centralizarse en un solo lugar. 
6. **Trazabilidad distribuida.** Como las peticiones pueden pasar de un microservicio a otro (por ejemplo, de registro a clínico o a gestión de camas), se debe poder rastrear el recorrido completo de cada petición para identificar dónde ocurre un fallo.

Estos elementos **no están disponibles en Spring Boot**.  
Aquí es donde entra **Spring Cloud**, una colección de componentes open source que permite implementar todo lo anterior y más.

En la web de Spring (spring.io) se encuentran numerosos proyectos de Spring Cloud. Algunos de los principales:

- **Eureka (Netflix):** registro y descubrimiento de servicios.
- **Ribbon:** balanceo de carga.
- **Hystrix:** tolerancia a fallos.
- **Feign Client:** creación sencilla de clientes REST.
- **Zuul Proxy Gateway:** implementación centralizada de aspectos transversales (seguridad, logging, etc.).
- **Sleuth:** trazabilidad distribuida.
- **Zipkin:** tablero visual para rastrear peticiones entre microservicios.

Con estos componentes de Spring Cloud podemos cubrir los requisitos no funcionales de nuestras arquitecturas de microservicios.

---

## 📝 Resumen estructurado

🔹 **Problema:**  
Spring Boot permite crear microservicios, pero no resuelve requisitos no funcionales como descubrimiento, balanceo, fallos, seguridad o trazabilidad.

🔹 **Solución:**  
Spring Cloud provee componentes especializados que complementan a Spring Boot.

🔹 **Principales requisitos y sus soluciones en Spring Cloud:**

- **Registro/descubrimiento de servicios:** _Eureka (Netflix)_
- **Balanceo de carga:** _Ribbon_
- **Tolerancia a fallos:** _Hystrix_
- **Integración sencilla:** _Feign Client_
- **Cross-cutting concerns (seguridad, logging, etc.):** _Zuul Proxy Gateway_
- **Trazabilidad distribuida:** _Sleuth + Zipkin_

🔹 **Beneficio:**  
Permite construir arquitecturas de microservicios escalables, tolerantes a fallos, seguras y fáciles de integrar.

---