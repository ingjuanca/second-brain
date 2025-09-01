
---

Hola y bienvenidos a la serie de fundamentos de Spring Cloud. Soy Barath T.P. Reddy, un arquitecto de software y instructor de bestsellers, y haré todo lo posible para ayudarles a aprovechar al máximo este curso.

Comenzaremos este curso aprendiendo qué es Spring Cloud, por qué necesitamos usar Spring Cloud y los diversos componentes o proyectos que forman parte de este ecosistema de Spring Cloud.

Luego, crearemos dos proyectos de microservicios diferentes: el servicio de cupones y el servicio de productos, que se comunicarán entre sí a través de la API REST que exponen. Vamos a utilizar todos los componentes de Spring Cloud en estos dos microservicios y veremos cómo estos microservicios se beneficiarán de Spring Cloud.

El primero de los componentes de Spring Cloud que usarán es el Servidor de Nombres Eureka. Este permitirá que nuestros microservicios se registren con el servidor de nombres y luego otros microservicios pueden usar el servidor de nombres para obtener los detalles de los microservicios con los que desean comunicarse y comunicarse fácilmente con ellos.

Así que se trata de la registración y el descubrimiento de microservicios.

A continuación, usarán el componente de cliente Feign de Spring Cloud, que facilita la creación de clientes RESTful en lugar de usar el template REST de Spring. Si utilizan el cliente Feign, simplemente crean una interfaz y también pueden usar anotaciones como @RequestMapping y más, que anteriormente usábamos en el lado del servidor.

Así que hacen gestión declarativa de clientes RESTful utilizando el cliente Feign. Luego, realizarán balanceo de carga del lado del cliente utilizando el componente Ribbon. Ribbon nos permite distribuir nuestras solicitudes entre diferentes instancias del mismo microservicio que se están ejecutando.

Así que la gestión del lado del cliente se puede hacer fácilmente utilizando el componente Ribbon. La tolerancia a fallos es muy importante para cualquier microservicio, y si un microservicio está caído, todos sus clientes no deberían simplemente lanzar excepciones a sus clientes. Así que aprenderán a usar el componente Hystrix y manejar excepciones o fallos de manera elegante.

A continuación, está el proxy Zuul, que nos ayudará a hacer balanceo de carga del lado del servidor, que es un efecto secundario, pero el uso principal del proxy Zuul es que todas las solicitudes de microservicios pueden pasar a través de este proxy Zuul, donde podemos abordar preocupaciones transversales como la seguridad.

Cualquier cosa que deseen a través de sus microservicios se puede poner en este proxy Zuul, así que aprenderemos a crear un servidor proxy Zuul y luego configurar todos sus microservicios para usar el proxy Zuul. Cuando tenemos muchos microservicios funcionando dentro de la organización, debemos ser muy cuidadosos en lo que respecta al rastreo.

¿De dónde proviene la solicitud? ¿A dónde va? Todo eso debe ser gestionado fácilmente. Así que Sleuth es un componente que nos ayuda con el rastreo distribuido. Una vez que configuren Sleuth para sus microservicios, pueden rastrear de dónde ha originado la solicitud, a qué microservicio ha ido y todo eso se puede rastrear fácilmente utilizando el ID de rastreo que genera Sleuth. Además, Zipkin tomará todos los registros de Sleuth y los mostrará en un panel centralizado o en una bonita interfaz de usuario, por lo que será muy fácil rastrear y ver los rastros utilizando Zipkin.

Una vez que tengan todos estos fundamentos, pasarán a dominar la gestión de configuraciones y también el streaming de datos utilizando Spring Cloud.

---

**Resumen:**

El documento es una introducción a un curso sobre Spring Cloud, presentado por Barath T.P. Reddy. El curso cubre los fundamentos de Spring Cloud, incluyendo su importancia y los componentes clave como Eureka, Feign, Ribbon, Hystrix, Zuul, Sleuth y Zipkin. Se enseñará a crear microservicios que se comuniquen entre sí, gestionar la registración y descubrimiento de microservicios, balanceo de carga, tolerancia a fallos y rastreo distribuido. Al final, los participantes aprenderán sobre la gestión de configuraciones y el streaming de datos en Spring Cloud.