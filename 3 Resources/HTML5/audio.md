
---
## 🔊 ¿Qué es `<audio>`?

La etiqueta `<audio>` se utiliza para **insertar contenido de audio** en una página web. Fue introducida en HTML5 y permite reproducir archivos de sonido sin necesidad de plugins externos como Flash.

---

## 🧩 Atributos comunes

| Atributo   | Descripción                                                                       |
| ---------- | --------------------------------------------------------------------------------- |
| `src`      | Ruta del archivo de audio (puede omitirse si se usan `<source>`)                  |
| `controls` | Muestra los controles de reproducción (play, pausa, volumen, etc.)                |
| `autoplay` | Reproduce el audio automáticamente al cargar la página                            |
| `loop`     | Reproduce el audio en bucle                                                       |
| `muted`    | Inicia el audio en silencio                                                       |
| `preload`  | Indica si el audio debe cargarse al cargar la página (`auto`, `metadata`, `none`) |

---

## ✅ Ejemplo básico

```html
<audio src="audio/musica.mp3" controls>
  Tu navegador no soporta la etiqueta de audio.
</audio>
```

---

## ✅ Ejemplo con múltiples fuentes

```html
<audio controls>
  <source src="audio/musica.mp3" type="audio/mpeg">
  <source src="audio/musica.ogg" type="audio/ogg">
  Tu navegador no soporta la reproducción de audio.
</audio>
```

> 🔍 Esto permite que el navegador elija el formato que mejor soporte.

---

## 🧠 Buenas prácticas

- Siempre incluye el atributo `controls` para que el usuario tenga control sobre la reproducción.
- Usa múltiples formatos (`.mp3`, `.ogg`) para mayor compatibilidad.
- Incluye un mensaje alternativo para navegadores antiguos.
- No uses `autoplay` sin una buena razón, ya que puede afectar la experiencia del usuario.

---