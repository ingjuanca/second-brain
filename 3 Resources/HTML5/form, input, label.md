
---

## 🧾 ¿Qué es `<form>`?

La etiqueta `<form>` define un **formulario HTML** que permite a los usuarios **enviar datos** al servidor. Es un **contenedor** para campos de entrada, botones y otros elementos interactivos.

### 🔧 Atributos comunes de `<form>`:

|Atributo|Descripción|
|---|---|
|`action`|URL a la que se enviarán los datos del formulario|
|`method`|Método de envío: `GET` (visible en la URL) o `POST` (oculto)|
|`target`|Dónde se abrirá la respuesta (`_self`, `_blank`, etc.)|
|`autocomplete`|Habilita o desactiva el autocompletado (`on` / `off`)|

---

## 🔤 ¿Qué es `<input>`?

La etiqueta `<input>` define un **campo de entrada** dentro de un formulario. Es una **etiqueta autocontenida** y puede tener muchos tipos diferentes.

### 🔧 Atributos comunes de `<input>`:

|Atributo|Descripción|
|---|---|
|`type`|Tipo de entrada: `text`, `email`, `password`, `checkbox`, `radio`, `submit`, etc.|
|`name`|Nombre del campo (clave para enviar datos)|
|`value`|Valor por defecto del campo|
|`placeholder`|Texto de ayuda dentro del campo|
|`required`|Campo obligatorio|
|`readonly`, `disabled`|Solo lectura o deshabilitado|

---
### 🔤 ¿Qué es `<label>`?

La etiqueta `<label>` en HTML se utiliza para definir una **etiqueta descriptiva** para un elemento de formulario, como un campo de entrada (`<input>`), una lista desplegable (`<select>`), etc. Mejora la accesibilidad y la usabilidad, ya que al hacer clic en el texto del `<label>`, se activa el control asociado.

### 📌 Características clave:

- Se asocia a un elemento de formulario mediante el atributo `for`, cuyo valor debe coincidir con el `id` del elemento.
- También puede envolver directamente al elemento de formulario, sin necesidad del atributo `for`.
- El valor del atributo `for` debe coincidir con el valor del atributo `id` del elemento de formulario al que se refiere.

## ✅ Ejemplo completo de formulario

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Formulario de contacto</title>
</head>
<body>
  <h2>Contáctanos</h2>
  <form action="/enviar" method="POST">
    <label for="nombre">Nombre:</label><br>
    <input type="text" id="nombre" name="nombre" placeholder="Tu nombre" required><br><br>
    <label for="email">Correo electrónico:</label><br>
    <input type="email" id="email" name="email" placeholder="tucorreo@ejemplo.com" required><br><br>
    <label for="mensaje">Mensaje:</label><br>
    <textarea id="mensaje" name="mensaje" rows="4" cols="40" placeholder="Escribe tu mensaje aquí..." required></textarea><br><br>
    <input type="submit" value="Enviar">
  </form>
</body>
</html>
```

---
## 🧠 Buenas prácticas

- Usa `label` con `for` para mejorar la accesibilidad.
- Usa `required` para validar campos obligatorios sin JavaScript.
- Usa `type="email"`, `type="tel"`, etc., para mejorar la experiencia en dispositivos móviles.