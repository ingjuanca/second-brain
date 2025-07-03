
---
### 🎯 **Objetivo del capítulo**

Aprender a **usar comillas y caracteres de escape** para controlar cómo Bash interpreta lo que escribes. Esto es clave cuando trabajas con:

- Archivos con espacios
    
- Caracteres especiales (`$`, `*`, `!`, etc.)
    
- Variables en cadenas de texto
    
- Comandos dentro de comandos
    

---
## 🧰 **Tipos de citas y escapes en Bash**

|Tipo|Símbolo|¿Qué protege?|
|---|---|---|
|Comillas simples|`'texto'`|Protege todo literalmente|
|Comillas dobles|`"texto"`|Protege parcialmente (permite expansión)|
|Escape de carácter|`\`|Protege solo el carácter que sigue|

---
### 🟥 1. **Comillas simples `'...'`**

Todo entre comillas simples se **interpreta literalmente**.  
No se expanden variables, comandos ni caracteres especiales.

```bash
echo 'El usuario es $USER' 
# Salida: El usuario es $USER
```

✅ Útiles cuando **no quieres** que Bash interprete nada.

---

### 🟨 2. **Comillas dobles `"..."`**

Permiten expansión de:

- Variables: `$USER`, `$HOME`
    
- Comandos: `$(...)`
    
- Aritmética: `$((...))`
    

```bash
echo "El usuario es $USER" 
# Salida: El usuario es juan
```

✅ Ideal para combinar texto con variables sin perder funcionalidad.

---
### 🟦 3. **Escape con barra invertida `\`**

Protege solo **el carácter inmediatamente siguiente**.  
Se usa para espacios, comillas, o caracteres especiales.

```bash
echo Hola\ Mundo 
# Salida: Hola Mundo  

echo \"Hola\" 
# Salida: "Hola"
```

💡 También útil dentro de comillas dobles:

```bash
echo "Este es un \"mensaje\"" 
# Salida: Este es un "mensaje"
```

---

## 🧪 Ejemplos combinados

### 📁 Trabajar con nombres con espacios:

```bash
touch "mi archivo.txt" 
ls mi\ archivo.txt
```

### 💲 Evitar expansión de variable:

```bash
echo '$HOME' # Salida: $HOME
```

### 🧾 Expandir variable dentro de texto:

```bash
echo "Tu directorio es: $HOME"
```

---

### 🚫 ¡Cuidado!

❌ Sin comillas ni escape:

```bash
echo Aquí está el archivo: archivo con espacios.txt 
# Error: Bash interpreta cada palabra como argumento diferente
```


✅ Con comillas:

```bash
echo "Aquí está el archivo: archivo con espacios.txt"
```

---
### 🧠 Consideraciones

- Usa **comillas dobles** la mayoría de las veces (más flexibles).
    
- Usa **comillas simples** si quieres total literalidad.
    
- Escapa (`\`) solo cuando sea necesario dentro de otras citas.
    

---