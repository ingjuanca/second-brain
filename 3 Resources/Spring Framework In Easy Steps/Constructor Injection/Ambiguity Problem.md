
---

## Problema de ambigüedad en la inyección por constructor

En esta lección se explica un **problema común** al usar **inyección de dependencias por constructor en Spring**, por qué ocurre y cómo resolverlo.

---

### 1️⃣ El problema de la ambigüedad

- Se crea una clase **`Addition`** con **varios constructores**:
    
```java
Addition(int a, int b)        // Constructor con enteros 
Addition(double a, double b)  // Constructor con dobles 
Addition(String a, String b)  // Constructor con cadenas
```
    
    Cada constructor imprime en consola cuál se está ejecutando.
    
- En el archivo `config.xml` se define:
    
```xml
<bean id="addition" class="...Addition">     
	<constructor-arg value="10"/>     
	<constructor-arg value="20"/> 
</bean>
```
    
- Al ejecutar la prueba:
    
    - **Spring invoca el constructor de enteros**, como se espera.
        
- Si se **cambia el orden de los constructores en el código**:
    
    - Spring puede llamar al **constructor de dobles** o incluso al **de cadenas**.
        

#### ¿Por qué ocurre?

- Por defecto, **Spring trata todos los valores como `String`** en el XML.
    
- Si encuentra un constructor que recibe `String`, **lo usará primero**.
    
- Si no hay constructor de `String`, buscará otros y tomará el **primero que coincida** (por ejemplo, el de `double`).
    
- De ahí la **ambigüedad**: Spring puede elegir un constructor distinto al esperado.
    

---

### 2️⃣ Solución con el atributo `type`

Para indicar explícitamente el tipo de cada argumento en el XML:

```xml
<constructor-arg value="10" type="int"/> 
<constructor-arg value="20" type="int"/>
```

- Spring ahora **forzará el uso del constructor con parámetros `int`**,  
    incluso si existen constructores de `String` o `double`.
    

---

### 3️⃣ Variación: Orden de los parámetros

- Supongamos un solo constructor:
    
```java
Addition(int a, double b)
```
    
- En `config.xml`:
    
```xml
<constructor-arg value="10"/> 
<constructor-arg value="20.3"/>
```
    
    Funciona correctamente.
    
- Pero si se **invierte el orden en el XML**:
    
```xml
<constructor-arg value="20.3"/> 
<constructor-arg value="10"/>
```
    
    Spring **aún inyecta correctamente** (20.3 al double, 10 al int),  
    lo cual podría ser **indeseado** si se quiere respetar el orden exacto.
    

---

### 4️⃣ Solución con el atributo `index`

Para garantizar el orden:

```xml
<constructor-arg value="10" index="0"/> 
<constructor-arg value="20.3" index="1"/>
```

- `index` empieza en **0** y sigue el orden de los parámetros del constructor.
    
- Si se coloca un índice incorrecto, Spring lanza una **excepción de inicialización**,  
    evitando inyecciones con orden equivocado.
    

---

### Otros atributos útiles

Además de `type` e `index`, Spring ofrece también el atributo `name`  
(para emparejar por nombre de parámetro), aunque en la lección se cubren  
principalmente **`type`** e **`index`**.

---

**Resumen completo del documento:**

Cuando una clase tiene **constructores sobrecargados**, la **inyección por constructor en Spring** puede presentar **ambigüedades**:

- Spring trata los valores definidos en XML como `String` y busca primero un constructor que acepte cadenas.
    
- Si no existe, intenta convertir los valores a otros tipos (`double`, `int`, etc.) y elige el primer constructor que coincida, lo que puede no ser el deseado.
    
- Además, Spring puede **inyectar los parámetros en cualquier orden** si no se especifica, siempre que los tipos sean compatibles.
    

**Soluciones:**

1. **Atributo `type`**: especifica el tipo exacto de cada argumento para que Spring seleccione el constructor correcto.
    
2. **Atributo `index`**: fuerza el orden de los argumentos según la posición de los parámetros en el constructor (empezando en 0).  
    Si el índice no coincide con el tipo de parámetro, Spring lanza una excepción.
    

Estos atributos permiten **eliminar la ambigüedad** y asegurar que Spring use el **constructor y el orden de parámetros correctos** en la inyección de dependencias.