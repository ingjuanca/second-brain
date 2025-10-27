
---

## 🧩 **Título: Introducción a la Programación Orientada a Aspectos (AOP)**

---

### 🔹 **1. ¿Qué es AOP?**

**AOP (Aspect-Oriented Programming)** significa _Programación Orientada a Aspectos_.  
Se trata de un enfoque que permite **agregar servicios transversales (cross-cutting concerns)** a las clases de una aplicación **sin modificar su código fuente**.

Estos servicios pueden ser:

- Manejo de transacciones
    
- Registro de logs
    
- Seguridad
    
- Envío de correos, etc.
    

📘 **Ejemplo:**  
Supón una clase de negocio `OrderServiceImpl` con métodos `placeOrder()` y `shipOrder()`.  
Podríamos querer aplicar:

- Transacciones al realizar un pedido.
    
- Envío de email después del envío.
    
- Logging de cada operación.
    

Con AOP, todo esto se puede aplicar **externamente** sin modificar esos métodos directamente.

---

## 🧱 **2. Términos fundamentales en AOP**

Estos son los conceptos clave:

|Término|Descripción|
|---|---|
|**Aspect**|Es una **clase** que representa un servicio transversal (como logging, transacciones, seguridad, etc.).|
|**Advice**|Es un **método dentro del Aspect**, que contiene la lógica que se ejecutará (por ejemplo, escribir un log o iniciar una transacción).|
|**Pointcut**|Es una **expresión** que indica **en qué métodos** se aplicará el _advice_. Determina los puntos donde el aspecto debe actuar.|
|**Join Point**|Es la **combinación de un Pointcut y un Advice**. Define _qué método_ recibirá _qué advice_.|
|**Target**|Es el **objeto real del negocio** (por ejemplo, `OrderServiceImpl`) al cual se aplican los _advice_.|
|**Weaving (entrelazado)**|Es el **proceso de aplicar los advice** a los métodos del target. Spring crea una clase combinada que mezcla la lógica de negocio y la lógica del aspecto.|
|**Proxy**|Es el **objeto generado después del weaving**, que contiene tanto la lógica original como los aspectos aplicados.|

📘 **En resumen:**

- El **aspecto** define _qué hacer_ (por ejemplo, loggear).
    
- El **pointcut** define _dónde hacerlo_.
    
- El **weaving** es el proceso de _combinar ambos_.
    
- El **proxy** es el resultado final que ejecuta ambos comportamientos.
    

---

## 🧩 **3. Sintaxis de Pointcut (expresiones)**

Las **expresiones Pointcut** se usan para identificar métodos donde se aplicarán los aspectos.

La estructura general es:

```
access-modifier return-type package.class.method(parameter-types)
```

📘 **Ejemplo:**

```
execution(public int com.bharath.MyClass.multiply(int, int))
```

Este pointcut se aplicará solo a:

- Un método llamado `multiply`
    
- Dentro de `MyClass` en el paquete `com.bharath`
    
- Con tipo de retorno `int`
    
- Dos parámetros tipo `int`
    

---

### 🔸 **Uso de comodines (`*` y `..`)**

| Símbolo | Significado                                                                  |
| ------- | ---------------------------------------------------------------------------- |
| `*`     | Cualquier cosa (comodín) – se puede usar para método, clase, paquete o tipo. |
| `..`    | “Cualquier cantidad de subpaquetes” o “cualquier número de parámetros”.      |

📗 **Ejemplos prácticos:**

| Expresión                       | Significado                                                                                |
| ------------------------------- | ------------------------------------------------------------------------------------------ |
| `execution(public * get*())`    | Cualquier método público que empiece con `get` y no reciba parámetros.                     |
| `execution(* com.app..*(..))`   | Cualquier método, de cualquier clase, dentro del paquete `com.app` o sus subpaquetes.      |
| `execution(public void *Id())`  | Métodos públicos `void` cuyo nombre termine en `Id`, sin parámetros.                       |
| `execution(public int *E*(..))` | Métodos públicos `int` que contengan “E” en el nombre, con cualquier número de parámetros. |

📌 **Resumiendo:**

- `*` = cualquier cosa (comodín).
    
- `..` = cualquier subpaquete o cualquier parámetro.
    
- Las expresiones definen _dónde se aplicará el advice_.
    

---

## 🧠 **4. Frameworks AOP más utilizados**

Existen varios frameworks que implementan AOP en Java:

|Framework|Descripción|
|---|---|
|**AspectJ**|Framework más completo y potente para AOP en Java.|
|**Spring AOP**|Implementación ligera de AOP dentro de Spring Framework. Internamente usa AspectJ.|
|**JBoss AOP**|Framework de AOP integrado con el ecosistema de JBoss.|

📘 **En Spring**, se pueden aplicar aspectos de tres formas:

1. **AspectJ annotation-driven (recomendada y más usada).**
    
2. **AspectJ XML-driven (configuración XML tradicional).**
    
3. **Classic Spring Proxy-based (obsoleta).**
    

👉 En la práctica moderna se usa la **versión con anotaciones de AspectJ**.

---

## ⚙️ **5. Anotaciones importantes de AspectJ**

|Anotación|Descripción|
|---|---|
|`@Aspect`|Marca una clase como un aspecto.|
|`@Before`|Ejecuta un advice **antes** de que se invoque el método objetivo.|
|`@After`|Ejecuta un advice **después** de que se complete el método (sin importar el resultado).|
|`@AfterReturning`|Ejecuta un advice **solo si el método devuelve un valor correctamente**.|
|`@AfterThrowing`|Ejecuta un advice **si el método lanza una excepción**.|
|`@Around`|Ejecuta código **antes y después** del método (puede controlar si el método se ejecuta o no).|

📘 Estas anotaciones se colocan **en el Aspect**, no en las clases de negocio.

---

## 🧩 **6. Caso práctico: Logging con AOP**

El documento concluye con un **ejemplo práctico** que se implementará paso a paso.

### 🔸 **Objetivo:**

Crear un aspecto que registre en consola cada vez que se ejecuta un método `multiply()` de una clase de servicio.

---

### 🔹 **Pasos del proyecto**

1. **Crear proyecto Maven AOP**
    
    - Agregar dependencias de Spring y AspectJ en el `pom.xml`.
        
2. **Crear clase de servicio:**
    
    `public interface MathService {     int multiply(int a, int b); }`
    
    `public class MathServiceImpl implements MathService {     public int multiply(int a, int b) {         return a * b;     } }`
    
3. **Crear aspecto (logging):**
    
    `@Aspect public class LoggingAspect {     @Before("execution(* com.app.MathServiceImpl.multiply(..))")     public void logBefore() {         System.out.println("Método multiply() invocado");     } }`
    
4. **Configurar Spring (`applicationContext.xml` o clase Java):**
    
    - Registrar los beans.
        
    - Habilitar AspectJ con:
        
        `<aop:aspectj-autoproxy/>`
        
5. **Ejecutar prueba:**
    
    `public class Test {     public static void main(String[] args) {         ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");         MathService service = context.getBean("mathService", MathService.class);         System.out.println(service.multiply(4, 5));     } }`
    

✅ Resultado en consola:

`Método multiply() invocado Resultado: 20`

---

## 🧾 **Resumen general**

|Concepto|Descripción|
|---|---|
|**AOP**|Permite aplicar servicios externos (logging, seguridad, transacciones) sin modificar el código original.|
|**Aspect**|Clase que contiene la lógica del servicio externo.|
|**Advice**|Método dentro del aspecto que define la acción a ejecutar.|
|**Pointcut**|Expresión que indica dónde aplicar el advice.|
|**Join Point**|Unión de pointcut + advice (define qué se aplica y dónde).|
|**Weaving**|Proceso de combinar la lógica del negocio con los aspectos.|
|**Proxy**|Objeto final con lógica combinada (negocio + aspecto).|
|**Spring AOP**|Implementación ligera de AOP basada en AspectJ.|
|**Anotaciones**|`@Aspect`, `@Before`, `@After`, `@Around`, `@AfterReturning`, `@AfterThrowing`.|

---

## ✅ **Conclusión final**

La **Programación Orientada a Aspectos (AOP)** permite:

- Modularizar las **funciones transversales** de una aplicación.
    
- **Reducir duplicación de código**.
    
- **Separar las preocupaciones** (cada clase se enfoca solo en su lógica principal).
    

En **Spring MVC**, esto se implementa con **AspectJ y anotaciones**, logrando que servicios como logs, seguridad o manejo de transacciones se apliquen automáticamente a múltiples métodos sin alterar su implementación.

> 💡 En resumen:  
> AOP es la forma más elegante de aplicar comportamientos comunes en toda la aplicación de forma **centralizada, reutilizable y no invasiva**.