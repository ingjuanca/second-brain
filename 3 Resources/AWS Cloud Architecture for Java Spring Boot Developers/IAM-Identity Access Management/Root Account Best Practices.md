
---

## Mejores prácticas para la cuenta Root

Hasta ahora, todos hemos estado usando las **credenciales del usuario root** para acceder a la consola de AWS.

Sin embargo, AWS **recomienda fuertemente no usar la cuenta root para tareas diarias**.

Tal vez ahora pienses:  
“Entonces, ¿por qué la hemos usado hasta ahora?”

No te preocupes.

La razón principal es que el **usuario root tiene privilegios ilimitados** dentro de la cuenta.

Si alguien roba estas credenciales, puede:

- Eliminar toda tu infraestructura
    
- Acceder a todos tus datos de producción
    
- Ir a IAM y eliminar usuarios, grupos y roles
    
- Bloquear completamente el acceso a la cuenta
    

Por eso, **habilitamos MFA (Multi-Factor Authentication) como primer paso**, para proteger la cuenta root.

---

### Recomendación de AWS

AWS recomienda lo siguiente:

1. **Crear un usuario administrador** para el uso diario
    
2. Mantener la **cuenta root protegida y sin usar**
    

---

### Creación de un usuario administrador

Creamos un usuario IAM, por ejemplo llamado **Windows Admin** (el nombre es solo ilustrativo).

- Le damos acceso a la consola de AWS
    
- Definimos una contraseña personalizada
    
- Creamos el usuario sin asignarle permisos directamente
    

---

### Crear un grupo de administradores

Luego:

- Creamos un **User Group** llamado **Administrators**
    
- Agregamos únicamente al usuario administrador creado
    
- Asignamos la policy **AdministratorAccess** al grupo
    

Ahora, este usuario puede hacer **todo lo necesario para tareas diarias**, sin usar la cuenta root.

---

### Beneficio clave

Si algún día:

- Pierdes las credenciales del usuario administrador
    
- O sospechas que fueron comprometidas
    

Simplemente puedes:

- Eliminar ese usuario IAM
    
- Crear uno nuevo
    

👉 **La cuenta root permanece segura e intacta**.

---

### Uso cotidiano recomendado

A partir de este punto:

- Usa el **usuario administrador** para tareas administrativas
    
- Usa **usuarios desarrolladores** para tareas técnicas
    
- **No uses nunca la cuenta root**, salvo casos excepcionales
    

---

### Acceso a Billing (Facturación)

Por defecto:

- Los usuarios IAM (incluso administradores) **no pueden ver Billing**
    

Para permitirlo:

1. Inicias sesión con la **cuenta root**
    
2. Vas a **Account settings**
    
3. Habilitas la opción:
    
    > _IAM users and roles can access billing information_
    
4. Guardas los cambios
    

Después de esto:

- El usuario administrador puede acceder a información de facturación
    

---

### Estado final recomendado

Al final, tendrás:

- ✅ **Cuenta root**
    
    - Protegida con MFA
        
    - Guardada de forma segura
        
    - No usada en el día a día
        
- ✅ **Cuenta administrador (IAM)**
    
    - Para tareas administrativas
        
    - Con permisos completos
        
- ✅ **Cuentas de desarrolladores**
    
    - Con permisos limitados
        
    - Aplicando principio de menor privilegio
        

---

# 📝 **RESUMEN COMPLETO (VERSIÓN PROFESIONAL)**

Este documento explica las **mejores prácticas de seguridad para la cuenta root de AWS**, y cómo estructurar correctamente los accesos administrativos usando IAM.

---

## ⭐ 1. Riesgos del uso de la cuenta root

- Tiene **privilegios ilimitados**
    
- Su compromiso implica **pérdida total de la cuenta**
    
- Permite borrar usuarios, roles, grupos y datos críticos
    

---

## ⭐ 2. Recomendación oficial de AWS

- **Nunca usar la cuenta root para tareas diarias**
    
- Habilitar **MFA obligatoriamente**
    
- Guardar las credenciales root en un lugar seguro
    

---

## ⭐ 3. Modelo de acceso recomendado

- Crear un **usuario IAM administrador**
    
- Asignarle permisos mediante un **grupo Administrators**
    
- Usar ese usuario para administración diaria
    
- Crear usuarios separados para desarrolladores
    

---

## ⭐ 4. Manejo de Billing

- Por defecto, IAM no accede a Billing
    
- El acceso debe habilitarse explícitamente desde la cuenta root
    
- Esto permite delegar facturación sin exponer root
    

---

# 🎯 **Idea principal**

La **cuenta root es un “botón nuclear”** y debe mantenerse protegida, con MFA y sin uso cotidiano.  
La administración diaria debe hacerse mediante **usuarios IAM administradores**, permitiendo mayor control, seguridad y capacidad de respuesta ante incidentes.

---

Si quieres, el siguiente paso ideal sería:

✅ Diseñar una **estructura IAM completa (admin + dev + roles)**  
✅ Crear un **checklist de seguridad inicial para cuentas AWS nuevas**  
✅ Prepararte para **preguntas de entrevista sobre seguridad e IAM**  
✅ Ver este modelo aplicado a un **equipo real de desarrollo**