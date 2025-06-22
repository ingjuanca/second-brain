A continuación te presento un **árbol jerárquico simplificado** que muestra qué tipo de contenido puede contener cada etiqueta HTML común. Esto te ayudará a visualizar cómo se estructura un documento HTML y qué elementos pueden anidarse dentro de otros.

---

### 🌳 **Árbol de Contenido HTML Simplificado**

```
<!DOCTYPE html>
└── <html>
    ├── <head>
    │   ├── <title>
    │   ├── <meta>
    │   ├── <link>
    │   ├── <style>
    │   └── <script>
    └── <body>
        ├── <header>
        │   ├── <h1> a <h6>
        │   └── <nav>
        │       └── <a>
        ├── <main>
        │   ├── <section>
        │   │   ├── <h1> a <h6>
        │   │   ├── <p>, <ul>, <ol>, <article>, <figure>, etc.
        │   └── <article>
        │       ├── <header>, <h1>, <p>, <img>, <video>, etc.
        │       └── <footer>
        ├── <aside>
        │   └── <p>, <ul>, <nav>, etc.
        ├── <figure>
        │   ├── <img>, <video>, <canvas>
        │   └── <figcaption>
        ├── <footer>
        │   └── <p>, <nav>, <small>, etc.
        ├── <form>
        │   ├── <input>, <label>, <textarea>, <button>, <select>, <fieldset>
        ├── <table>
        │   ├── <thead>, <tbody>, <tfoot>
        │   ├── <tr>
        │   │   ├── <th>, <td>
        └── Otros elementos:
            ├── <p>, <div>, <span>, <br>, <hr>
            ├── <audio>, <video>, <canvas>, <iframe>
            ├── <ul>, <ol>, <li>
            ├── <strong>, <em>, <mark>, <code>, <blockquote>
```

---

### 📝 Notas importantes:

- **Etiquetas de bloque** como `<div>`, `<section>`, `<article>`, `<header>`, etc., pueden contener otras etiquetas de bloque o en línea.
- **Etiquetas en línea** como `<span>`, `<a>`, `<strong>`, `<em>`, etc., solo deben contener texto o elementos en línea.
- Algunas etiquetas como `<img>`, `<input>`, `<br>`, y `<hr>` son **autocontenidas** (no tienen contenido interno).

---