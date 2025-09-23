
---

Las **dependencias de tipo primitivo** pueden inyectarse de dos maneras:

- **Constructor Injection (inyección por constructor)**, o
- **Setter Injection (inyección por métodos setter)**, y en ambos casos se utiliza la etiqueta **`<value>`**.

---

### Ejemplo

Supongamos que tenemos una clase **`Product`** con una dependencia **`ID`** de tipo **Integer**.

Para inyectar este valor:

1. **Setter Injection**:

    - Se usa el elemento `<property>` para el campo `ID`.
    - Dentro de `<property>`, se utiliza la etiqueta `<value>` para asignar el valor.

```xml
<property name="id">     
	<value>101</value> 
</property>
```

2. **Atributo `value` dentro de `<property>`**:

    - En lugar de usar un elemento `<value>` separado, se puede usar un **atributo `value`** en la misma etiqueta `<property>`:

```xml
<property name="id" value="101"/>
```

3. **Uso de P Schema (o P namespace)**:
 
    - Forma más compacta para inyectar valores primitivos con **menos líneas de XML**.
    - Se definen los valores directamente como **atributos en el elemento `<bean>`** usando el prefijo `p:`:

```xml
<bean id="product" class="com.example.Product" p:id="101"/>
```

---

**Resumen completo del documento:**

Para **inyectar dependencias de tipo primitivo** en Spring se utilizan **constructor injection** o **setter injection**, siempre apoyándose en la etiqueta **`<value>`**.  
Existen tres maneras de asignar el valor:

1. **Etiqueta `<value>` dentro de `<property>`** (forma tradicional).
    
2. **Atributo `value` dentro de `<property>`** (más compacto).
    
3. **P Schema o P namespace**, que permite definir los valores directamente en el elemento `<bean>` reduciendo el XML necesario.

Estas tres técnicas se explorarán en las siguientes lecciones para facilitar la configuración de valores primitivos en los beans de Spring.