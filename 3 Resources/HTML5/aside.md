
---

## 🧱 ¿Qué es `<aside>`?

La etiqueta `<aside>` es un **elemento semántico de bloque** que se utiliza para representar **contenido complementario o secundario** al contenido principal de la página o sección.

> 📌 El contenido dentro de `<aside>` debe estar relacionado con el contenido que lo rodea, pero **no es parte esencial** del flujo principal.

---

## 🧠 ¿Cuándo usar `<aside>`?

- Barras laterales con enlaces relacionados
- Biografías de autores en artículos
- Citas destacadas o notas informativas
- Publicidad o widgets
- Listas de artículos relacionados

---

## ✅ Ejemplo de uso

```html
<main>
  <article>
    <h2>Cómo mejorar el rendimiento web</h2>
    <p>Optimizar imágenes, reducir scripts innecesarios y usar caché son algunas de las mejores prácticas...</p>
    <aside>
      <h3>Consejo rápido</h3>
      <p>Utiliza formatos de imagen modernos como WebP para reducir el tamaño sin perder calidad.</p>
    </aside>
  </article>
</main>
```

En este ejemplo:

- `<aside>` contiene un **consejo relacionado** con el artículo, pero no forma parte del contenido principal.
- Ayuda a **enriquecer la experiencia del usuario** sin interrumpir la lectura.

---

## 🧩 Diferencia con otras etiquetas

| Etiqueta    | Uso principal                            |
| ----------- | ---------------------------------------- |
| `<section>` | Agrupación temática dentro del contenido |
| `<article>` | Contenido independiente y reutilizable   |
| `<aside>`   | Contenido complementario o contextual    |

---

## 📝 Buenas prácticas

- No usar `<aside>` para contenido irrelevante o decorativo.
- Asegúrate de que el contenido tenga **relación contextual** con el contenido principal.
- Puede colocarse dentro de `<article>`, `<main>`, o incluso en `<body>` como una barra lateral general.

---