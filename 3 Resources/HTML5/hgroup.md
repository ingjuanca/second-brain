La etiqueta `<hgroup>` se utiliza para **agrupar un conjunto de encabezados relacionados**, como un título principal y un subtítulo. Su propósito es indicar que los encabezados dentro del grupo forman una **unidad semántica**.

> 📌 **Importante:** Aunque fue parte de la especificación inicial de HTML5, **`<hgroup>` fue desaconsejada (deprecated)** por el W3C y **no es ampliamente soportada** por los navegadores ni recomendada en proyectos modernos.

---

### 🧠 ¿Cuándo se usaba?

Se usaba cuando un título principal (`<h1>`) y un subtítulo (`<h2>`, `<h3>`, etc.) debían considerarse como un solo encabezado para propósitos semánticos y de accesibilidad.

---

### 🧩 Ejemplo de uso de `<hgroup>`

```html
<header>
  <hgroup>
    <h1>Universidad Nacional</h1>
    <h2>Facultad de Ingeniería</h2>
  </hgroup>
</header>
```

En este ejemplo:

- `<h1>` es el título principal.
- `<h2>` es un subtítulo.
- Ambos están agrupados como una unidad semántica.

---

### ⚠️ Estado actual

- **No recomendado** en HTML5 moderno.
- En su lugar, se recomienda usar simplemente los encabezados jerárquicos sin `<hgroup>`, o agruparlos con `<header>` o `<div>` si se necesita estilo o estructura adicional.

---

### ✅ Alternativa recomendada

```html
<header>
  <h1>Universidad Nacional</h1>
  <h2>Facultad de Ingeniería</h2>
</header
```

---
