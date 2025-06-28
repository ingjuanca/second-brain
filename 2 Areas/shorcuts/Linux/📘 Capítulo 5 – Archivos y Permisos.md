
---

### 🎯 **Objetivo del capítulo**

Entender cómo funcionan los archivos y permisos en Linux, incluyendo:

- Cómo se estructuran los archivos y directorios
- Cómo ver, cambiar y entender los permisos
- Qué significan los permisos de lectura, escritura y ejecución
- Comandos para inspeccionar y modificar los permisos y propiedad

---

## 🧰 **Comandos Introducidos**

|Comando|Descripción|Ejemplo|
|---|---|---|
|`ls -l`|Lista archivos con detalles, incluidos permisos|`ls -l`|
|`chmod`|Cambia los permisos de archivos/directorios|`chmod 755 script.sh`|
|`umask`|Muestra o cambia los permisos por defecto|`umask`|
|`touch`|Crea archivos vacíos o actualiza la fecha de modificación|`touch archivo.txt`|
|`mkdir`|Crea un nuevo directorio|`mkdir nueva_carpeta`|
|`rmdir`|Elimina directorios vacíos|`rmdir vacía`|
|`rm`|Elimina archivos o carpetas|`rm archivo.txt`|
|`stat`|Muestra información detallada de un archivo|`stat archivo.txt`|
|`chmod`|Cambia los permisos del archivo (modo numérico o simbólico)|`chmod u+x script.sh`|

---

## 🔍 Detalle por Comando y sus Opciones

---

### 📁 `ls -l`

Muestra permisos, propietarios, tamaño, y fecha de modificación de archivos/directorios.

#### Ejemplo:

```bash
ls -l # -rw-r--r-- 1 juan juan 1234 Jun 26 17:30 archivo.txt
```


📌 Permisos desglosados:

|Sección|Significado|
|---|---|
|`-`|Tipo (archivo: `-`, directorio: `d`)|
|`rw-`|Permisos del **usuario** (read/write)|
|`r--`|Permisos del **grupo**|
|`r--`|Permisos para **otros**|

---

### 🧱 `chmod` — _Change Mode_

Permite cambiar los permisos de un archivo o directorio.

#### 🔧 Modo simbólico

|Símbolo|Significado|
|---|---|
|`u`|usuario (owner)|
|`g`|grupo|
|`o`|otros|
|`a`|todos|
|`+`|añade permisos|
|`-`|elimina permisos|
|`=`|establece permisos específicos|
|`r`|lectura|
|`w`|escritura|
|`x`|ejecución|

#### 🧪 Ejemplos:

```bash
chmod u+x script.sh     # añade permiso de ejecución al usuario 
chmod go-w archivo.txt  # quita permiso de escritura a grupo y otros 
chmod a=r archivo.txt   # todos solo lectura
```


#### 🔢 Modo numérico

|Número|Permiso|
|---|---|
|0|---|
|1|--x|
|2|-w-|
|3|-wx|
|4|r--|
|5|r-x|
|6|rw-|
|7|rwx|

💡 Ejemplo:

```bash
chmod 755 script.sh  # rwx r-x r-x 
chmod 644 archivo.txt  # rw- r-- r--
```

---

### 🧮 `umask`

Muestra o establece los permisos predeterminados al crear archivos.

```bash
umask        # Muestra la máscara actual (ej: 0022) 
umask 0002   # Establece una nueva máscara
```


📌 El permiso final = `777` (para carpetas) o `666` (para archivos) menos la `umask`.

---

### 📦 `touch`

Crea archivos vacíos o actualiza la marca de tiempo de uno existente.

```bash
touch archivo.txt
```

---

### 📂 `mkdir`

Crea un nuevo directorio.

#### Opciones útiles:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-p`|Crea directorios intermedios si no existen|`mkdir -p a/b/c`|

---

### 🗑️ `rmdir`

Elimina un directorio **vacío**.

```bash
rmdir vacía
```

📌 Si el directorio **contiene archivos**, se debe usar `rm -r`.

---

### ❌ `rm`

Elimina archivos o carpetas.

#### Opciones importantes:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-r`|Borra directorios recursivamente|`rm -r carpeta`|
|`-f`|Forzar eliminación (sin preguntar)|`rm -f archivo.txt`|
|`-i`|Confirma antes de borrar|`rm -i archivo.txt`|

⚠️ _Este comando no envía los archivos a la papelera. El borrado es permanente._

---

### 🔎 `stat`

Muestra **información detallada** sobre un archivo o directorio.
```bash
stat archivo.txt
```

Muestra:

- Tamaño
- Último acceso
- Permisos en formato numérico y simbólico
- Usuario y grupo propietario

---