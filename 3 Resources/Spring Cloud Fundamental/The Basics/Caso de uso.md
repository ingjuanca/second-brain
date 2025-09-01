
---

Antes de comenzar a dominar los componentes de Spring Cloud, necesitamos tener un par de microservicios implementados, de manera que podamos integrar los componentes de Spring donde y cuando se requiera.

Vas a crear dos microservicios:

- **Product Microservice (Servicio de Productos)**
- **Coupon Microservice (Servicio de Cupones)**

Estos dos servicios expondrán **APIs RESTful** que permitirán a un usuario final crear un producto.

En el proceso, el **Product Service** usará el **Coupon Service** para aplicar un código de cupón que el cliente envía, y así obtener un descuento.

Toda la información de los cupones será gestionada por el **Coupon Service**.  
El **Product Service** solo será responsable de:

- Crear el producto en la base de datos,
- Guardar su precio,
- Guardar la descripción y demás detalles que el cliente envíe.

### Flujo del proceso:

1. El cliente envía una solicitud para crear un producto.
2. El **Product Service** llama al **Coupon Service** para obtener el descuento correspondiente al código de cupón que el cliente proporcionó.   
3. El **Product Service** aplica ese descuento al precio dado por el cliente.
4. El **Product Service** guarda el producto con el precio ajustado en la base de datos.
5. Todos los detalles de los cupones se guardan en la base de datos del **Coupon Service**.

El **Coupon Service** expondrá un par de APIs REST:

- Una para **crear cupones**,
- Otra para **obtener detalles de un cupón** a partir de su código.

---

## 📝 Resumen estructurado

🔹 **Contexto:**  
Se plantea un caso práctico para implementar y probar componentes de Spring Cloud con dos microservicios.

🔹 **Microservicios creados:**

1. **Coupon Service:**
    
    - Administra toda la información de cupones.
        
    - Base de datos propia para cupones.
        
    - APIs REST:
        
        - Crear cupones.
            
        - Obtener detalles de un cupón por su código.
            
2. **Product Service:**
    
    - Responsable de crear productos en la base de datos.
        
    - Recibe precio, descripción y detalles desde el cliente.
        
    - Llama al Coupon Service para validar y aplicar descuentos.
        

🔹 **Flujo:**  
Cliente → Product Service → Coupon Service → descuento aplicado → producto guardado en BD.

---