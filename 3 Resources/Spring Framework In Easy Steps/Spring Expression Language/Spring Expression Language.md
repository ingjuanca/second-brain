
---

## Introducción al **Spring Expression Language (SpEL)**

El documento explica cómo usar el **Spring Expression Language (SpEL)**, una característica avanzada de Spring que permite **evaluar expresiones dinámicas dentro de las anotaciones** (principalmente `@Value`) para asignar valores a campos, incluso durante la inyección de dependencias.

---

### 🔹 ¿Qué es SpEL?

- SpEL permite **analizar y ejecutar expresiones** dentro del contenedor de Spring.
    
- Las expresiones pueden incluir:
    
    - Clases
        
    - Variables
        
    - Métodos (estáticos o de instancia)
        
    - Constructores
        
    - Objetos
        
    - Operadores y constantes
        
- Estas expresiones se **evalúan en tiempo de ejecución** y su resultado se inyecta en los campos anotados.
    

---

### 🔹 Sintaxis básica

Una expresión de SpEL siempre se escribe dentro de una anotación `@Value`, con la siguiente estructura:

`@Value("#{expresion}")`

- El **símbolo `#` (hash o pound)** le indica a Spring que debe **evaluar la expresión**.
    
- Todo lo que esté dentro de las **llaves `{}`** será procesado por el contenedor de Spring.
    

---

### 🔹 Ejemplo básico con tipos primitivos

#### Ejemplo 1: Suma de enteros

`@Value("#{66 + 44}") private int id;`

- El contenedor evalúa `66 + 44 = 110` y asigna ese valor al campo `id`.
    

```java
@Value("#{66 + 44}")
private int id = 15;
```
#### Ejemplo 2: Operador ternario

`@Value("#{5 > 6 ? 22 : 33}") private int number;`

- Como `5 > 6` es **falso**, se inyecta el valor **33** en el campo.
    

> SpEL soporta operadores aritméticos, relacionales y lógicos, así como el operador ternario `condición ? valor1 : valor2`.

```java
@Value("#{5>6?22:33}")
private boolean active;
```

---

### 🔹 Uso de **métodos estáticos**

SpEL también permite invocar **métodos estáticos** dentro de las expresiones.

#### Ejemplo:

```java
@Value("#{T(java.lang.Math).abs(-99)}") 
private int id;
```

**Explicación:**

- `T(...)` le indica a Spring que se trata de una **clase estática**.
    
- `Math.abs(-99)` ejecuta el método estático `abs` de la clase `java.lang.Math`.
    
- El resultado (`99`) es el valor inyectado en el campo.
    

**Sintaxis general:**

`T(<paquete.clase>).<método>(<parámetros>)`

---

### 🔹 Creación de objetos dentro de expresiones

También se pueden crear **nuevos objetos** con el operador `new` dentro de la expresión.

#### Ejemplo:

```java
@Value("#{new Integer(88)}") 
private Integer number;
```

- Spring creará un nuevo objeto `Integer` y asignará el valor `88`.
    

---

### 🔹 Acceso a variables y constantes estáticas

SpEL permite acceder a **constantes estáticas** de clases Java:

#### Ejemplo:

```java
@Value("#{T(java.lang.Integer).MIN_VALUE}") 
private int minValue;
```

- Aquí, `T(java.lang.Integer)` hace referencia a la clase `Integer`.
    
- `MIN_VALUE` es una **constante estática final** definida dentro de la clase `Integer`.
    
- Spring inyecta automáticamente el valor mínimo que puede almacenar un tipo `int`.
    

De la misma forma, se puede acceder a `MAX_VALUE`:

```java
@Value("#{T(java.lang.Integer).MAX_VALUE}")
```

---

### 🔹 Ejemplo completo

```java
@Component public class Instructor {          
@Value("#{66 + 44}")     
private int id; // Resultado: 110      

@Value("#{T(java.lang.Math).abs(-99)}")     
private int positiveId; // Resultado: 99      

@Value("#{new Integer(88)}")     
private Integer objectId; // Resultado: 88      

@Value("#{T(java.lang.Integer).MIN_VALUE}")     
private int minValue; // Resultado: -2147483648 }
```

---

### 🔹 Resumen general

| Característica           | Descripción                                              | Ejemplo                             |
| ------------------------ | -------------------------------------------------------- | ----------------------------------- |
| **Expresiones básicas**  | Se evalúan operaciones matemáticas, lógicas o ternarias. | `#{5 > 3 ? 1 : 0}`                  |
| **Métodos estáticos**    | Permite llamar funciones estáticas con `T(<clase>)`.     | `#{T(java.lang.Math).abs(-9)}`      |
| **Objetos nuevos**       | Permite crear instancias de clases.                      | `#{new Integer(100)}`               |
| **Constantes estáticas** | Accede a valores `final static` de clases Java.          | `#{T(java.lang.Integer).MAX_VALUE}` |

---

### 🔹 Conclusión

El **Spring Expression Language (SpEL)** es una herramienta poderosa que permite:

1. Inyectar valores calculados dinámicamente.
    
2. Llamar métodos estáticos y acceder a variables de clase.
    
3. Crear objetos dentro de expresiones.
    
4. Reducir el código XML y hacer las configuraciones más flexibles y expresivas.
    

En esta lección se aprendió:

- A usar `@Value("#{expresion}")` para cálculos.
    
- A invocar métodos estáticos con `T(<clase>).método()`.
    
- A crear objetos con `new`.
    
- A acceder a constantes como `MIN_VALUE` o `MAX_VALUE`.
    

En resumen, **SpEL amplía enormemente las capacidades de configuración en Spring**, permitiendo escribir expresiones dinámicas dentro de las anotaciones sin necesidad de código adicional ni configuración XML extensa. ✅