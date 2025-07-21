
---

### 🎯 **Objetivo del capítulo**

Aprender a **empaquetar (agrupar)** y **comprimir (reducir tamaño)** archivos usando herramientas tradicionales de Linux como `tar`, `gzip`, `bzip2`, `xz`, y `zip`.

---

## 📦 1. **`tar` – Archivado (sin compresión)**

El comando `tar` permite empaquetar varios archivos/directorios en **un solo archivo** (llamado _tarball_), ideal para respaldos o envíos.

```bash
tar -cf archivo.tar archivo1 archivo2 
tar -xf archivo.tar
```

### 🧾 Opciones comunes de `tar`

|Opción|Significado|Ejemplo|
|---|---|---|
|`-c`|Crear un nuevo archivo tar|`tar -cf backup.tar file.txt`|
|`-x`|Extraer el contenido|`tar -xf backup.tar`|
|`-t`|Ver contenido del archivo tar|`tar -tf backup.tar`|
|`-f`|Indica el nombre del archivo tar|(siempre debe usarse con `-c`, `-x`, etc.)|
|`-v`|Verboso (muestra progreso)|`tar -cvf backup.tar file.txt`|

---

## 🗜️ 2. **Compresión con `gzip` y `gunzip`**

`gzip` comprime archivos, reemplazándolos por una versión `.gz`.  
`gunzip` descomprime.

```bash
gzip archivo.txt       # crea archivo.txt.gz 
gunzip archivo.txt.gz  # recupera archivo original
```

### Opciones útiles:

|Comando|Efecto|
|---|---|
|`gzip -k`|Mantiene el archivo original|
|`gzip -r`|Comprime directorios recursivamente|
|`gunzip archivo.gz`|Descomprime archivo `.gz`|

---

## 🗂️ 3. **`tar` + `gzip` (comprimido en un paso)**

```bash
tar -czf archivo.tar.gz carpeta/ 
tar -xzf archivo.tar.gz
```

|Opción|Significado|
|---|---|
|`-z`|Comprimir con `gzip` o descomprimir|
|`-c`|Crear archivo tar|
|`-x`|Extraer archivo tar.gz|
|`-f`|Especificar archivo|

---

## 🔐 4. **Compresión con `bzip2` y `xz`**

### bzip2

```bash
bzip2 archivo.txt      # → archivo.txt.bz2 
bunzip2 archivo.txt.bz2
```

### xz

```bash
xz archivo.txt         # → archivo.txt.xz 
unxz archivo.txt.xz
```

| Comando | Descripción                       |
| ------- | --------------------------------- |
| `bzip2` | Compresión más eficiente que gzip |
| `xz`    | Compresión aún más eficiente      |

---

## 🧵 5. **`tar` con `bzip2` o `xz`**

### Con `bzip2` (`.tar.bz2`):

```bash
tar -cjf archivo.tar.bz2 carpeta/ 
tar -xjf archivo.tar.bz2
```


| Opción | Significado                |
| ------ | -------------------------- |
| `-j`   | Usa `bzip2` para comprimir |

---

### Con `xz` (`.tar.xz`):

```bash
tar -cJf archivo.tar.xz carpeta/ 
tar -xJf archivo.tar.xz
```


| Opción | Significado             |
| ------ | ----------------------- |
| `-J`   | Usa `xz` para comprimir |

---

## 🧳 6. **Uso de `zip` y `unzip`**

`zip` crea archivos `.zip` compatibles con Windows.

```bash
zip archivo.zip archivo1 archivo2 
unzip archivo.zip
```


|Opción|Descripción|
|---|---|
|`-r`|Comprime recursivamente carpetas|
|`-e`|Encripta con contraseña|

---

### 📦 Ejemplo completo:

```bash
tar -cvJf backup.tar.xz /home/juan/documentos
```


🔹 Crea un archivo comprimido `.tar.xz`  
🔹 Incluye todo el contenido del directorio `/home/juan/documentos`

---