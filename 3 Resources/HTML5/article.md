
---

## 🧱 ¿Qué es `<article>`?

La etiqueta `<article>` es un **elemento semántico de bloque** que representa un **contenido independiente y autocontenible** dentro de un documento. Está pensado para contenido que **puede distribuirse o reutilizarse de forma independiente**, como:

- Publicaciones de blog
- Noticias
- Comentarios
- Entradas de foros
- Productos en una tienda

> 📌 Cada `<article>` debe tener su propio **encabezado** y, opcionalmente, un `<footer>` con información adicional (autor, fecha, etc.).

---

## 🧠 ¿Por qué usar `<article>`?

- Mejora la **estructura semántica** del documento.
- Ayuda a los motores de búsqueda a identificar contenido relevante.
- Mejora la **accesibilidad** para lectores de pantalla.

---

## ✅ Ejemplo de uso

```html
<main>
  <article>
    <header>
      <h2>¿Qué es HTML5?</h2>
      <p>Publicado el 22 de junio de 2025 por Juan Pérez</p>
    </header>
    <p>HTML5 es la última versión del lenguaje de marcado HTML. Introduce nuevas etiquetas semánticas, soporte para multimedia y APIs modernas.</p>
    <footer>
      <p>Categoría: Desarrollo Web</p>
    </footer>
  </article>
  <article>
    <header>
      <h2>Ventajas del diseño responsive</h2>
      <p>Publicado el 20 de junio de 2025 por Ana Gómez</p>
    </header>
    <p>El diseño responsive permite que los sitios web se adapten a diferentes tamaños de pantalla, mejorando la experiencia del usuario en móviles y tablets.</p>
    <footer>
      <p>Categoría: Diseño Web</p>
    </footer>
  </article>
</main>
```

---

## 🧩 Diferencia con `<section>`

|Etiqueta|Uso principal|
|---|---|
|`<article>`|Contenido **independiente** y reutilizable|
|`<section>`|Agrupación **temática** dentro del contenido|

---

## 📝 Buenas prácticas

- Usa `<article>` solo si el contenido **tiene sentido por sí solo**.
- Incluye un `<header>` con un título (`<h1>` a `<h6>`) y metadatos si es necesario.
- Puedes anidar `<section>` dentro de `<article>` si hay subtemas.

---