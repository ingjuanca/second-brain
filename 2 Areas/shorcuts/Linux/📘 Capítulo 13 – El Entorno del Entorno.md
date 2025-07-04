
---
### 🎯 **Objetivo del capítulo**

Comprender cómo funciona el **entorno del sistema en Bash**, incluyendo:

- Variables de entorno
    
- Cómo se configuran y heredan
    
- Archivos de configuración
    
- Cómo afectan al comportamiento del shell y de los comandos
    

---

## 🌐 ¿Qué es el entorno?

Es un conjunto de **variables del sistema** que afectan el funcionamiento de comandos, scripts y programas.  
Cada vez que abres un shell, se crea un entorno basado en:

1. Archivos de configuración globales y personales
    
2. Variables heredadas del sistema
    
3. Variables que defines tú mismo
    

---

## 🔧 Comandos clave

|Comando|Descripción|Ejemplo|
|---|---|---|
|`printenv`|Muestra variables del entorno|`printenv`|
|`env`|Ejecuta un comando con variables temporales|`env VAR=valor comando`|
|`set`|Muestra todas las variables (shell + entorno)|`set`|
|`export`|Convierte una variable local en variable global|`export VAR=valor`|
|`unset`|Elimina una variable del entorno|`unset VAR`|

---

## 🧱 Variables comunes del entorno

|Variable|Descripción|Ejemplo|
|---|---|---|
|`PATH`|Rutas donde Bash busca ejecutables|`/usr/bin:/bin:/usr/local/bin`|
|`HOME`|Directorio personal del usuario|`/home/juan`|
|`USER`|Nombre del usuario|`juan`|
|`SHELL`|Ruta del intérprete actual|`/bin/bash`|
|`PWD`|Directorio actual|`/home/juan/documentos`|
|`EDITOR`|Editor de texto predeterminado|`nano`|
|`LANG`|Idioma y codificación del sistema|`es_ES.UTF-8`|

---

## 🧪 Cómo definir variables

### 1. **Variable local (temporal)**

```bash
SALUDO="Hola Mundo" 
echo $SALUDO
```

Solo existe en el shell actual.

---

### 2. **Variable de entorno (exportada)**

```bash
export EDITOR=vim
```


Ahora `EDITOR` está disponible para **subprocesos** (como scripts o programas).

---

### 3. **Variable temporal para un solo comando**

```bash
LANG=en_US.UTF-8 date
```

Solo se aplica **a ese comando**.

---

## ⚙️ Archivos de configuración del entorno

| Archivo           | Propósito                                         |
| ----------------- | ------------------------------------------------- |
| `/etc/profile`    | Configuración global del entorno (para todos)     |
| `~/.bash_profile` | Configuración personal (solo login shells)        |
| `~/.bashrc`       | Configuración personal (para shells interactivos) |
| `~/.profile`      | Alternativa a `.bash_profile`                     |

💡 Cuando abres una terminal interactiva (no de login), se ejecuta `~/.bashrc`.  
💡 Para login (como al conectarse por SSH), se usa `~/.bash_profile`.

---

## 📁 Modificar el PATH

Supón que tienes un programa en `~/mis_scripts`. Puedes agregarlo a tu PATH así:

```bash
export PATH=$PATH:~/mis_scripts
```

Para hacerlo permanente, agrégalo al final de `~/.bashrc` o `~/.profile`:

```bash
echo 'export PATH=$PATH:~/mis_scripts' >> ~/.bashrc
```

---

## 🧠 Consideraciones

- El entorno es **heredado por procesos hijos**.
    
- Los scripts y programas leen variables como `PATH`, `HOME`, `LANG`, etc.
    
- Puedes tener **variables privadas** (sin `export`) o **globales** (`export`).
    
- Las modificaciones en `~/.bashrc` solo aplican al usuario actual.
    

---