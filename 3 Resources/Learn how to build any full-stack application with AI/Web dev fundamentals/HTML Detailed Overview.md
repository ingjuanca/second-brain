
---

> Ahora revisemos la estructura de la página web o el código HTML. Para empezar, así es como se ve una estructura típica de HTML. Y básicamente proporciona la estructura de toda la página de tu sitio web.
> 
> Esta primera línea gris que solo dice `doctype`, básicamente te permite saber que esta es una página HTML. Y luego, a continuación, tenemos estos corchetes angulares. Tenemos un corchete angular y luego `HTML`. Y luego en la parte inferior (este es un concepto importante) hay un corchete angular de cierre. Así que siempre que hay una barra diagonal (_slash_), significa que esto está abierto y esto está cerrado. Todos los corchetes angulares se llaman etiquetas. Así que esta es una etiqueta HTML. Cada etiqueta necesita tener una apertura y un cierre. Con cada etiqueta tenemos un HTML y un cierre de HTML. Tenemos una etiqueta de apertura `head`, una etiqueta de cierre `head`, una etiqueta de apertura `body` y una etiqueta de cierre `body`. Lo que tenemos aquí dentro de estas etiquetas son etiquetas anidadas dentro de esta etiqueta principal. Puedes pensar en esta etiqueta HTML como, esencialmente, la etiqueta principal de la estructura HTML. Y luego, dentro de esta, tenemos muchas etiquetas. La primera etiqueta aquí es una etiqueta de encabezado (`head`). Y dentro de la etiqueta de encabezado tenemos una etiqueta de título (`title`). Aquí es donde va el título. Y debajo de esto tenemos una etiqueta `body`. Y este es todo el contenido de la página web que va dentro de la etiqueta `body`. Dentro de un elemento HTML puedes añadir cosas como botones, títulos, etc.
> 
> En términos de estructura se ve así dentro del archivo HTML: hay una etiqueta llamada HTML, y luego todo aquí está contenido dentro de la etiqueta HTML. Y ahora, más abajo, profundicemos un poco más en la estructura.
> 
> Esta etiqueta `head` almacena metadatos sobre tu sitio web. Justo ahora tenemos el título de nuestro sitio. Los metadatos son esencialmente la información que tú, como usuario, realmente no ves. Está en una especie de parte trasera (_back-end_) del sitio web. Este título es lo que se muestra en Google. Básicamente proporciona todos los datos de tu página web que en realidad no se muestran al usuario en tu sitio web, sino que se usan más que nada en el fondo para propósitos de SEO. Es decir, Google Search y Google Analytics. El título es solo el título de la página web cuando escribes el sitio en google.com.
> 
> A continuación, la etiqueta `body` es todo lo que es visible para el usuario. Todo lo que tenemos aquí que dice "el contenido de la página web va aquí". Todo dentro de los confines de esta etiqueta `body` de apertura y cierre es todo lo que el usuario ve en tu sitio web. Puedes poner un elemento de texto dentro de esta etiqueta `body` para ver el texto.
> 
> Ahora, la mejor manera de aprender con la IA aquí es simplemente hacer que la IA genere algo de código para ti; puedes ir a Bolt, V0 o incluso a Cursor para crear una página web simple. Y luego podrás ver cómo se utiliza todo este código _front-end_. Así es como normalmente leerías una estructura HTML muy estándar. De nuevo, no necesitamos perder el tiempo repasando la sintaxis específica para añadir títulos o texto de párrafo, ya que la IA es bastante buena con la sintaxis; solo depende de ti entender exactamente qué está sucediendo a un nivel alto. Solo debes saber que estas son etiquetas cuando se abren y cierran, y que estas son cosas contenidas dentro de algo así como una etiqueta madre o padre. Siempre y cuando conozcas la estructura de cómo funcionan estos archivos HTML en tu página web, sabrás qué está pasando en tu código a un nivel alto. Una vez más, la IA normalmente no cometerá errores aquí, pero es bueno conocer la estructura general de tu página web por tu propio bien.
> 
> Ahora repasemos cómo se ve un archivo HTML típico dentro de esta etiqueta `body`. Como mencioné, todo dentro de esta etiqueta `body` (donde dice "el contenido de la página web va aquí") es todo lo que vemos físicamente en el sitio. Tenemos una etiqueta `body` de apertura y cierre. En esta siguiente sección vemos que hay una etiqueta `body` de apertura y cierre. Todo este contenido dentro de la etiqueta `body` es esencialmente lo que estaría aquí. Solo lo estoy simplificando para mostrarte cómo se ve. Este es un archivo HTML muy típico. Si quisieras empezar una aplicación web con un título, simplemente añadimos una etiqueta `H1`, el nombre de nuestro título y luego cerramos la etiqueta `H1`. Este es el encabezado principal. Y también puedes usar etiquetas `H2` o `H3` para títulos más pequeños.
> 
> Y si quisiéramos añadir cualquier otra cosa, como un texto de párrafo o una imagen, podemos hacer lo mismo pero con etiquetas `div`. Puedes pensar en estas etiquetas `div` (esta es una etiqueta `div` de apertura y cierre) como contenedores que pueden albergar cualquier tipo de contenido. Si quisieras añadir algunos párrafos o una imagen y están relacionados, puedes pensar en este `div` como una caja que puede contener todo. Dentro de esta etiqueta `div` aquí estamos guardando una imagen. Y debajo de esta también estamos guardando una etiqueta `p`, que es básicamente una etiqueta de párrafo. Estas etiquetas `div` pueden albergar cualquier tipo de contenido. Pero esta etiqueta de imagen (`image`) solo puede albergar imágenes; tiene que estar en este formato de imagen. Y esta etiqueta `p` solo puede albergar texto; por eso solo hay texto dentro de esta etiqueta `p`. Con este `div` puedes albergar cualquier tipo de contenido, pero para las etiquetas específicas dentro del `div`, solo tiene que corresponder con la etiqueta que estés denotando aquí. De nuevo, `p` es solo texto e `image` es solo imágenes. Con esta etiqueta de imagen podemos ver que tenemos el `src`, que es básicamente la fuente (_source_) de la imagen o de dónde proviene la imagen. Y a la derecha de esto tenemos el texto alternativo (`alt`), que describe la imagen para propósitos de SEO.
> 
> Y como mencioné, la razón por la que mantenemos estas dos etiquetas juntas dentro de una etiqueta `div` es típicamente porque están relacionadas. Esto se vería probablemente como una imagen en la parte superior, y luego una descripción de la imagen justo debajo. Y debido a que están relacionadas, anidarías ambas dentro de la misma etiqueta `div`. Puedes ver que esta es una etiqueta `div` de apertura y una etiqueta `div` de cierre. Todo lo que está dentro es todo lo relacionado con esta etiqueta `div`.
> 
> Y ahora, moviéndonos hacia abajo, la última etiqueta que podrías ver comúnmente es también una etiqueta `a`. Esta es simplemente una etiqueta de enlace. Todo lo anidado, justo como cuando tenemos una etiqueta `div` (si esto fuera una etiqueta `a`, corchete de apertura `a` corchete de cierre `a`), es una etiqueta de enlace. Todo lo que está anidado dentro de esta etiqueta `a` es cliqueable. Esto también puede ser una imagen o texto. Y es por esto que es importante anidar las etiquetas correctamente. Para hacer que una imagen y un texto sean cliqueables hacia el mismo enlace, necesitas asegurarte de que ambas etiquetas hijas estén anidadas dentro de la etiqueta padre adecuada. Por esto estructuramos nuestros archivos HTML de la manera en que lo hacemos.
> 
> Y debajo de esto podemos ver que tenemos un `div` con solo contenido de texto. Esto es solo para mostrarte que un `div` puede contener cualquier cosa: puede ser imagen y texto, o puede ser solo texto. Y este es un `div` anidado con un enlace. Como mencioné, este es el enlace `a` con un `div` anidado. Y este sería el `div` principal. Y todo esto está anidado dentro de la etiqueta `body`.

## 💡 Ideas más relevantes

- **Regla de oro de las etiquetas (Tags):** La estructura de HTML se compone de etiquetas delimitadas por corchetes angulares, donde casi todas requieren obligatoriamente una etiqueta de apertura y otra de cierre (identificada por una barra diagonal `/`).
    
- **Anidación e importancia del orden:** Los elementos se organizan jerárquicamente mediante etiquetas "padres" que contienen etiquetas "hijas". Una correcta anidación es crucial; por ejemplo, para lograr que un bloque de texto y una imagen redirijan al mismo enlace, ambos elementos deben colocarse dentro de una etiqueta contenedora de enlace (`a`).
    
- **Secciones troncales del documento:**
    
    - **`doctype`**: Informa al navegador que el archivo corresponde a un documento HTML.
        
    - **`head`**: Guarda los metadatos y configuraciones invisibles para el usuario común, útiles para el posicionamiento en buscadores (SEO) y analítica, como la etiqueta `title` que indexa Google.
        
    - **`body`**: Representa el contenedor principal de todo el contenido visual y público que el usuario final experimenta en la interfaz del sitio web.
        
- **Etiquetas de contenido comunes:**
    
    - **`H1`, `H2`, `H3`**: Se emplean para definir títulos y encabezados de mayor a menor jerarquía visual.
        
    - **`p`**: Diseñada de forma exclusiva para albergar bloques de texto o párrafos.
        
    - **`image`**: Se usa solo para imágenes y cuenta con atributos como `src` (origen del archivo) y `alt` (texto descriptivo para SEO).
        
    - **`div`**: Funciona como una "caja" o contenedor multipropósito capaz de agrupar de forma lógica distintos tipos de elementos relacionados (como una imagen combinada con su descripción de texto).
        
- **Enfoque de aprendizaje práctico con IA:** No es necesario memorizar la sintaxis exacta de cada elemento en HTML porque las herramientas de IA (como Bolt, V0 o Cursor) son sumamente precisas escribiéndola. El rol prioritario del estudiante es comprender la lógica estructural a nivel macro para saber auditar el código.