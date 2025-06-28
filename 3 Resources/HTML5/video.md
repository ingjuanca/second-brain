
---

## 🎥 ¿Qué es `<video>`?

La etiqueta `<video>` se utiliza para **insertar contenido de video** directamente en una página web, sin necesidad de plugins externos como Flash. Fue introducida en HTML5 y permite controlar la reproducción del video mediante atributos o JavaScript.

---

## 🧩 Atributos comunes

|Atributo|Descripción|
|---|---|
|`src`|Ruta del archivo de video (puede omitirse si se usan `<source>`)|
|`controls`|Muestra los controles de reproducción (play, pausa, volumen, etc.)|
|`autoplay`|Reproduce el video automáticamente al cargar la página|
|`loop`|Reproduce el video en bucle|
|`muted`|Inicia el video sin sonido|
|`poster`|Imagen que se muestra antes de que el video comience|
|`preload`|Indica si el video debe cargarse al cargar la página (`auto`, `metadata`, `none`)|
|`width` / `height`|Dimensiones del reproductor de video|

---

## ✅ Ejemplo básico

```html
<video src="videos/ejemplo.mp4" controls width="640" height="360">
  Tu navegador no soporta la etiqueta de video.
</video>
```

---

## ✅ Ejemplo con múltiples fuentes y `poster`

```html
<video controls width="640" height="360" poster="imagenes/portada.jpg">
  <source src="videos/ejemplo.mp4" type="video/mp4">
  <source src="videos/ejemplo.webm" type="video/webm">
  Tu navegador no soporta la reproducción de video.
</video>
```

> 🔍 Esto permite que el navegador elija el formato que mejor soporte.

---

## 🧠 Buenas prácticas

- Usa múltiples formatos (`.mp4`, `.webm`, `.ogg`) para mayor compatibilidad.
- Incluye el atributo `controls` para que el usuario tenga control sobre la reproducción.
- Usa `poster` para mostrar una imagen previa atractiva.
- Evita `autoplay` a menos que el video esté silenciado (`muted`), ya que puede afectar la experiencia del usuario.

---