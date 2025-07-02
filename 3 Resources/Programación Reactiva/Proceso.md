
---

![[Pasted image 20250702112654.png]]
### 🧠 ¿Qué es un Proceso en Computación?

Un **proceso** es una instancia de un programa en ejecución. Mientras que un programa es simplemente un conjunto de instrucciones almacenadas en disco, un proceso es lo que ocurre cuando esas instrucciones se cargan en memoria y se ejecutan. Cada proceso tiene su propio espacio de memoria aislado, que incluye:

- Código del programa
- Datos
- Recursos del sistema asignados por el sistema operativo (como memoria, sockets, etc.)

---

![[Pasted image 20250702113023.png]]
### 🧵 Procesos vs. Hilos (Threads)

- Un **proceso** es una unidad de recursos.
- Un **hilo** es una unidad de ejecución dentro de un proceso.
- Cada proceso tiene al menos un hilo, pero puede tener muchos.
- Los hilos dentro de un mismo proceso comparten la memoria asignada al proceso, lo que permite una ejecución más eficiente.

![[Pasted image 20250702113142.png]]

---

![[Pasted image 20250702113411.png]]
### ⚙️ Ejecución y Planificación

- El **sistema operativo** utiliza un **planificador (scheduler)** para asignar hilos a los núcleos del procesador.
- En sistemas con un solo procesador, los hilos se ejecutan de forma intercalada mediante **cambios de contexto (context switching)**.
- En sistemas con múltiples núcleos, los hilos pueden ejecutarse en paralelo.
- Cada cambio de contexto requiere guardar el estado del hilo actual para poder reanudarlo más tarde.

---

### 🧱 Memoria: Stack vs. Heap

- **Stack memory**: contiene variables locales, referencias a objetos y la información de llamadas a funciones. Cada hilo tiene su propia pila.
- **Heap memory**: almacena objetos creados dinámicamente (como `ArrayList`, `HashMap`, etc.).
- El tamaño de la pila se define al crear el hilo y no puede cambiarse después.

---
### ☕ Java y los Hilos

- En Java, un hilo es un **envoltorio (wrapper)** alrededor de un hilo del sistema operativo.
- La ejecución de métodos en Java (por ejemplo, `method1 → method2 → method3`) se gestiona mediante la pila del hilo.
- Java asigna por defecto 1 MB de pila por hilo, aunque esto puede variar según el sistema operativo (por ejemplo, 2 MB en macOS).

---
### 💸 Costos y Eficiencia

- Crear y destruir procesos o hilos es **costoso** en términos de recursos.
- En arquitecturas de microservicios, los hilos pueden quedar inactivos durante llamadas de red lentas, desperdiciando CPU.
- Aunque se pueden crear más hilos para aprovechar mejor la CPU, esto consume más memoria.
- Por eso, se buscan mecanismos más eficientes para manejar operaciones de red sin desperdiciar recursos del sistema.

---

### 🧩 Conclusión

Los procesos y hilos son fundamentales para la ejecución de programas modernos. Entender cómo interactúan con la CPU, la memoria y el sistema operativo permite optimizar el rendimiento de las aplicaciones, especialmente en entornos donde los recursos son limitados o costosos.

---