
---

## Comprobación de dependencias – Creación del Bean y Prueba

En esta lección se muestra cómo **forzar la inyección de dependencias** en Spring usando la anotación **`@Required`**.

---

### Caso de uso: **Prescripción médica**

Se crea un **bean `Prescription`** con:

- `id` (int) – identificador de la prescripción,
- `patientName` (String) – nombre del paciente,
- `medicines` (List<String>) – lista de medicamentos.

**Pasos iniciales en Eclipse:**

1. Crear la clase `Prescription` en el paquete:

    `com.bharath.springcore.dependencycheck`

```

```
2. Añadir los campos: `id`, `patientName`, `List<String> medicines`.
    
3. Generar getters, setters y método `toString()`.
    
4. Crear el archivo `config.xml` en la misma carpeta.
    
5. Copiar y adaptar una clase de prueba (`Test.java`) para:
    
    - Cargar el `ApplicationContext`,
        
    - Obtener el bean `Prescription`,
        
    - Imprimir su contenido.
        

En esta primera parte solo se crea la estructura, sin configurar aún las dependencias.

---

## Comprobación de dependencias – En acción

Ahora se configura la **validación de dependencias obligatorias**:

1. En `config.xml`, declarar el bean:
    
    `<bean id="prescription"       class="com.bharath.springcore.dependencycheck.Prescription"/>`
    
    (sin inyectar valores inicialmente).
    
2. En la clase `Prescription`:
    
    - Usar la anotación `@Required` **en el método setter** del campo `id`.
        
    - **Importante:** La anotación **no se aplica directamente al campo**, solo al **setter**.
        
3. Ejecutar la prueba:
    
    - Spring crea el bean,
        
    - Pero **no lanza excepción**, incluso si `id` vale `0` y los demás campos son `null`.
        

---

### Habilitar el soporte para `@Required`

Por defecto **Spring tiene desactivado el soporte de anotaciones**.  
Para activarlo:

- En `config.xml` agregar el bean:
    
    `<bean class="org.springframework.beans.factory.annotation.RequiredAnnotationBeanPostProcessor"/>`
    
- Esto permite a Spring procesar la anotación `@Required`.
    

---

### Probar de nuevo

- Ejecutar la prueba otra vez:
    
    - Ahora Spring lanza:
        
        `BeanInitializationException: Property 'id' is required for bean 'prescription'`
        
    - Significa que el campo `id` es obligatorio y no se ha asignado.
        

---

### Solucionar la excepción

- En `config.xml` asignar un valor:
    
    `<bean id="prescription"       class="com.bharath.springcore.dependencycheck.Prescription"       p:id="123"/>`
    
- Re-ejecutar la prueba:
    
    - Spring ya no lanza excepción porque `id` tiene valor.
        

De forma similar, se pueden marcar como obligatorios los campos `patientName` y `medicines` aplicando `@Required` a sus respectivos **métodos setter**.

---

**Resumen completo del documento:**

La lección enseña a **exigir que ciertas propiedades de un bean Spring sean obligatorias** mediante la anotación `@Required`:

1. **Crear el bean `Prescription`** con campos `id`, `patientName` y `medicines`.
    
2. **Aplicar `@Required` en el setter** de la propiedad que se quiere obligatoria (por ejemplo, `setId`).
    
3. **Habilitar el soporte para `@Required`** en `config.xml` añadiendo el bean:
    
    `<bean class="org.springframework.beans.factory.annotation.RequiredAnnotationBeanPostProcessor"/>`
    
4. Si al crear el bean la propiedad obligatoria no se ha configurado, Spring lanza una **BeanInitializationException**.
    
5. Para resolver, se inyecta el valor requerido en el XML (por ejemplo, `p:id="123"`).
    

Así se asegura que Spring **valide automáticamente** que las propiedades marcadas con `@Required` reciban un valor antes de que el bean se utilice.