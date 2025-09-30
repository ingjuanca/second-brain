
---

## Uso de la anotación **@Scope** en Spring

Spring ofrece varios **alcances (scopes)** para los beans:

- `singleton` 
- `prototype`
- `request`
- `session`
- `globalsession`

Hasta ahora, se han usado principalmente **singleton** y **prototype** mediante configuración XML.  
Los scopes `request` y `session` se utilizan en aplicaciones web con Spring MVC, y `globalsession` en aplicaciones portlet.

En esta lección se explica cómo configurar **scopes mediante anotaciones** en lugar de XML.

---

### Comportamiento por defecto en Spring

- Si no se especifica un scope, **Spring usa `singleton` por defecto**.
    
- Esto significa que el contenedor **crea un solo objeto**, sin importar cuántas veces se solicite.
    

**Ejemplo:**

1. En la clase de prueba, obtener dos veces el bean `Instructor`.
    
```java
Instructor instructor1 = context.getBean("instructor"); 
Instructor instructor2 = context.getBean("instructor");
```
    
2. Imprimir sus `hashCode`:
    
```java
System.out.println(instructor1.hashCode()); System.out.println(instructor2.hashCode());
```
    
3. La salida muestra el **mismo hashCode**, confirmando que **ambos apuntan al mismo objeto**.
    
	`123456`
	`123456`

---

### Configuración con **@Scope**

- Para cambiar el alcance, se edita la clase `Instructor.java`.
    
- Junto a la anotación `@Component`, se agrega:
    
```java
@Component 
@Scope("prototype") 
public class Instructor { ... }
```
    
- Importar `@Scope` desde:
    
    `org.springframework.context.annotation.Scope`
    
- Ahora, al ejecutar la prueba:
    
    - Los `hashCode` de `instructor1` y `instructor2` serán **diferentes**,
    - Porque el contenedor Spring **crea un nuevo objeto cada vez** que se solicita.
        

---

### Puntos clave

- `@Scope` **siempre se usa junto a anotaciones estereotipo** como `@Component`.
    
- No puede usarse de forma independiente.
    
- Valores comunes:
    
    - `"singleton"` → un solo objeto compartido (por defecto).
    - `"prototype"` → un nuevo objeto en cada solicitud.
    - `"request"`, `"session"`, `"globalsession"` → aplican en contextos web.
        

---

**Resumen completo del documento:**

El documento explica cómo **usar la anotación `@Scope`** en Spring para definir el alcance de los beans.

1. **Por defecto**, Spring crea beans con scope `singleton` (un único objeto en toda la aplicación).
    
    - Esto se verifica porque dos solicitudes del mismo bean devuelven el mismo objeto (igual `hashCode`).
        
2. Con `@Scope("prototype")`, Spring crea **un nuevo objeto cada vez que se solicita** (hashCodes diferentes).
    
3. `@Scope` se declara junto con `@Component` (u otras anotaciones estereotipo) y se importa desde `org.springframework.context.annotation`.
    
4. Además de `singleton` y `prototype`, existen los scopes `request`, `session` y `globalsession`, usados en aplicaciones web o portlet.
    

En resumen, `@Scope` permite controlar fácilmente si un bean debe ser **único** en toda la aplicación o **recrearse cada vez que se solicite**, simplificando la configuración respecto al XML. ✅