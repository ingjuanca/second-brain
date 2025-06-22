Aquí tienes una tabla con algunas de las **etiquetas que quedaron obsoletas (deprecated)** o **eliminadas** en HTML5, junto con su propósito original y la alternativa recomendada:

---

### 🗂️ **Etiquetas Obsoletas en HTML5**

| Etiqueta HTML | Propósito Original                                | Alternativa en HTML5                       |
| ------------- | ------------------------------------------------- | ------------------------------------------ |
| `<acronym>`   | Definir acrónimos                                 | Usa `<abbr>`                               |
| `<applet>`    | Insertar applets de Java                          | Usa `<object>` o tecnologías modernas      |
| `<basefont>`  | Definir fuente base para el documento             | Usa CSS (`font-family`, `font-size`, etc.) |
| `<big>`       | Texto más grande                                  | Usa CSS (`font-size`)                      |
| `<center>`    | Centrar contenido                                 | Usa CSS (`text-align: center`)             |
| `<dir>`       | Lista de directorios                              | Usa `<ul>`                                 |
| `<font>`      | Estilo de fuente (tipo, tamaño, color)            | Usa CSS                                    |
| `<frame>`     | Definir un marco en un frameset                   | Usa `<iframe>` o diseño con CSS            |
| `<frameset>`  | Contenedor de marcos                              | Usa `<iframe>` o diseño con CSS            |
| `<noframes>`  | Contenido alternativo para navegadores sin frames | Ya no es necesario                         |
| `<isindex>`   | Campo de búsqueda único                           | Usa `<form>` con `<input type="search">`   |
| `<strike>`    | Texto tachado                                     | Usa `<del>` o CSS (`text-decoration`)      |
| `<tt>`        | Texto con fuente monoespaciada                    | Usa CSS (`font-family: monospace`)         |
| `<u>`         | Subrayado (antes se usaba para énfasis visual)    | Usa CSS (`text-decoration: underline`)     |
| `<xmp>`       | Mostrar texto sin interpretar HTML                | Usa `<pre>` o escapar caracteres           |

---

Estas etiquetas fueron eliminadas o desaconsejadas porque **mezclaban contenido con presentación**, lo cual va en contra de las buenas prácticas modernas de desarrollo web, que promueven la separación entre estructura (HTML), estilo (CSS) y comportamiento (JavaScript).