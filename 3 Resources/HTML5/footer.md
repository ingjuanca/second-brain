
  ---

## 🧱 ¿Qué es `<footer>`?

La etiqueta `<footer>` es un **elemento semántico de bloque** que representa el **pie de página** de un documento o sección. Se utiliza para contener información complementaria como:

- Créditos
- Derechos de autor
- Enlaces de contacto
- Mapa del sitio
- Enlaces legales o de privacidad

> 📌 Puede haber **más de un `<footer>`** en una página: uno general para el sitio y otros dentro de secciones o artículos.

---

## 🧠 ¿Por qué usar `<footer>`?

- Mejora la **estructura semántica** del documento.
- Ayuda a los motores de búsqueda y tecnologías asistivas a identificar contenido de cierre o complementario.
- Es ideal para **SEO** y **accesibilidad**.

---

## ✅ Ejemplo de uso básico

```html
<footer>
  <p>&copy; 2025 Mi Sitio Web. Todos los derechos reservados.</p>
  <nav>
    <ul>
      <li><a href="/politica-privacidad">Política de privacidad</a></li>
      <li><a href="/terminos">Términos y condiciones</a></li>
      <li><a href="/contacto">Contacto</a></li>
    </ul>
  </nav>
</footer>
```

---

## 🧩 Ejemplo dentro de un artículo

```html
<article>
  <h2>Cómo usar HTML5</h2>
  <p>HTML5 introduce nuevas etiquetas semánticas que mejoran la estructura del contenido...</p>
  <footer>
    <p>Publicado por Juan Pérez el 22 de junio de 2025</p>
  </footer>
</article>
```

---

## 📝 Buenas prácticas

- Usa `<footer>` para **cerrar secciones** o el documento completo.
- Incluye enlaces útiles, información legal o de contacto.
- No lo uses para contenido principal o navegación general (eso va en `<main>` o `<nav>`).

---