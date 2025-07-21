
---

### 🎯 **Objetivo del capítulo**

Profundizar en técnicas más avanzadas de scripting en Bash:

- Validaciones más robustas
    
- Uso de estructuras complejas (`case`, `select`)
    
- Funciones personalizadas
    
- Buenas prácticas de organización
    

---

## 🔧 1. **Validación de argumentos**

Antes de usar los argumentos, es buena práctica verificar que existan:

```bash
if [ $# -ne 1 ]; then   
	echo "Uso: $0 archivo"   
	exit 1 
fi
```

|Comando|Significado|
|---|---|
|`$#`|Número de argumentos pasados|
|`$0`|Nombre del script|
|`$1`|Primer argumento (y así sucesivamente)|

---

## 🔄 2. **Estructura `case`**

Útil para múltiples condiciones:

```bash
echo "¿Deseas continuar? (s/n)" 
read respuesta  

case $respuesta in   
	s|S) echo "Continuando...";;   
	n|N) echo "Saliendo...";;   
	*) echo "Opción no válida";; 
esac
```

| Símbolo | Significado                       |
| ------- | --------------------------------- |
| `       | `                                 |
| `*`     | Coincide con cualquier otro valor |
| `;;`    | Termina una opción del `case`     |

---

## 🔘 3. **Menú interactivo con `select`**

```bash

select opcion in Leer Escribir Salir; do   
	case $opcion in     
		Leer) echo "Elegiste Leer";;     
		Escribir) echo "Elegiste Escribir";;     
		Salir) break;;     
		*) echo "Opción no válida";;   
	esac 
done
```


🔹 `select` muestra un menú numérico automáticamente.  
🔹 Ideal para scripts interactivos.

---

## 🧱 4. **Funciones**

Permiten reutilizar código y mejorar la organización:

```bash
saludo() {   
	echo "Hola, $1" 
}  

saludo "Juan"
```


|Característica|Descripción|
|---|---|
|`nombre()`|Define la función|
|`$1`, `$2`|Argumentos de la función|
|`return`|Retorna un valor numérico (opcional)|

---

### 💡 Función con validación y salida

```bash
es_par() {   
	if (( $1 % 2 == 0 )); then     
		return 0   
	else     
		return 1   
	fi 
}  

es_par 4 && echo "Es par" || echo "Es impar"
```


---

## 🧹 5. **Buenas prácticas**

| Práctica                  | Descripción                                 |
| ------------------------- | ------------------------------------------- |
| Comentarios claros        | Explica qué hace cada parte del script      |
| `set -e`                  | Detiene el script si ocurre un error        |
| `set -u`                  | Falla si se usan variables no definidas     |
| Nombrado descriptivo      | Usa nombres claros para scripts y variables |
| Modularizar con funciones | Mejora la legibilidad y mantenimiento       |
| Uso de códigos de retorno | Establece `exit 1` o `return 1` en errores  |

---

## 📁 Ejemplo completo: Script interactivo

```bash
#!/bin/bash 
set -e  

menu() {
	select opcion in Listar Fecha Salir; do     
	case $opcion in       
		Listar) ls -l ;;       
		Fecha) date ;;       
		Salir) break ;;       
		*) echo "Opción inválida";;     
	esac   
done 
}  

menu 
echo "Script finalizado"
```

---