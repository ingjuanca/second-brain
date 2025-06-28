
---

## 🖼️ 1. `<img>` – Imagen

### 📌 ¿Qué es?

La etiqueta `<img>` se usa para **insertar imágenes** en una página web. Es una **etiqueta autocontenida** (no tiene etiqueta de cierre).

### ✅ Atributos comunes:

|Atributo|Descripción|
|---|---|
|`src`|Ruta de la imagen (URL o archivo local)|
|`alt`|Texto alternativo (importante para accesibilidad y SEO)|
|`width` / `height`|Dimensiones de la imagen (opcional)|

### 🧪 Ejemplo:

```html
<img src="logo.png" alt="Logotipo de la empresa" width="200">
```

---

## 🧱 2. `<figure>` – Contenedor de contenido visual

### 📌 ¿Qué es?

La etiqueta `<figure>` se usa para **agrupar contenido visual** (como imágenes, gráficos, videos, etc.) junto con su descripción. Es un **elemento semántico de bloque**.

> Se usa cuando el contenido puede ser **referenciado de forma independiente** del flujo principal del texto.

---

## 📝 3. `<figcaption>` – Leyenda o descripción

### 📌 ¿Qué es?

La etiqueta `<figcaption>` proporciona una **descripción o leyenda** para el contenido dentro de `<figure>`. Debe colocarse como **primer o último hijo** de `<figure>`.

---

## ✅ Ejemplo completo de uso

```html
<figure>
  <img src="paisaje.jpg" alt="Paisaje montañoso al atardecer" width="600">
  <figcaption>Paisaje capturado en los Alpes suizos durante el otoño.</figcaption>
</figure
```

---

## 🧠 ¿Por qué es útil para SEO y accesibilidad?

- El atributo `alt` en `<img>` mejora la accesibilidad y permite que los motores de búsqueda comprendan el contenido visual.
- `<figure>` y `<figcaption>` **añaden contexto semántico**, lo que ayuda a los buscadores a entender mejor la relación entre la imagen y su descripción.

---

¿