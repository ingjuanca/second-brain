
---
### 🎯 **Objetivo del capítulo**

Aprender a **crear, copiar, mover, renombrar y eliminar archivos y directorios**, utilizando los comandos más comunes para el manejo diario de archivos en Linux.

---

## 🧰 **Comandos Introducidos**

|Comando|Descripción|Ejemplo|
|---|---|---|
|`cp`|Copia archivos o directorios|`cp archivo.txt copia.txt`|
|`mv`|Mueve o renombra archivos o directorios|`mv archivo.txt carpeta/`|
|`rm`|Elimina archivos o carpetas|`rm archivo.txt`|
|`mkdir`|Crea uno o varios directorios|`mkdir nueva_carpeta`|
|`rmdir`|Elimina directorios vacíos|`rmdir vacía`|
|`touch`|Crea archivos vacíos o actualiza la fecha de modificación|`touch nuevo.txt`|
|`file`|Muestra el tipo de archivo|`file imagen.png`|

> Nota: Algunos de estos ya se presentaron en capítulos anteriores, pero aquí se amplían con más ejemplos y combinaciones.

---

## 🔍 **Detalle por Comando y sus Opciones**

---

### 📄 `cp` — _Copiar archivos o carpetas_

```bash
cp origen destino
```

#### 🔧 Opciones útiles:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-r`|Copia recursivamente un directorio|`cp -r dir1 dir2/`|
|`-i`|Pide confirmación antes de sobrescribir|`cp -i a.txt b.txt`|
|`-u`|Solo copia si el archivo fuente es más reciente|`cp -u fuente.txt destino.txt`|
|`-v`|Modo verboso (muestra lo que está copiando)|`cp -v *.jpg backup/`|
|`-a`|Copia preservando atributos (archivos y carpetas)|`cp -a datos/ copia_datos/`|

---

### 🔀 `mv` — _Mover o renombrar archivos y carpetas_

```bash
mv origen destino
```

#### 🔧 Opciones útiles:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-i`|Pregunta antes de sobrescribir|`mv -i documento.txt backup/`|
|`-u`|Solo mueve si el archivo fuente es más reciente|`mv -u archivo.txt destino/`|
|`-v`|Muestra lo que está moviendo|`mv -v archivo.txt carpeta/`|

💡 **Renombrar archivo**:

```bash
mv viejo.txt nuevo.txt
```

---

### ❌ `rm` — _Eliminar archivos o carpetas_

Este comando **borra permanentemente**.

```bash
rm archivo.txt
```

#### 🔧 Opciones importantes:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-r`|Elimina recursivamente (carpetas y su contenido)|`rm -r carpeta/`|
|`-f`|Fuerza la eliminación, sin preguntar|`rm -f *.log`|
|`-i`|Confirma cada eliminación|`rm -i archivo.txt`|
|`-rf`|Elimina carpetas sin confirmar|`rm -rf vieja_carpeta/`|

---

### 🏗️ `mkdir` — _Crear directorios_

Ya lo vimos antes, pero aquí se destaca:

#### 🔧 Opciones:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-p`|Crea estructura completa de directorios anidados|`mkdir -p año/mes/día`|
|`-v`|Muestra mensajes al crear|`mkdir -v proyectos/2025`|

---

### 🧹 `rmdir` — _Eliminar directorios vacíos_

```bash
rmdir carpeta_vacía
```

- Solo funciona si el directorio no contiene archivos.

---

### 🕓 `touch` — _Crear o actualizar archivos_

```bash
touch archivo.txt
```

- Crea el archivo si no existe
- Si existe, actualiza la fecha de modificación

💡 Puedes crear varios a la vez:

```bash
touch archivo1 archivo2 archivo3
```

---

### 📑 `file` — _Identificar tipo de archivo_

Ya visto en el capítulo 4, pero es útil cuando se trabaja con múltiples archivos:

```bash
file imagen.png # imagen.png: PNG image data, 800 x 600, 8-bit/color RGB
```

---

## 📌 Consideraciones del capítulo

- **Linux no requiere extensiones** como `.txt`, `.jpg`, etc., para identificar archivos.
- Es mejor **no usar espacios** en los nombres de archivo (puedes usar `_` o comillas).
- Si usas un nombre con espacio, debes entrecomillarlo:

```bash
cp "mi archivo.txt" copia.txt
```

