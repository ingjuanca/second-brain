
---

## Autoconexión (Autowiring) **por nombre** en Spring

En esta lección se explica cómo realizar **autowiring byName**, es decir, que Spring **inyecte automáticamente una dependencia buscando el bean por su nombre**.

---

### Diferencia con _byType_

Hasta ahora se había visto `autowire="byType"`, que busca el bean por **tipo de clase**.  
Con `autowire="byName"` solo hay que **cambiar el valor del atributo**:

`autowire="byName"`

y Spring buscará el bean por **nombre** (id del bean en el XML) en vez de por tipo.

---

### Funcionamiento paso a paso

1. En la clase `Employee` existe una dependencia:
    
    `private Address address;`
    
2. Con `autowire="byName"`, el contenedor:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:c="http://www.springframework.org/schema/c"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">


	<bean class="com.bharath.spring.springcoreadvanced.autowiring.Address"
		name="address" p:hno="123" p:street="mira mesa" p:city="san diego" />
		
	<bean class="com.bharath.spring.springcoreadvanced.autowiring.Employee"
		name="employee" autowire="byName"/>

</beans>
```
	
    - Ve que `Employee` necesita un objeto `Address`.
        
    - **Toma el nombre de la variable de referencia** (`address`).
        
    - Busca en el archivo `config.xml` un bean cuyo **id sea exactamente `address`**.
        
    - Si lo encuentra, **inyecta ese bean** en `Employee`.
        
    - En este modo, **Spring no verifica el tipo de la clase**, solo el **nombre del bean**.
        

---

### Prueba de funcionamiento

- Al ejecutar la prueba, Spring:
    
    - Crea el bean `Employee`,
        
    - Encuentra el bean con id `address`,
        
    - Lo instancia y lo inyecta,
        
    - Todo funciona como se espera.
        

---

### Comportamiento si falta el bean

- Si se elimina el bean `address` del XML y se ejecuta de nuevo:
    
    - La propiedad `address` en `Employee` será **null**,
        
    - No se lanza excepción, simplemente no se inyecta nada.
        

---

### Duplicados de nombre

- Si se **duplica** el bean con el mismo nombre `address`:
    
    - Spring lanza:
        
        `Configuration problem: Bean name 'address' is already used...`
        
        (Duplicate bean definition parsing exception).
        
    - **No se permiten dos beans con el mismo id**.
        
- Solución: dar un **nombre diferente** a uno de ellos (por ejemplo `address1`).  
    Con nombres distintos, Spring buscará el que coincida exactamente con el nombre de la variable.
    

---

### Resumen completo del documento

El autowiring **byName** permite que el contenedor Spring inyecte automáticamente las dependencias **buscando el bean por su nombre**:

1. En la clase dependiente (por ejemplo `Employee`) Spring toma el **nombre de la propiedad** (`address`).
    
2. En el XML debe existir un bean con **id idéntico** (`id="address"`).
    
3. Spring ignora el tipo de clase: solo importa el **nombre**.
    
4. Si no encuentra un bean con ese nombre, la propiedad queda en **null**.
    
5. Si hay **dos beans con el mismo id**, Spring lanza un error de configuración.
    
6. Para evitar el error, cada bean debe tener un **id único**.
    

Con esto, se completa la explicación de las dos principales modalidades de autoconexión en XML:

- `byType` (por tipo de clase)
    
- `byName` (por id/nombre del bean).