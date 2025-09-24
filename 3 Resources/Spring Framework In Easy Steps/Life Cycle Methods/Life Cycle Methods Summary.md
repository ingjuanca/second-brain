
---

### Resumen de los **métodos de ciclo de vida** en Spring

El **contenedor de Spring** ofrece **dos métodos de ciclo de vida** para cada objeto (bean) que crea:

- **init method**: se ejecuta **justo después de crear el objeto**, para inicializarlo.
    
- **destroy method**: se ejecuta **justo antes de que el objeto sea destruido**, para liberar recursos.
    

---

#### Uso de estos métodos

En el **método init** normalmente se coloca:

- Código de inicialización,
    
- Por ejemplo: abrir conexiones a bases de datos o servicios web.
    

En el **método destroy** se ubica:

- Código de limpieza,
    
- Por ejemplo: cerrar conexiones o eliminar archivos temporales.
    

---

### Tres formas de configurar los métodos de ciclo de vida

1. **Configuración XML**
    
    - En el elemento `<bean>` del archivo de configuración se usan los atributos:
        
        `init-method="nombreMetodoInit" destroy-method="nombreMetodoDestroy"`
        
    - Los nombres de los métodos en la clase pueden ser **cualesquiera**, no necesariamente “init” o “destroy”.
        
2. **Anotaciones**
    
    - Colocar en los métodos de la clase las anotaciones:
        
        `@PostConstruct @PreDestroy`
        
    - Los nombres de los métodos también pueden ser libres.
        
    
    > **Nota:** Por defecto, **el soporte de anotaciones en Spring está deshabilitado**.  
    > Para habilitarlo se debe declarar en el XML:
    > 
    > `<context:annotation-config/>`
    > 
    > (y configurar previamente el namespace `context`).
    
3. **Interfaces de Spring**
    
    - Implementar en la clase:
        
        - `InitializingBean` (con el método `afterPropertiesSet()` para la inicialización),
            
        - `DisposableBean` (con el método `destroy()` para la destrucción).
            

---

**Resumen completo del documento:**

Los **métodos de ciclo de vida** en Spring permiten ejecutar código especial:

- **después de crear un bean** (init),
    
- **antes de destruirlo** (destroy).
    

Hay **tres maneras** de configurarlos:

1. **A través de XML**, especificando en `<bean>` los atributos `init-method` y `destroy-method`.
    
2. **Mediante anotaciones**, usando `@PostConstruct` y `@PreDestroy` (previa activación con `<context:annotation-config/>`).
    
3. **Implementando interfaces**, concretamente `InitializingBean` y `DisposableBean`.
    

En todos los casos, los nombres de los métodos dentro de la clase pueden ser cualquier nombre válido en Java.  
Estas técnicas permiten controlar la **inicialización** y **limpieza de recursos** de los beans en el ciclo de vida gestionado por Spring.