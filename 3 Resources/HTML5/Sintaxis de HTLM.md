La **sintaxis de HTML (HyperText Markup Language)** se basa en el uso de **etiquetas** (tags) para estructurar y presentar contenido en la web. Aquí te explico los elementos clave de su sintaxis:

---

### 🧱 **Estructura Básica de un Documento HTML**

```html
<!DOCTYPE html>

<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Título de la página</title>
</head>
<body>
  <h1>Encabezado principal</h1>
  <p>Este es un párrafo de ejemplo.</p>
</body>
</html>
```

---

### 🧩 **Explicación de cada parte**

| Sección                               | Código                         | Descripción                                                                                                                                                 |
| ------------------------------------- | ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Declaración del tipo de documento** | `<!DOCTYPE html>`              | Indica al navegador que el documento está escrito en HTML5. Es obligatorio y debe ir al principio.                                                          |
| **Elemento raíz**                     | `<html lang="es">`             | Contenedor principal de todo el documento HTML. El atributo `lang="es"` indica que el contenido está en español.                                            |
| **Encabezado del documento**          | `<head>...</head>`             | Contiene metadatos (información sobre el documento), enlaces a hojas de estilo, scripts, y el título de la página. No se muestra directamente en la página. |
| **Codificación de caracteres**        | `<meta charset="UTF-8">`       | Define la codificación de caracteres. `UTF-8` permite usar la mayoría de los caracteres de todos los idiomas.                                               |
| **Título de la página**               | `<title>Mi Página Web</title>` | Aparece en la pestaña del navegador. Es importante para SEO y accesibilidad.                                                                                |
| **Cuerpo del documento**              | `<body>...</body>`             | Contiene todo el contenido visible de la página: texto, imágenes, enlaces, formularios, etc.                                                                |
| **Contenido visible**                 | `<h1>`, `<p>`, etc.            | Elementos dentro del `<body>` que representan encabezados, párrafos, listas, imágenes, etc.                                                                 |

---

### 🔤 **Elementos de la Sintaxis HTML**

| Elemento          | Descripción                                                                                                                                            |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `<!DOCTYPE html>` | Declara el tipo de documento (HTML5 en este caso).                                                                                                     |
| `<etiqueta>`      | Marca el inicio de un elemento.                                                                                                                        |
| `</etiqueta>`     | Marca el final de un elemento.                                                                                                                         |
| `<etiqueta />`    | Etiqueta autocontenida (por ejemplo, `<img />`, aunque en HTML5 no es obligatorio cerrar así).                                                         |
| **Atributos**     | Proporcionan información adicional sobre un elemento. Se escriben dentro de la etiqueta de apertura. Ejemplo: `<img src="foto.jpg" alt="Descripción">` |

---

### 🧩 **Ejemplo con Atributos**

- `href`: URL de destino.
- `target="_blank"`: Abre el enlace en una nueva pestaña.

---

### ✅ **Buenas Prácticas**

- Usar etiquetas semánticas (`<header>`, `<main>`, `<footer>`, etc.).
- Cerrar correctamente las etiquetas.
- Anidar correctamente los elementos.
- Usar comillas para los valores de los atributos.

---
### 📝 Notas adicionales

- Todo documento HTML debe tener **una sola etiqueta `<html>`**, que contiene las secciones `<head>` y `<body>`.
- El orden correcto de estas secciones es importante para que los navegadores interpreten bien el contenido.
- Puedes incluir hojas de estilo (`<link>`), scripts (`<script>`), íconos (`<link rel="icon">`), y más dentro del `<head>`.
