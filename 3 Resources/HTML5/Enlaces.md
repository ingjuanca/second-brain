La etiqueta `<a>` (de _anchor_, ancla) se usa para crear enlaces que pueden llevar a:

- Otras páginas web
- Archivos
- Correos electrónicos
- Secciones dentro de la misma página
- Scripts o acciones personalizadas

```html
<header>
    <h1>Mi Sitio Web</h1>
    <nav>
      <ul>
        <li><a href="#inicio">Inicio</a></li>
        <li><a href="#servicios">Servicios</a></li>
        <li><a href="#portafolio">Portafolio</a></li>
        <li><a href="#contacto">Contacto</a></li>
      </ul>
    </nav>
  </header>
```

---

## 🧩 **Atributos comunes de `<a>`**

|Atributo|Descripción|
|---|---|
|`href`|URL o destino del enlace|
|`target`|Define dónde se abrirá el enlace (`_blank`, `_self`, etc.)|
|`title`|Texto que aparece al pasar el cursor|
|`download`|Indica que el enlace descarga un archivo|
|`rel`|Relación entre el documento actual y el destino (útil para SEO y seguridad)|

---

### 🆚 Comparación con otros valores de `target`

| Valor     | Comportamiento                                          |
| --------- | ------------------------------------------------------- |
| `_self`   | Abre en la misma pestaña (predeterminado)               |
| `_blank`  | Abre en una nueva pestaña o ventana                     |
| `_parent` | Abre en el marco padre (si se usan frames)              |
| `_top`    | Abre en la ventana completa, eliminando cualquier frame |

---

## ✅ **Ejemplos de uso de `<a>`**

### 1. Enlace a otra página web

```html
<a href="https://www.ejemplo.com">Visitar Ejemplo</a>
```

### 2. Enlace que se abre en una nueva pestaña

```html
<a href="https://www.ejemplo.com" target="_blank">Abrir en nueva pestaña</a>
```

### 3. Enlace a una sección de la misma página
```html
<a href="#contacto">Ir a la sección de contacto</a>
  
<!-- Más abajo en la misma página -->
<section id="contacto">
  <h2>Contacto</h2>
</section>
```

### 4. Enlace para descargar un archivo

```html
<a href="documento.pdf" download>Descargar PDF</a>
```

### 5. Enlace para enviar un correo electrónico

```html
<a href="mailto:correo@ejemplo.com">Enviar correo</a>
```

### 6. Enlace con título y relación

```html
<a href="https://www.ejemplo.com" title="Ir al sitio de ejemplo" rel="noopener noreferrer">Ejemplo</a>
```

---

## 🧠 Buenas prácticas para SEO y accesibilidad

- Usa **texto descriptivo** en el enlace (evita "haz clic aquí").
- Usa `rel="noopener noreferrer"` con `target="_blank"` por seguridad.
- Usa `title` para mejorar la accesibilidad.
- Asegúrate de que los enlaces funcionen correctamente y no estén rotos.