
---

### Resumen: Dependency Check, Inner Beans y Scope

En esta lección se presenta un **resumen de tres conceptos clave** aprendidos:

---

#### 1. **Dependency Check (@Required)**

- Con la anotación **`@Required`** podemos **asegurar que una dependencia obligatoria reciba un valor** antes de que el bean se utilice.
    
- Si esa dependencia **no se proporciona en tiempo de ejecución**, Spring lanza una **BeanInitializationException**.
    
- Por defecto, si no se usa `@Required`, Spring puede asignar **valores por defecto**.  
    Pero si se marca la dependencia con `@Required`, **es obligatorio** configurarla, de lo contrario la aplicación fallará al iniciar.
    

---

#### 2. **Inner Beans (Beans internos)**

- Permiten **definir un bean dentro de otro bean** cuando existe una relación de dependencia directa.
    
- En la configuración XML:
    
```xml
<bean id="beanPrincipal" class="...">     
	<property name="dependencia">         
		<bean class="..."/>     
	</property> 
</bean>
```
    
- El **bean interno no necesita un nombre**, porque solo se usa dentro del bean que lo contiene.
    
- Es útil cuando la dependencia **no se reutilizará en otro lugar**.
    

---

#### 3. **Scopes (Alcances) de los Beans**

- Spring define **cinco alcances**:
    
    1. **Singleton (por defecto):**  
        _Spring crea **una sola instancia** del bean, sin importar cuántas veces se solicite._
        
    2. **Prototype:**  
        _Spring crea **una nueva instancia cada vez** que se solicita el bean._
        
    3. **Request:**  
        _Una instancia por **petición HTTP**, aplicable en aplicaciones web con Spring MVC._
        
    4. **Session:**  
        _Una instancia por **sesión de usuario**, en aplicaciones Spring MVC._
        
    5. **Global Session:**  
        _Una instancia global para **portlets** (muy poco usado)._
        
- Para cambiar el alcance por defecto, se especifica el atributo `scope` en la configuración XML:
    
```xml
<bean id="miBean" class="..." scope="prototype"/>
```
    

---

**Resumen completo del documento:**

El texto repasa tres temas fundamentales de Spring:

1. **Dependency Check:** con `@Required` garantizamos que un bean reciba todas sus dependencias críticas.  
    Si no se proporcionan, Spring lanza una excepción de inicialización.
    
2. **Inner Beans:** permiten declarar un bean directamente dentro de otro en el archivo XML, ideal cuando la dependencia no será reutilizada.
    
3. **Scopes de Beans:** Spring ofrece cinco tipos de alcance (singleton, prototype, request, session y global session).  
    _Por defecto es singleton_, pero se puede cambiar a prototype u otros especificando `scope` en el XML.  
    Los alcances request y session se usan en aplicaciones web Spring MVC, y global session en aplicaciones de portlets.
    

Este resumen concluye la sección, mostrando cómo **asegurar dependencias**, **definir beans internos** y **controlar el ciclo de vida y cantidad de instancias de los beans** dentro del contenedor Spring.