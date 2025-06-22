Vamos a detallar las etiquetas **`<nav>`**, **`<ul>`**, **`<ol>`** y **`<li>`**, y cómo usarlas correctamente para mejorar la **estructura, accesibilidad y SEO** de una página web.

---

## 🧭 1. `<nav>` – Navegación

### ¿Qué es?

La etiqueta `<nav>` define una **sección de navegación** que contiene enlaces a otras partes del sitio o a sitios externos.

### ¿Por qué es importante para el SEO?

- Ayuda a los motores de búsqueda a identificar los **enlaces de navegación principales**.
- Mejora la **accesibilidad** para lectores de pantalla.

---

## 📋 2. `<ul>` – Lista no ordenada

### ¿Qué es?

Crea una lista de elementos sin un orden específico. Se usa comúnmente para menús de navegación.

---

## 🔢 3. `<ol>` – Lista ordenada

### ¿Qué es?

Crea una lista numerada. Se usa cuando el orden de los elementos es importante (por ejemplo, pasos de un proceso).

---

## 🧩 4. `<li>` – Elemento de lista

### ¿Qué es?

Define un **elemento dentro de una lista** (`<ul>` o `<ol>`). Cada ítem del menú o lista va dentro de un `<li>`.

---

## 🧪 Ejemplo de uso correcto para SEO

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Ejemplo de Navegación</title>
</head>

<body>
  <header>
    <h1>Mi Sitio Web</h1>
    <!-- Menú de navegación principal -->
    <nav>
      <ul>
        <li><a href="#inicio">Inicio</a></li>
        <li><a href="#servicios">Servicios</a></li>
        <li><a href="#portafolio">Portafolio</a></li>
        <li><a href="#contacto">Contacto</a></li>
      </ul>
    </nav>
  </header>
  <main>
    <section id="inicio">
      <h2>Bienvenido</h2>
      <p>Contenido de la sección de inicio.</p>
    </section>
    <section id="servicios">
      <h2>Servicios</h2>
      <ol>
        <li>Diseño Web</li>
        <li>Desarrollo Frontend</li>
        <li>Optimización SEO</li>
      </ol>
    </section>
  </main>
</body>
</html>
```

---

### ✅ Buenas prácticas para SEO

- Usa `<nav>` solo para **bloques de navegación reales**.
- Usa `<ul>` para menús y `<ol>` para pasos o procesos.
- Asegúrate de que los enlaces tengan **texto descriptivo**.
- Usa atributos `id` en las secciones para que los enlaces naveguen correctamente.

---