  ---

## 🧱 ¿Qué es `<section>`?

La etiqueta `<section>` es un **elemento semántico de bloque** que se utiliza para **agrupar contenido temáticamente relacionado** dentro de una página web. Cada sección debe tener un propósito claro y, preferiblemente, un encabezado.

> 📌 **Importante:** No se debe usar `<section>` solo para agrupar contenido visualmente. Su uso debe tener sentido semántico.

---

## 🧠 ¿Cuándo usar `<section>`?

- Para dividir el contenido principal en **partes temáticas**.
- Dentro de `<article>`, `<main>`, o incluso otras `<section>`.
- Cada `<section>` debe tener al menos un **encabezado** (`<h1>` a `<h6>`).

---

## ✅ Ejemplo de uso

```html
<main>
  <section id="quienes-somos">
    <h2>¿Quiénes somos?</h2>
    <p>Somos una empresa dedicada al desarrollo de software personalizado.</p>
  </section>
  <section id="servicios">
    <h2>Servicios</h2>
    <ul>
      <li>Desarrollo web</li>
      <li>Aplicaciones móviles</li>
      <li>Consultoría tecnológica</li>
    </ul>
  </section>
  <section id="contacto">
    <h2>Contacto</h2>
    <p>Escríbenos a contacto@ejemplo.com o llámanos al 123-456-7890.</p>
  </section>
</main>
```

---

## 🧩 Diferencia con otras etiquetas similares

| Etiqueta    | Uso principal                                             |
| ----------- | --------------------------------------------------------- |
| `<div>`     | Agrupación genérica sin significado semántico             |
| `<article>` | Contenido independiente y reutilizable (como una noticia) |
| `<section>` | Agrupación temática dentro del contenido principal        |

---

## 🧠 Buenas prácticas

- Usa `<section>` solo si el contenido tiene un **tema claro** y puede tener un encabezado.
- No lo uses como reemplazo de `<div>` si no hay un propósito semántico.
- Mejora la **accesibilidad** y el **SEO** al estructurar correctamente el contenido.

---