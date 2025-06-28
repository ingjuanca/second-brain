
---
### 🎯 **Objetivo del capítulo**

Introducir comandos esenciales para **ver, combinar, contar, ordenar y manipular contenido de archivos de texto** desde la terminal. Este capítulo es clave para trabajar con logs, datos y archivos de configuración.

---

## 🧰 **Comandos Introducidos**

| Comando | Descripción                                           | Ejemplo                      |
| ------- | ----------------------------------------------------- | ---------------------------- |
| `cat`   | Muestra el contenido de un archivo                    | `cat archivo.txt`            |
| `less`  | Muestra contenido paginado (ya visto, pero reaparece) | `less archivo.log`           |
| `head`  | Muestra las primeras líneas de un archivo             | `head archivo.txt`           |
| `tail`  | Muestra las últimas líneas de un archivo              | `tail archivo.txt`           |
| `sort`  | Ordena líneas de un archivo                           | `sort archivo.txt`           |
| `uniq`  | Elimina duplicados consecutivos                       | `uniq archivo.txt`           |
| `wc`    | Cuenta líneas, palabras y caracteres                  | `wc archivo.txt`             |
| `cut`   | Extrae columnas de texto delimitado                   | `cut -d ":" -f1 /etc/passwd` |
| `paste` | Une líneas de varios archivos en columnas             | `paste file1 file2`          |

---

## 🔍 **Detalle por Comando y sus Opciones**

---

### 🐱 `cat` — _Concatenar y mostrar contenido_

```bash
cat archivo.txt
```

#### 🔧 Opciones útiles:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-n`|Muestra números de línea|`cat -n texto.txt`|
|`archivo1 archivo2`|Muestra ambos archivos juntos|`cat uno.txt dos.txt`|
|`>`|Redirecciona la salida (sobrescribe)|`cat > nuevo.txt` (espera entrada)|
|`>>`|Añade contenido al final de un archivo|`cat >> log.txt`|

---

### 📖 `less` — _Ver contenido con paginación_

Ya se cubrió, pero sigue siendo clave aquí.

---

### 🔼 `head` — _Primeras líneas del archivo_

```bash
head archivo.txt
```

#### 🔧 Opciones:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-n N`|Muestra las primeras N líneas|`head -n 5 archivo.txt`|

---

### 🔽 `tail` — _Últimas líneas del archivo_

```bash
tail archivo.txt
```

#### 🔧 Opciones:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-n N`|Muestra las últimas N líneas|`tail -n 10 archivo.txt`|
|`-f`|Muestra nuevas líneas a medida que se agregan|`tail -f archivo.log`|

💡 Muy útil para monitorear logs en tiempo real.

---

### 🧮 `wc` — _Word Count_

Cuenta líneas, palabras y caracteres.

```bash
wc archivo.txt
```


#### 🔧 Opciones:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-l`|Solo cuenta líneas|`wc -l archivo.txt`|
|`-w`|Solo cuenta palabras|`wc -w archivo.txt`|
|`-c`|Solo cuenta bytes (caracteres)|`wc -c archivo.txt`|

---

### 🔤 `sort` — _Ordenar líneas_

```bash
sort archivo.txt
```

#### 🔧 Opciones:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-r`|Orden inverso|`sort -r archivo.txt`|
|`-n`|Ordena numéricamente|`sort -n numeros.txt`|
|`-u`|Elimina duplicados|`sort -u archivo.txt`|
|`-k N`|Ordena por la columna N|`sort -k 2 archivo.txt`|

---

### 🔁 `uniq` — _Eliminar líneas duplicadas consecutivas_

```bash
uniq archivo.txt
```

💡 Requiere que los duplicados estén uno tras otro, por eso a menudo se usa con `sort`.

```bash
sort archivo.txt | uniq
```

#### 🔧 Opciones:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-c`|Muestra cuántas veces aparece cada línea|`uniq -c archivo.txt`|
|`-d`|Muestra solo duplicados|`uniq -d archivo.txt`|

---

### ✂️ `cut` — _Extraer columnas delimitadas_

```bash
cut -d ":" -f1 /etc/passwd
```

#### 🔧 Opciones:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-d`|Define el delimitador de columnas|`-d ","` para CSV|
|`-f N`|Selecciona columna(s)|`cut -f2`|
|`-c N-M`|Selecciona caracteres en rango|`cut -c1-5 archivo.txt`|

---

### 📋 `paste` — _Combinar líneas en columnas_

```bash
paste archivo1 archivo2
```

- Une línea 1 de archivo1 con línea 1 de archivo2, y así sucesivamente.

#### 🔧 Opción:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-d`|Define delimitador personalizado|`paste -d "," file1 file2`|

