
---

En esta lección hablaremos rápidamente sobre el **security group que se auto-referencia** (_self-referencing security group_).

Ve a _Network Security Groups_.

Aquí aterrizarás en esta página.

Aquí tenemos el **security group default**.

Si recuerdas, dijimos que por defecto **todas las solicitudes inbound están denegadas**, y debemos agregar las reglas “allow” una por una.  
Eso es lo que habíamos dicho.

Bien.

Este es el security group default. Y si verificas, tiene **una regla inbound por defecto**.

Habíamos dicho que por defecto no habría reglas, pero el security group default es una **excepción**.  
Si revisas, tiene una regla inbound.

Si observas, dice que **todo el tráfico está permitido**.  
De hecho, eso es lo que indica: todos los puertos están abiertos, cualquier tráfico está permitido.

Sin embargo, si revisas el **source**, notarás que hace referencia a un **security group**.  
Y si lo verificas, en realidad es **su propio ID**.

Así que básicamente **se está permitiendo a sí mismo**.

Podrías preguntarte: “¿Por qué existe una regla inbound auto-referenciada? No tiene sentido”.  
Puedes pensar eso, pero a veces puedes tener una arquitectura de microservicios con múltiples servicios que necesitan comunicarse entre sí.

Por ejemplo:  
Si tienes muchos servicios que se llaman unos a otros, en lugar de crear múltiples security groups para cada aplicación, podrías crear **un solo security group**, que se permita a sí mismo, y adjuntar ese security group a todos los microservicios.  
Así, si el servicio _orders_ necesita hablar con el servicio _payment_, podrá hacerlo.

Es solo una opción para explorar. No digo que esta sea la forma que debas usar siempre.

Volvamos a la consola de AWS.

Vamos a crear un nuevo security group con una regla inbound auto-referenciada, solo para practicar.

Creamos un security group y le damos un nombre.  
En este caso, el nombre no importa mucho.

No podemos agregar reglas inbound durante la creación, así que ignoramos eso.

Creamos el security group.

Ahora que ya está creado, vamos a **Edit inbound rules**.

Aquí podemos agregar la regla.

Si quieres permitir todo el tráfico, puedes hacerlo.  
O si quieres permitir solo el puerto 80, también puedes hacerlo.

Ahora podemos seleccionar el **mismo security group** como origen (_source_), y de esta forma la regla se vuelve auto-referenciada.

Así es como agregamos una regla self-referencing.

Espero que esto esté claro.

Usaremos este concepto más adelante cuando desarrollemos y despleguemos nuestra aplicación.

Por ahora está bien; podemos eliminar este security group.

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

El documento explica qué es un **security group auto-referenciado** en AWS, por qué existe y cómo crear uno.

---

## ⭐ 1. ¿Qué es un Security Group auto-referenciado?

Es un security group cuya regla inbound permite tráfico **proveniente del mismo security group**.  
Es decir, el security group **se autoriza a sí mismo** como origen.

Ejemplo:  
Inbound rule:

- Tipo: All traffic
    
- Source: sg-12345 (el mismo SG)
    

---

## ⭐ 2. ¿Por qué el SG “default” tiene esta regla?

Aunque normalmente los SG no tienen reglas inbound, el **security group default** es una excepción:

- Permite **todo el tráfico intra-grupo**
    
- Pero **solo** si proviene de instancias que usan ese mismo SG
    

Esto permite que instancias dentro del mismo default SG se comuniquen libremente entre sí.

---

## ⭐ 3. ¿Cuándo se usa esto?

Es útil en escenarios donde múltiples microservicios necesitan comunicarse entre sí, por ejemplo:

- service-orders → service-payments
    
- service-shipping → service-inventory
    

En lugar de crear muchos SG para cada interacción, puedes:

- Crear un SG único
    
- Activar una regla self-referencing
    
- Adjuntar ese SG a todos los microservicios
    

Esto permite comunicación interna sin exponer puertos a direcciones IP externas.

---

## ⭐ 4. Cómo crear uno (pasos)

1. Crear un security group sin reglas inbound.
    
2. Una vez creado, ir a **Edit inbound rules**.
    
3. Agregar una regla (all traffic o un puerto específico).
    
4. En el origen (_source_), seleccionar **el mismo security group**.
    
5. Guardar.
    

Resultado: el SG se permite a sí mismo.

---

## ⭐ 5. Uso futuro

Este patrón se utilizará más adelante en el curso al desplegar aplicaciones multi-tier o microservicios.

---

Si quieres, puedo también:

✅ generar una versión ultra resumida (5–8 líneas)  
✅ crear un diagrama visual ASCII  
✅ comparar este patrón con SG → SG (app-SG → db-SG)  
✅ explicarlo con un ejemplo basado en Spring Boot y microservicios