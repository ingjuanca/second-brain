
---
### **Objetivo del capítulo**

Aprender a **redirigir la entrada y salida** de los comandos en la terminal, lo que permite:

- Guardar la salida de un comando en un archivo
- Usar archivos como entrada para comandos
- Encadenar comandos usando pipes (`|`)
- Controlar la salida de errores (`stderr`)

---

## 🔁 **Conceptos clave de redirección**

|Símbolo|Significado|Ejemplo|
|---|---|---|
|`>`|Redirecciona la salida y **sobrescribe** el archivo destino|`echo hola > saludo.txt`|
|`>>`|Redirecciona la salida y **añade** al final del archivo|`echo mundo >> saludo.txt`|
|`<`|Usa un archivo como entrada del comando|`cat < saludo.txt`|
|`|`|Pipe: conecta la salida de un comando con la entrada de otro|
|`2>`|Redirecciona solo errores (stderr)|`ls xyz 2> errores.txt`|
|`&>`|Redirecciona stdout y stderr juntos|`comando &> todo.txt`|
|`2>&1`|Une stderr a stdout (útil en scripts)|`comando > salida.txt 2>&1`|

---

## 🧰 **Ejemplos de uso por tipo de redirección**

---

### 📤 **Redireccionar salida estándar (`stdout`)**

#### Sobrescribir un archivo:

```bash
ls > lista.txt
```

#### Añadir al final sin borrar contenido:

```bash
date >> registro.log
```

---

### 📥 **Redireccionar entrada estándar (`stdin`)**

```bash
sort < nombres.txt
```

Esto ordena el contenido de `nombres.txt` como si lo escribieras a mano.

---

### 🔗 **Pipe (`|`) — Conectar comandos**

Encadena comandos: la **salida del primero** pasa como **entrada al segundo**.

#### Ejemplos:

```bash
ls | sort # Ordena alfabéticamente los archivos listados  
cat archivo.txt | wc -l # Cuenta las líneas de archivo.txt
```

---

### ❌ **Redirigir errores (`stderr`)**

```bash
ls archivo_inexistente 2> errores.txt
```

Solo el mensaje de error se guarda, no la salida estándar.

---

### 🔄 **Redirigir stdout + stderr al mismo archivo**

```bash
comando > salida.txt 2>&1
```

O (en sistemas más nuevos, como Bash 4+):

```bash
comando &> salida.txt
```

---

## 🧪 Combinaciones útiles

### Grabar salida + errores en un solo archivo:

```bash
find / -name archivo.txt > resultado.txt 2>&1
```

### Monitorear logs y filtrar:

```bash
tail -f syslog.log | grep error
```

### Guardar listado ordenado:

```bash
ls -l | sort -k 5 -n > listado_ordenado.txt
```

---

## 🧠 Notas adicionales

- Puedes **combinar múltiples redirecciones** en una sola línea.
- `>` crea el archivo si no existe, o lo **sobrescribe si ya existe**.
- El símbolo `2>` redirige los errores, porque `2` representa `stderr`.
   - `1>` representa la salida estándar (aunque suele omitirse: `>` ya es `1>`).
- `&1` hace referencia al descriptor de archivo 1 (stdout).


