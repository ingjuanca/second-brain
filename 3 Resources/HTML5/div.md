
---

## 🧱 ¿Qué es `<div>`?

La etiqueta `<div>` (abreviatura de _division_) es un **elemento de bloque genérico** que se usa para **agrupar contenido** y aplicar estilos o scripts. A diferencia de etiquetas semánticas como `<section>` o `<article>`, `<div>` **no tiene significado semántico por sí misma**.

> 📌 Se usa principalmente como **contenedor estructural** cuando no hay una etiqueta semántica más adecuada.

---

## 🧠 ¿Cuándo usar `<div>`?

- Para agrupar elementos que comparten un estilo o comportamiento.
- Para crear estructuras de diseño (columnas, cajas, contenedores).
- Como base para aplicar estilos con CSS o manipulación con JavaScript.

---

## ✅ Ejemplo básico de uso

```html
<div>
  <h2>Bienvenido</h2>
  <p>Este es el contenido principal de la página.</p>
</div>
```
---

## 🎨 Ejemplo con estilo CSS

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Ejemplo con div</title>
  <style>
    .caja {
      background-color: #f0f0f0;
      padding: 20px;
      border: 1px solid #ccc;
      margin-bottom: 20px;
    }
  </style>
</head>
<body>
  <div class="caja">
    <h2>Sección 1</h2>
    <p>Contenido de la primera sección.</p>
  </div>
  <div class="caja">
    <h2>Sección 2</h2>
    <p>Contenido de la segunda sección.</p>
  </div>
</body>
</html>
```

---

## 🧩 Diferencia con etiquetas semánticas

|Etiqueta|¿Tiene significado semántico?|Uso recomendado|
|---|---|---|
|`<div>`|❌ No|Agrupación genérica|
|`<section>`|✅ Sí|Agrupación temática|
|`<article>`|✅ Sí|Contenido independiente|
|`<aside>`|✅ Sí|Contenido complementario|

---

## 📝 Buenas prácticas

- Usa `<div>` solo cuando **no haya una etiqueta semántica más adecuada**.
- Asigna clases o identificadores (`class`, `id`) para aplicar estilos o scripts.
- No abuses de `<div>` innecesariamente (lo que se conoce como _divitis_).

---