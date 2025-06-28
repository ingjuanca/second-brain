
---
### 🎯 Objetivo

Familiarizarte con el uso de comandos simples en la terminal bash, verificando que funciona correctamente.

---

## 🧰 Comandos y Descripción

|Comando|Descripción|Ejemplo|
|---|---|---|
|`date`|Muestra la fecha y hora actuales|`date`|
|`cal`|Muestra el calendario del mes actual|`cal`|
|`clear`|Limpia la pantalla de la terminal|`clear`|
|`exit`|Cierra la terminal (termina la sesión)|`exit`|

---

### 🔍 Detalle por Comando

---

#### 📅 `date`

Muestra la fecha y hora actuales del sistema.

##### ➕ Opciones útiles:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`+%Y`|Año en 4 dígitos|`date +%Y` → `2025`|
|`+%m`|Mes en 2 dígitos|`date +%m` → `06`|
|`+%d`|Día del mes|`date +%d` → `26`|
|`+%H:%M:%S`|Hora, minutos y segundos|`date +%H:%M:%S` → `19:04:12`|
|`+%A`|Día de la semana|`date +%A` → `Thursday`|
|`+%B`|Nombre del mes|`date +%B` → `June`|
|`--help`|Muestra ayuda del comando|`date --help`|

💡 Puedes combinar formatos:

```bash

date +"Hoy es %A, %d de %B de %Y" # Salida: Hoy es Thursday, 26 de June de 2025
```
---

#### 🗓️ `cal`

Muestra un calendario en la terminal. Por defecto, muestra el mes actual.

##### ➕ Opciones útiles:

|Opción|Descripción|Ejemplo|
|---|---|---|
|`cal 2025`|Muestra todos los meses de 2025|`cal 2025`|
|`cal 6 2025`|Muestra junio de 2025|`cal 6 2025`|
|`-y`|Muestra todo el año actual|`cal -y`|
|`-3`|Muestra el mes actual + anterior y siguiente|`cal -3`|
|`--help`|Muestra ayuda del comando|`cal --help`|

---

#### 🧹 `clear`

Limpia el contenido de la terminal.

- No tiene opciones útiles.
- Atajo equivalente: `Ctrl + L`.

---

#### 🚪 `exit`

Finaliza la sesión actual en la terminal.

- También puedes cerrar la terminal directamente.
- No requiere opciones.

---
