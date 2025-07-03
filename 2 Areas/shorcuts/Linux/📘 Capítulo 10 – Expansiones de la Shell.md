
---
### 🎯 **Objetivo del capítulo**

Aprender a usar las **expansiones que realiza Bash automáticamente** antes de ejecutar comandos, lo que te permite escribir comandos más potentes y flexibles con menos esfuerzo.

Incluye:

- Expansión de nombres de archivos (wildcards)
    
- Expansión de llaves `{}`
    
- Expansión de variables `$`
    
- Expansión aritmética
    
- Expansión de comandos con `` ` ` `` o `$(...)`
    

---

## 🔁 **Tipos de Expansión y su Uso**

---

### 🌟 1. **Expansión de nombres de archivos (Wildcards)**

Permite seleccionar archivos mediante patrones. Se usa mucho con `ls`, `cp`, `rm`, etc.

|Símbolo|Significado|Ejemplo|Explicación|
|---|---|---|---|
|`*`|Cero o más caracteres|`ls *.txt`|Todos los `.txt`|
|`?`|Un solo carácter|`ls archivo?.txt`|Coincide con `archivo1.txt`, `archivoA.txt`, etc.|
|`[...]`|Un solo carácter de un conjunto|`ls foto[1-3].jpg`|`foto1.jpg`, `foto2.jpg`, `foto3.jpg`|
|`[^...]`|Un carácter que **no** esté en el conjunto|`ls archivo[^0-9].txt`|Todos excepto los que terminan en número|

💡 Bash expande los patrones **antes** de ejecutar el comando.

---

### 🧃 2. **Expansión de llaves `{}`**

Permite generar múltiples cadenas (usado para crear o procesar varios archivos a la vez).

|Sintaxis|Resultado|
|---|---|
|`file{1,2,3}.txt`|`file1.txt`, `file2.txt`, `file3.txt`|
|`{a,b,c}`|`a`, `b`, `c`|
|`{01..05}`|`01`, `02`, `03`, `04`, `05`|
|`{A..Z}`|`A`, `B`, ..., `Z`|
|`mkdir project{1..3}`|Crea `project1`, `project2`, `project3`|

---

### 💲 3. **Expansión de variables**

Muestra el valor de una variable. Puedes usar variables del entorno o definir propias.

```bash
echo $HOME 
# /home/juan  
name="Carlos" 
echo "Hola, $name" 
# Hola, Carlos
```

#### Buenas prácticas:

- Usa comillas dobles `"$VAR"` para preservar espacios.
    
- Puedes usar llaves para claridad: `${USER}`, `${PATH}`
    

---

### ➗ 4. **Expansión aritmética**

Realiza cálculos directamente en la terminal.
```bash
echo $((3 + 5)) # 8
```
#### Soporta operaciones:

|Operación|Símbolo|Ejemplo|
|---|---|---|
|Suma|`+`|`echo $((5 + 3))`|
|Resta|`-`|`echo $((10 - 4))`|
|Multip.|`*`|`echo $((2 * 3))`|
|División|`/`|`echo $((8 / 2))`|
|Módulo|`%`|`echo $((10 % 3))`|

---

### 🔁 5. **Expansión de comandos** (`command substitution`)

Ejecuta un comando y **usa su salida como parte del comando mayor**.

```bash
echo "Hoy es: $(date)"
```

🔁 Forma alternativa:

```bash 
echo "Hoy es: `date`"
```

💡 `$(...)` es más moderno, legible y permite anidamiento.

---

## 🔎 Ejemplos prácticos

|Comando|Resultado / Uso|
|---|---|
|`cp archivo{1,2,3}.txt backup/`|Copia 3 archivos a una carpeta|
|`echo $((1024 * 2))`|Muestra 2048|
|`echo "Archivos: $(ls|wc -l)"`|
|`ls *.jpg`|Lista todos los archivos `.jpg`|
|`mkdir prueba{A..D}`|Crea 4 carpetas: `pruebaA` a `pruebaD`|

---
### 🧠 Consideraciones finales

- **Las expansiones ocurren antes de ejecutar el comando**, por eso pueden usarse como parte del mismo.
- Puedes combinar varias en una misma línea.
- Estas herramientas hacen que Bash sea poderosa para tareas repetitivas, automatización y scripting.

---
