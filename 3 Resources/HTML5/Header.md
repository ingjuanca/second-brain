La etiqueta `<header>` es una **etiqueta semántica de bloque** introducida en HTML5. Se utiliza para definir un **encabezado** para una sección o para todo el documento. Generalmente contiene:

- Títulos (`<h1>` a `<h6>`)
- Logotipos
- Menús de navegación (`<nav>`)
- Subtítulos o descripciones

> 🔸 **Importante:** Puede haber múltiples `<header>` en una página, por ejemplo, uno para el sitio completo y otros para secciones o artículos individuales.

---

### 🧩 Ejemplo de uso básico

```html
<header>
    <h1>Mi Sitio Web</h1>
    <p>Bienvenido a mi página personal</p>
    <nav>
      <ul>
        <li><a href="#inicio">Inicio</a></li>
        <li><a href="#servicios">Servicios</a></li>
        <li><a href="#contacto">Contacto</a></li>
      </ul>
    </nav>
</header>
```

---

### 📝 Buenas prácticas

- No debe usarse dentro de `<footer>`, `<address>`, o `<form>`.
- No se debe confundir con `<head>`, que contiene metadatos del documento.
- Puede repetirse en diferentes secciones para encabezados locales.