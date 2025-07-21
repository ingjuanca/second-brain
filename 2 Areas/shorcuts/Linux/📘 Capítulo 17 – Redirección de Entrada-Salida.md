
---

### 🎯 **Objetivo del capítulo**

Aprender a **redireccionar** la entrada y salida de comandos para aprovechar al máximo la terminal: guardar salidas en archivos, encadenar comandos, y manipular flujos de datos.

---

## 🔄 1. **Redirección de salida estándar (`stdout`)**

### 🔹 `>`

Redirige la salida del comando a un archivo (lo **sobrescribe**).

```bash
ls > listado.txt
```

| Símbolo | Acción                        | Nota                     |
| ------- | ----------------------------- | ------------------------ |
| `>`     | Redirige salida (sobrescribe) | Crea o reemplaza archivo |

---

### 🔹 `>>`

Redirige la salida y la **añade** al final del archivo (sin borrar lo anterior).

```bash
date >> log.txt
```

| Símbolo | Acción                     |
| ------- | -------------------------- |
| `>>`    | Añade al archivo existente |

---

## 🔁 2. **Redirección de entrada estándar (`stdin`)**

### 🔹 `<`

Toma contenido de un archivo como entrada para un comando.

```bash
sort < datos.txt
```

|Símbolo|Acción|
|---|---|
|`<`|Usa un archivo como entrada|

---

## 🔀 3. **Redirección de errores (`stderr`)**

### 🔹 `2>`

Redirige los mensajes de error.

```bash
ls archivo_que_no_existe 2> errores.txt
```

| Símbolo | Significado       |
| ------- | ----------------- |
| `2>`    | Redirige `stderr` |

---

### 🔹 `&>`

Redirige **tanto stdout como stderr** al mismo archivo (Bash 4+).

```bash
comando &> salida.txt
```

---

## 🔗 4. **Pipes (`|`) – Encadenamiento de comandos**

Envía la salida de un comando como **entrada de otro**.

```bash
ls -l | less
```

| Símbolo | Acción |
| ------- | ------ |
| `       | `      |

---

### 🧪 Ejemplo completo

```bash
cat archivo.txt | grep "clave" | sort > resultado.txt
```

🔹 Muestra las líneas que contienen "clave"  
🔹 Las ordena  
🔹 Las guarda en `resultado.txt`

---

## 🛠️ Combinaciones útiles

| Comando                  | Acción                         |
| ------------------------ | ------------------------------ |
| `command > archivo`      | Guarda la salida               |
| `command >> archivo`     | Añade la salida                |
| `command 2> errores.txt` | Guarda errores                 |
| `command < entrada.txt`  | Usa archivo como entrada       |
| `command1                | command2`                      |
| `command &> salida.txt`  | Guarda salida y errores juntos |

---