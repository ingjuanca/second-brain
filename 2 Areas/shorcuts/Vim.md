
---

## 🧭 **Navegación**

- `h`, `j`, `k`, `l`: Mover cursor izquierda, abajo, arriba, derecha
- `w`, `W`: Mover al inicio de la siguiente palabra (palabra normal / palabra grande)
- `b`, `B`: Mover al inicio de la palabra anterior
- `e`, `E`: Mover al final de la palabra
- `0`: Ir al inicio de la línea
- `^`: Ir al primer carácter no blanco de la línea
- `$`: Ir al final de la línea
- `gg`: Ir al inicio del archivo
- `G`: Ir al final del archivo
- `nG`: Ir a la línea _n_
- `Ctrl + d`: Bajar medio pantalla
- `Ctrl + u`: Subir medio pantalla
- `Ctrl + f`: Página siguiente
- `Ctrl + b`: Página anterior
- `%`: Saltar entre paréntesis, llaves o corchetes coincidentes

---

## ✍️ **Modos**

- `i`: Insertar antes del cursor
- `I`: Insertar al inicio de la línea
- `a`: Insertar después del cursor
- `A`: Insertar al final de la línea
- `o`: Nueva línea debajo
- `O`: Nueva línea arriba
- `Esc`: Volver al modo normal
- `v`: Modo visual
- `V`: Modo visual línea
- `Ctrl + v`: Modo visual por bloque

---

## ✂️ **Edición**

- `x`: Eliminar carácter bajo el cursor
- `dd`: Eliminar línea
- `dw`: Eliminar palabra
- `d$`: Eliminar hasta el final de la línea
- `D`: Eliminar hasta el final de la línea (igual que `d$`)
- `yy`: Copiar línea
- `yw`: Copiar palabra
- `y$`: Copiar hasta el final de la línea
- `p`: Pegar después del cursor
- `P`: Pegar antes del cursor
- `u`: Deshacer
- `Ctrl + r`: Rehacer
- `r<char>`: Reemplazar carácter bajo el cursor
- `~`: Cambiar mayúscula/minúscula del carácter

---

## 🔍 **Búsqueda**

- `/texto`: Buscar hacia adelante
- `?texto`: Buscar hacia atrás
- `n`: Siguiente coincidencia
- `N`: Coincidencia anterior
- `*`: Buscar palabra bajo el cursor hacia adelante
- `#`: Buscar palabra bajo el cursor hacia atrás

---

## 🔄 **Sustitución**

- `:s/viejo/nuevo/`: Sustituir en la línea actual
- `:s/viejo/nuevo/g`: Sustituir todas las ocurrencias en la línea
- `:%s/viejo/nuevo/g`: Sustituir en todo el archivo
- `:%s/viejo/nuevo/gc`: Sustituir con confirmación

---

## 🧪 **Guardar y Salir**

- `:w`: Guardar
- `:q`: Salir
- `:wq` o `ZZ`: Guardar y salir
- `:q!`: Salir sin guardar
- `:x`: Guardar si hay cambios y salir

---

## 🧠 **Otros útiles**

- `.`: Repetir última acción
- `Ctrl + o`: Volver a posición anterior
- `Ctrl + i`: Ir a posición siguiente
- `:e nombre_archivo`: Abrir archivo
- `:bn`, `:bp`: Ir al siguiente/anterior buffer
- `:ls`: Listar buffers
- `:sp archivo`: Dividir horizontalmente y abrir archivo
- `:vsp archivo`: Dividir verticalmente
- `Ctrl + w + w`: Cambiar entre ventanas
- `Ctrl + w + q`: Cerrar ventana

---