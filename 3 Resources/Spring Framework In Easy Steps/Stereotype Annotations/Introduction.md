
---

## Introducción a **@Component** y **component-scan** en Spring

Hasta ahora se había trabajado con **configuración XML** para crear objetos e inyectar dependencias.  
En esta lección se explica cómo hacerlo mediante **anotaciones estereotipo** de Spring.

---

### Pasos principales

1. **Marcar clases con `@Component`**
    
    - La anotación `@Component` convierte a la clase en un **bean gestionado por Spring**.
        
    - Es equivalente a declarar un `<bean>` en el archivo XML.
        
2. **Configurar `context:component-scan` en el XML**
    
    - En el archivo de configuración de Spring se agrega:
        
        `<context:component-scan base-package="com.bharath"/>`
        
    - Con esto se indica al contenedor de Spring **qué paquetes debe escanear** en busca de clases anotadas con `@Component`.
        
    - El contenedor también explora **los subpaquetes** del paquete base.
        

---

### Ejemplo de funcionamiento del escaneo

Supongamos la siguiente estructura:

```bash
com  
├── one  
│   ├── three  
│   └── four  
└── two      
	└── five          
		└── six
```

- Si el base-package es `com.one`, Spring encontrará las clases en `com`, `one`, y sus subpaquetes `three` y `four`.
    
- Si se especifica `com.two.five`, Spring escaneará solo `five` (y encontrará únicamente las clases allí presentes).
    
- Si se especifica `com`, Spring escaneará **todo el árbol de paquetes** y registrará todas las clases anotadas con `@Component`.
    

---

### Convenciones de nombres

Cuando Spring registra un objeto con `@Component`:

- Usa el **nombre de la clase** convertido a **camelCase** para el id del bean.
    
    - Ejemplo: `ProductService` → `productService`.
        

Para recuperar el objeto en pruebas o en la aplicación, se debe usar este nombre camelCase.

---

### Restricciones

- `@Component` solo puede usarse en **clases propias** (que podemos modificar).
    
- No se puede aplicar a **clases integradas de Java** (como `String`) ni a clases de **terceros** ya compiladas.
    

---

### Resumen completo del documento

La anotación **@Component** permite registrar una clase como **bean de Spring** sin necesidad de declararla manualmente en XML.  
Para que el contenedor las detecte, se configura **`<context:component-scan>`** en el archivo de configuración, indicando el paquete base a escanear (incluyendo sus subpaquetes).

Puntos clave:

1. `@Component` reemplaza el uso de `<bean>` en XML.
    
2. `context:component-scan` define qué paquetes analizar.
    
3. Spring registra automáticamente las clases con `@Component` usando nombres en **camelCase**.
    
4. El escaneo funciona jerárquicamente según el paquete base configurado.
    
5. `@Component` solo se aplica a **clases de nuestra aplicación** (no a clases integradas ni de terceros).
    

En conclusión, esta técnica permite **simplificar la configuración** y eliminar gran parte del XML al dejar que Spring gestione automáticamente la creación de beans a través de anotaciones.