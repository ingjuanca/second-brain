
---

## Resumen de las **anotaciones estereotipo** en Spring

En esta lección se hace un repaso de lo aprendido en la sección de **stereotype annotations**, la cual es muy importante dentro de Spring Core.

---

### Puntos clave

1. **@Component**
    
    - En lugar de usar configuración XML, se puede usar la anotación `@Component` para marcar las clases Java.
        
    - Spring creará automáticamente un objeto de esa clase y lo gestionará como un **bean**.
        
2. **context:component-scan**
    
    - En el archivo XML se configura con:
        
```xml
<context:component-scan base-package="com.bharath.spring.springcoreadvanced.stereotype.annotations"/>
```
        
    - Esto le indica al contenedor de Spring qué paquetes debe **escanear** para encontrar clases anotadas con `@Component`.
        
    - También incluye todos los **subpaquetes** del paquete base.
        
    - Spring creará los objetos y realizará la **inyección de dependencias** automáticamente.
        
3. **@Scope**
    
    - Permite especificar el **alcance** del bean:
        
        - `singleton` (único objeto en toda la aplicación),
            
        - `prototype` (nuevo objeto en cada solicitud),
            
        - `request`,
            
        - `session`, etc.
            
4. **@Value**
    
    - Sirve para inyectar valores en los atributos del bean.
        
    - Se puede usar con:
        
        - **tipos primitivos** (int, String, etc.),
            
        - **colecciones** (List, Set, Map).
            
5. **@Autowired**
    
    - Se usa para inyectar automáticamente **objetos de referencia** o dependencias entre beans.
        
    - Puede aplicarse en campos, setters o constructores.
        

---

### Resumen completo del documento

La sección de **anotaciones estereotipo** en Spring enseña que ya no es necesario depender totalmente de la configuración en XML.  
En su lugar:

- Se usan anotaciones como **@Component** para que Spring gestione automáticamente los beans.
    
- Con **component-scan** se le indica al contenedor qué paquetes escanear en busca de clases anotadas.
    
- Con **@Scope** se define el alcance del bean.
    
- Con **@Value** se inyectan valores primitivos o colecciones.
    
- Con **@Autowired** se inyectan objetos de referencia (dependencias).
    

En conclusión, las **stereotype annotations simplifican enormemente la configuración**, reducen el XML y permiten que Spring maneje automáticamente la creación e inyección de beans. ✅