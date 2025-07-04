
---
### 🎯 **Objetivo del capítulo**

Introducir el concepto de **expresiones regulares (regex)** para hacer búsquedas avanzadas de texto, especialmente con el comando `grep`.

Este capítulo cubre:

- Qué es una expresión regular
    
- Cómo se usa con `grep`
    
- Caracteres especiales y sus significados
    
- Ejemplos prácticos para búsquedas complejas
    

---

## 🔍 ¿Qué es una expresión regular?

Una **expresión regular** es un **patrón de texto** usado para buscar coincidencias dentro de archivos, líneas o cadenas.  
Permite encontrar desde palabras exactas hasta estructuras específicas (como fechas, emails, etc.).

---
## 🧰 Comando principal: `grep`

```bash
grep "patrón" archivo.txt
```

| Opción | Descripción                                        | Ejemplo                       |
| ------ | -------------------------------------------------- | ----------------------------- |
| `-i`   | Ignora mayúsculas/minúsculas                       | `grep -i "error" log.txt`     |
| `-v`   | Muestra líneas que **no** coinciden                | `grep -v "ok" salida.txt`     |
| `-r`   | Busca recursivamente en directorios                | `grep -r "config" /etc`       |
| `-n`   | Muestra número de línea                            | `grep -n "Juan" personas.txt` |
| `-l`   | Muestra solo los nombres de archivos que coinciden | `grep -l "clave" *`           |
| `-E`   | Usa expresiones regulares extendidas (ERE)         | `grep -E "abc                 |

---

## 🧩 **Elementos básicos de expresiones regulares**

| Símbolo  | Significado                                  | Ejemplo                                       |
| -------- | -------------------------------------------- | --------------------------------------------- |
| `.`      | Cualquier carácter (excepto nueva línea)     | `h.t` → `hat`, `hit`, `hot`                   |
| `*`      | Cero o más repeticiones                      | `lo*` → `l`, `lo`, `loo...`                   |
| `+`      | Una o más repeticiones (`-E` o `grep -E`)    | `lo+` → `lo`, `loo`, etc.                     |
| `?`      | Cero o una vez                               | `colou?r` → `color`, `colour`                 |
| `^`      | Inicio de línea                              | `^Hola` → solo líneas que empiezan con “Hola” |
| `$`      | Fin de línea                                 | `fin$` → líneas que terminan en “fin”         |
| `[abc]`  | Uno de los caracteres indicados              | `[ch]ico` → `cico`, `hico`                    |
| `[^abc]` | Cualquier carácter **excepto** los indicados | `[^0-9]` → no numéricos                       |
| `[a-z]`  | Rango de caracteres                          | `[a-zA-Z]` → letras                           |
| `        | `                                            | O lógico (`grep -E`)                          |
| `()`     | Agrupación (con `-E`)                        | `(abc)+` → abc, abcabc…                       |
| `\`      | Escapa caracteres especiales                 | `\.` busca el carácter `.`                    |

---
## 📄 **Ejemplos prácticos**

| Búsqueda                               | Comando                                 |
| -------------------------------------- | --------------------------------------- |
| Líneas que contienen "Error" o "ERROR" | `grep -i "error" archivo.log`           |
| Líneas que **no** contienen “ok”       | `grep -v "ok" archivo.txt`              |
| Líneas que **empiezan con** “#”        | `grep "^#" archivo.conf`                |
| Líneas que **terminan con** “.sh”      | `grep "\.sh$" scripts.txt`              |
| Palabras que **tienen 3 letras**       | `grep -E "\b[a-zA-Z]{3}\b" archivo.txt` |
| Buscar cualquier número                | `grep -E "[0-9]+" archivo.txt`          |
| Buscar línea vacía                     | `grep "^$" archivo.txt`                 |

---

## 🧠 Consideraciones

- `grep` usa por defecto **regex básicas (BRE)**  
    Para usar paréntesis, `+`, `?`, `|` directamente, necesitas `grep -E` (regex extendidas o ERE).
    
- Usa comillas **dobles o simples** para proteger el patrón del intérprete de Bash.
    

---