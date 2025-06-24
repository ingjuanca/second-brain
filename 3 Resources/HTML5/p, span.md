 ---

## 🧱 1. `<p>` – Párrafo

### 📌 ¿Qué es?

La etiqueta `<p>` se utiliza para definir un **párrafo de texto**. Es un **elemento de bloque**, lo que significa que ocupa todo el ancho disponible y comienza en una nueva línea.

### ✅ Ejemplo:

```html
<p>Este es un párrafo de texto que se muestra como un bloque separado.</p>
```
### 🧠 Usos comunes:

- Agrupar oraciones relacionadas.
- Separar bloques de contenido textual.
- Mejora la legibilidad y estructura del contenido.

---

## 🧩 2. `<span>` – Contenedor en línea

### 📌 ¿Qué es?

La etiqueta `<span>` es un **elemento en línea** que se usa para aplicar estilo o identificar una parte específica del texto **sin alterar el flujo del contenido**.

### ✅ Ejemplo:

```html
<p>Este es un <span style="color: red;">texto en rojo</span> dentro de un párrafo.</p>
```

### 🧠 Usos comunes:

- Aplicar estilos CSS a una parte del texto.
- Marcar texto para manipulación con JavaScript.
- No tiene significado semántico por sí sola.

---

## 🔄 Comparación rápida

|Característica|`<p>` (párrafo)|`<span>` (en línea)|
|---|---|---|
|Tipo de elemento|De bloque|En línea|
|Comienza en nueva línea|✅ Sí|❌ No|
|Uso principal|Agrupar texto en párrafos|Aplicar estilo o lógica a partes del texto|
|Contenido permitido|Texto, elementos en línea|Solo texto o elementos en línea|

---

## 🧪 Ejemplo combinado

```html
<p>
  Bienvenido a nuestra página. Aquí puedes encontrar
  <span style="font-weight: bold; color: green;">ofertas especiales</span>
  y <span style="text-decoration: underline;">novedades exclusivas</span>.
</p>
```

Este ejemplo muestra cómo usar `<p>` para estructurar el texto y `<span>` para aplicar estilos a partes específicas.

---
