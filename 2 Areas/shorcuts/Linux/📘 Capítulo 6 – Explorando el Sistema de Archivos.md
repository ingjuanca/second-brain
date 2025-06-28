
---

### 🎯 **Objetivo del capítulo**

Aprender a **buscar archivos y entender cómo está estructurado el sistema de archivos** en Linux. Introduce comandos para:

- Localizar archivos por nombre o ubicación   
- Buscar en base al contenido o atributos
- Ver qué contienen los directorios del sistema

---

## 🧰 **Comandos Introducidos**

| Comando    | Descripción                                             | Ejemplo                    |
| ---------- | ------------------------------------------------------- | -------------------------- |
| `find`     | Búsqueda de archivos basada en múltiples criterios      | `find / -name archivo.txt` |
| `locate`   | Búsqueda rápida por nombre utilizando una base de datos | `locate config.json`       |
| `updatedb` | Actualiza la base de datos de `locate`                  | `sudo updatedb`            |
| `which`    | Muestra la ubicación de un ejecutable en el PATH        | `which python3`            |
| `type`     | Muestra cómo se interpretará un comando                 | `type ls`                  |

---

## 🔍 **Detalle por Comando y sus Opciones**

---

### 🔎 `find` — _Búsqueda completa y flexible_

Busca archivos o directorios de forma recursiva a partir de un directorio dado.

#### 📌 Sintaxis general:

```bash
find [ruta] [criterios] [acciones]
```

#### 🔧 Opciones comunes:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-name`|Busca por nombre exacto (sensible a mayúsculas)|`find /home -name archivo.txt`|
|`-iname`|Igual que `-name` pero insensible a mayúsculas|`find . -iname "archivo*.txt"`|
|`-type f`|Solo archivos|`find . -type f`|
|`-type d`|Solo directorios|`find . -type d`|
|`-size +1M`|Archivos de más de 1 MB|`find / -size +1M`|
|`-mtime -1`|Archivos modificados en el último día|`find . -mtime -1`|
|`-user juan`|Archivos pertenecientes al usuario "juan"|`find / -user juan`|
|`-exec comando {} \;`|Ejecuta una acción sobre los resultados|`find . -name "*.log" -exec rm {} \;`|

---

### 🧭 `locate` — _Búsqueda rápida por nombre_

Usa una base de datos indexada para encontrar rutas rápidamente.

```bash
locate archivo.txt
```

#### 🔧 Opciones:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`-i`|Búsqueda insensible a mayúsculas|`locate -i Foto.jpg`|
|`-c`|Muestra solo la cantidad de resultados|`locate -c archivo.txt`|
|`--regex`|Usa expresiones regulares para buscar|`locate --regex ".*\.log$"`|

📝 Nota: Los resultados dependen de cuándo se actualizó la base de datos con `updatedb`.

---

### 🔄 `updatedb` — _Actualiza la base de datos de locate_

Ejecuta este comando para tener una base de datos actualizada.

```bash
sudo updatedb
```

- Requiere permisos de administrador (`sudo`)
- Se recomienda ejecutarlo regularmente o usar `cron`

---

### 🧠 `which` — _Muestra la ruta del ejecutable de un comando_

Busca en el `$PATH` del usuario.

```bash
which python # /usr/bin/python
```

✅ Útil para confirmar qué versión se ejecutará si tienes varias instaladas.

---

### ℹ️ `type` — _Describe el tipo de un comando_

Indica si un comando es:

- Interno (built-in)
- Alias
- Función
- Archivo externo (ejecutable)

```bash
type cd # cd is a shell builtin  
type ls # ls is /bin/ls
```

---

## 🧭 Directorios del Sistema (resumen del capítulo)

|Ruta|Contenido típico|
|---|---|
|`/bin`|Comandos básicos del sistema|
|`/usr/bin`|Programas de usuario|
|`/sbin`|Comandos del sistema para administración|
|`/etc`|Archivos de configuración|
|`/home`|Directorios personales de los usuarios|
|`/root`|Directorio personal del usuario root|
|`/var`|Archivos variables (logs, colas, etc.)|
|`/tmp`|Archivos temporales|
|`/proc`, `/sys`|Información sobre procesos y hardware|

---