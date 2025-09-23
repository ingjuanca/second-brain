
---

### Uso de **value** como atributo

Cuando se inyectan **tipos primitivos**, ya vimos que se puede usar la etiqueta:

`<value>...</value>`

**dentro** de `<property>`.  
La **segunda variación** consiste en usar **`value` como atributo** del elemento `<property>`:

```xml
<property name="id" value="20"/> 
<property name="name" value="Bharath"/>
```

_En este caso, no se necesita una etiqueta `<value>` de cierre._  
Esto se denomina **“property value as attribute”** (valor como atributo).  
El método anterior se llama **“value as element”** (valor como elemento).

Al ejecutar la prueba (Run As → Java Application), la inyección de dependencias sigue funcionando de la misma manera.

---

### Uso de **p:schema** o **p:namespace**

Una tercera forma de inicializar o inyectar valores es usar el **p namespace** (también llamado **p schema**), un método muy sencillo y popular:

1. Declarar el **namespace p** en el elemento `<beans>` del archivo de configuración:

    `xmlns:p="http://www.springframework.org/schema/p"`

1. En el elemento `<bean>`, usar **atributos con prefijo `p:`** para las propiedades:

```xml
<bean id="emp" class="com.ejemplo.Employee" p:id="20" p:name="Bharath"/>
```


_Ya no es necesario definir `<property>` ni `<value>`._  
_El `<bean>` puede cerrarse en la misma línea._

Al ejecutar la prueba nuevamente, la inyección de dependencias funciona exactamente igual.

---

**Resumen completo del documento:**

Para inyectar **valores primitivos en Spring**, existen **tres métodos principales**:

1. **Value as element:** Usar la etiqueta `<value>` dentro de `<property>`.
    
    `<property name="id"><value>20</value></property>`
    
2. **Value as attribute:** Usar el atributo `value` directamente en `<property>`.
    
    `<property name="id" value="20"/>`
    
3. **p:schema (p namespace):** Definir los valores como atributos con prefijo `p:` dentro del `<bean>`.
    
    `<bean id="EMP" class="com.ejemplo.Employee" p:id="20" p:name="Bharath"/>`
    

Los tres métodos generan el mismo resultado: **Spring inyecta correctamente las dependencias**, pero el uso de **p namespace** es el más **compacto y conveniente**.