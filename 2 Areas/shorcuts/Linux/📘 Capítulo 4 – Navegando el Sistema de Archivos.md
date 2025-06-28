
---
### 🎯 **Objetivo**

Aprender a moverte dentro del sistema de archivos de Linux utilizando la terminal. Aquí se introducen conceptos clave como:

- Rutas absolutas y relativas
- Directorio raíz `/`
- Directorio actual `.` y padre `..`
- Y varios comandos esenciales

---

## 🧰 **Comandos Introducidos**

|Comando|Descripción básica|Ejemplo|
|---|---|---|
|`pwd`|Muestra la ruta del directorio actual|`pwd`|
|`cd`|Cambia de directorio|`cd /home/usuario`|
|`ls`|Lista el contenido de un directorio|`ls`|
|`file`|Muestra el tipo de un archivo|`file archivo.txt`|
|`less`|Muestra el contenido de archivos página por página|`less archivo.txt`|
|`lsb_release`|Muestra información sobre la distribución de Linux|`lsb_release -a`|

---

## 🔍 **Detalle por Comando y sus Opciones**

---

### 📍 `pwd` — _Print Working Directory_

Muestra la **ruta completa** del directorio actual.

- Sin opciones relevantes
- Útil para confirmar dónde estás ubicado

```bash

pwd # /home/juan/Documentos
```


---

### 📂 `cd` — _Change Directory_

Cambia el directorio actual.

#### 🔧 Opciones de uso común:

|Uso|Descripción|Ejemplo|
|---|---|---|
|`cd /ruta/absoluta`|Ir a una ruta absoluta desde raíz `/`|`cd /usr/bin`|
|`cd nombre_dir`|Ir a una subcarpeta desde el directorio actual|`cd proyectos`|
|`cd ..`|Ir al directorio padre|`cd ..`|
|`cd ~`|Ir al directorio personal del usuario|`cd ~`|
|`cd`|Ir al directorio personal (igual que `cd ~`)|`cd`|
|`cd -`|Volver al último directorio visitado|`cd -`|

---

### 📄 `ls` — _List_

Lista archivos y carpetas del directorio actual.

#### 🔧 Opciones útiles:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-l`|Muestra detalles (long format)|`ls -l`|
|`-a`|Muestra archivos ocultos (empiezan por `.`)|`ls -a`|
|`-h`|Tamaños legibles (útil junto con `-l`)|`ls -lh`|
|`-R`|Lista recursivamente subdirectorios|`ls -R`|
|`-t`|Ordena por fecha de modificación|`ls -lt`|
|`--color`|Añade color para distinguir tipos de archivos|`ls --color=auto`|

💡 Combinaciones:

```bash

ls -lha
```

Muestra todos los archivos, incluidos ocultos, con detalles y tamaño legible.

---

### 📑 `file` — _Identifica el tipo de archivo_

Inspecciona un archivo y reporta su tipo (texto, imagen, binario, etc.)

bash

CopyEdit

`file documento.txt # documento.txt: ASCII text`

No tiene opciones frecuentes en el uso básico.

---

### 📚 `less` — _Visualizador de texto por páginas_

Permite ver archivos largos página por página.

```bash
less archivo.log
```

📝 Atajos dentro de `less`:

|Tecla|Acción|
|---|---|
|`Espacio`|Página siguiente|
|`b`|Página anterior|
|`/texto`|Buscar texto|
|`q`|Salir|

---

### 🛠️ `lsb_release` — _Información del sistema_

Muestra detalles sobre la distribución de Linux.

#### 🔧 Opciones:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-a`|Muestra toda la información disponible|`lsb_release -a`|
|`-d`|Solo muestra la descripción|`lsb_release -d`|

---

### 📌 Conceptos Clave

|Símbolo|Significado|
|---|---|
|`/`|Directorio raíz|
|`.`|Directorio actual|
|`..`|Directorio padre|
|`~`|Directorio personal del usuario|
|`-`|Último directorio visitado (usado con `cd`)|

---
