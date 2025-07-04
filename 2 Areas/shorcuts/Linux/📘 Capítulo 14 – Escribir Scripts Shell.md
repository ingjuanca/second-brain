
---
### 🎯 **Objetivo del capítulo**

Aprender a **escribir y ejecutar scripts Bash**, desde lo más básico hasta introducir estructuras como condicionales y bucles.

Al final de este capítulo podrás crear tus propios programas automatizados en la terminal.

---

## 🧰 ¿Qué es un script Shell?

Un **script** es un archivo de texto que contiene comandos Bash escritos en orden.  
En lugar de escribir cada comando uno por uno, puedes ejecutarlos todos con un solo archivo.

---

## 🛠️ 1. **Estructura básica de un script**

```bash
#!/bin/bash 
# Comentario explicativo 
echo "Hola mundo"
```



- La línea `#!/bin/bash` se llama **shebang** y le dice al sistema qué intérprete usar.
    
- Los scripts deben tener permisos de ejecución.
    

### 🔑 Crear y ejecutar:

```bash
nano hola.sh 
chmod +x hola.sh 
./hola.sh
```


---

## ✍️ 2. **Comentarios**

Usa `#` para agregar explicaciones o desactivar líneas.

```bash
# Esto es un comentario 
echo "Hola" # también válido aquí
```

---

## 🧪 3. **Variables en scripts**

```bash
nombre="Juan" 
echo "Hola, $nombre"
```


📌 No dejes espacios alrededor del signo `=`.

---

## 📥 4. **Leer entrada del usuario**

```bash
echo "¿Cómo te llamas?" 
read nombre 
echo "Hola, $nombre"
```

---

## 🎯 5. **Argumentos del script**

Se accede a ellos mediante `$1`, `$2`, etc.

```bash
#!/bin/bash 
echo "Archivo: $1"
```

Ejecutar:

```bash
./mi_script.sh documento.txt
```

| Variable   | Significado                     |
| ---------- | ------------------------------- |
| `$0`       | Nombre del script               |
| `$1`, `$2` | Primer, segundo argumento...    |
| `$#`       | Número total de argumentos      |
| `$@`       | Todos los argumentos como lista |

---

## 🔀 6. **Condicionales (`if`)**

```bash
if [ -f "$1" ]; then   
	echo "Archivo existe" 
else   
	echo "No existe" 
fi
```

### 🔧 Operadores comunes en `[ ]`

|Expresión|Significado|
|---|---|
|`-f archivo`|Es un archivo regular|
|`-d dir`|Es un directorio|
|`-e archivo`|Existe|
|`string1 = string2`|Comparación de cadenas|
|`n1 -eq n2`|Igualdad numérica|

---

## 🔁 7. **Bucles (`for`, `while`)**

### 🔄 `for`:

```bash
for archivo in *.txt; do   
	echo "Archivo: $archivo" 
done
```

### 🔄 `while`:

```bash
contador=1 
while [ $contador -le 5 ]; do   
	echo "Número $contador"   
	contador=$((contador + 1)) 
done
```

---

## 🧠 8. **Salida y código de retorno**

```bash
echo "Listo" exit 0
```

- Puedes usar `exit 1`, `exit 2`, etc., para indicar errores personalizados.
    

---

## 📌 Ejemplo completo

```bash
#!/bin/bash  
echo "Nombre del archivo: $1"  
if [ -f "$1" ]; then   
	echo "El archivo existe. Número de líneas:"   
	wc -l "$1" 
else   
	echo "El archivo no existe"   
	exit 1 
fi
```

---
## 🧠 Consideraciones

- **Siempre prueba tus scripts en archivos no críticos.**
    
- Usa `set -x` al inicio para ver el detalle de cada ejecución (modo debug).
    
- Usa `chmod +x script.sh` para permitir su ejecución.
    
- Puedes crear funciones dentro de un script para organizar mejor tu código.
    

---