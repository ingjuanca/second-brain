
---


> Comencemos ahora a profundizar en la programación _front-end_.
> 
> Como mencioné anteriormente, el _front-end_ es esencialmente el diseño y la estructura de tu aplicación. Y tradicionalmente, el desarrollo de aplicaciones _front-end_ siempre se ha construido sobre tres pilares principales.
> 
> El primero es HTML, que es esencialmente el esqueleto de la página web. Así que puedes pensar que todo lo relacionado con la estructura se vincula con HTML. Después, CSS es la vestimenta y el maquillaje de tu página web; se encarga por completo del diseño visual y la disposición. Por lo tanto, todo lo que tiene que ver con el estilo y la distribución de tu aplicación web está contenido dentro de CSS. Y luego JavaScript es, en esencia, donde ocurre la magia. Es algo así como el cerebro detrás de todas las interacciones y animaciones geniales que verás típicamente dentro de una aplicación web. Por ejemplo, si pasas el cursor sobre el ícono de un menú y este cambia de tamaño o de color, eso sería JavaScript.
> 
> En el pasado, básicamente mantenías HTML, CSS y JavaScript separados. Tenías archivos individuales para HTML, para CSS y para JavaScript, y los programabas por separado. Pero ahora, en el desarrollo _front-end_ moderno, esencialmente combinas los tres. Antes, si querías editar el diseño visual, editabas archivos CSS. Si querías editar la estructura de tu página web, editabas archivos HTML. Y si querías editar interacciones y animaciones, editabas JavaScript.
> 
> Sin embargo, ahora hay un nuevo marco de trabajo llamado React, que fundamentalmente ha modernizado el _front-end_. React es una biblioteca que combina estos tres lenguajes. React realmente une HTML, CSS y JavaScript; no requiere que estos tres estén separados. Simplemente combinamos los tres y utilizamos únicamente el lenguaje JavaScript, y luego usamos la biblioteca React para simplificar el desarrollo. React es también lo que utilizaremos principalmente cuando construyamos nuestras aplicaciones con IA.
> 
> Con React, como mencioné, esencialmente estamos combinando HTML y CSS con una biblioteca y utilizando JavaScript. Por lo tanto, la estructura o el HTML se ve como HTML, pero en realidad es JavaScript. Y el diseño con CSS también se aplica utilizando JavaScript. Debido a todo esto, React simplifica la programación _front-end_. Básicamente, se le considera un estándar de la industria, especialmente cuando estás gestionando aplicaciones grandes que necesitan reutilización de código.
> 
> Algo que debes recordar aquí es que, aunque React puede manejar mucho, muchos desarrolladores todavía utilizan un archivo CSS independiente o soluciones de "CSS en JS" para necesidades de diseño más complejas. De manera que si estás construyendo con IA y estás usando React, pero notas que también tienen un archivo CSS separado, esto es bastante común y es solo para cuando necesitas un diseño complejo dentro de tu aplicación web. Esto es algo que debes recordar y tomar en cuenta cuando construyas con React.
> 
> También notarás esto cuando estés observando el código generado por IA, pero la principal belleza de esta forma moderna de programación _front-end_ es que permite el desarrollo basado en componentes. Lo que esto significa es que, si imaginas la construcción de tu aplicación como un Lego, cada pieza es autónoma pero encaja perfectamente con las demás.
> 
> Esto significa que si fueras a construir, por ejemplo, una nave de Star Wars en un juego de Lego, las alas serían un componente. La cabina sería un componente. El casco principal de la nave también sería un componente. Llevándolo a un ejemplo del mundo real, tus encabezados (_headers_) y pies de página (_footers_) podrían ser un componente dentro del desarrollo _front-end_. Y luego puedes reutilizar estos encabezados y pies de página en cualquier otra aplicación diferente que vayas a realizar. Si tuvieras una sección de llamada a la acción (_call to action_) dentro de tu página de destino (_landing page_), esto también sería un componente.
> 
> Y si no deseas tener componentes para cada sección de la página (un componente para el encabezado, otro para la sección principal y otro para la llamada a la acción), puedes hacer que toda tu página de inicio o página de destino sea un componente. Así, esencialmente puedes reutilizar esta página de destino, o todos los estilos dentro de ella, en la siguiente aplicación que construyas con React. Todo esto sirve fundamentalmente para darte contexto cuando comiences a generar código con React. Notarás que todos estos códigos de _front-end_ están básicamente autocontenidos y divididos en estos componentes.

## 💡 Ideas más relevantes

- **Los tres pilares tradicionales:** Históricamente, el _front-end_ se ha dividido en tres tecnologías independientes: **HTML** (la estructura o esqueleto), **CSS** (el aspecto visual, estilo y maquillaje) y **JavaScript** (el cerebro a cargo del dinamismo, las interacciones y animaciones).
    
- **La revolución de React:** React modernizó el desarrollo al integrar HTML, CSS y JavaScript en una sola biblioteca basada puramente en JavaScript, convirtiéndose en el estándar de la industria y en la herramienta principal que se usará junto con la IA en el curso.
    
- **Convivencia con archivos CSS:** Aunque React integra los estilos visuales, es un proceso estándar y muy común que la IA genere archivos CSS independientes o soluciones híbridas si la aplicación web requiere un diseño visual muy complejo.
    
- **Desarrollo basado en componentes:** React introduce una metodología similar a las piezas de Lego, donde el código se divide en bloques independientes, autónomos y reutilizables (como el encabezado de una página, una sección de llamada a la acción o una página web entera). Esto facilita enormemente reciclar estructuras completas para proyectos futuros.