
---

Vamos a aprender AWS Cloud desde cero en la siguiente clase. Antes de eso, quiero que configures tu cuenta de AWS para que puedas seguir todas las sesiones prácticas sin interrupciones.

Hay seis pasos para configurar tu cuenta de AWS. Te los explicaré en un momento.

Una vez creada la cuenta, habilitaremos la autenticación multifactor (MFA) para tu inicio de sesión, solo por seguridad. Luego configuraremos una alerta de facturación porque tendrás que ingresar una tarjeta de crédito o débito, y no queremos cargos inesperados. Esta alerta te enviará una notificación si tu uso supera cierto límite. Así que terminemos esto.

Primero, ve a aws.amazon.com. Haz clic en el botón “Create Account”. Verás esta página. Ingresa tu correo electrónico y un nombre para la cuenta (puede ser el que quieras). Haz clic en “Verify Email Address”. Recibirás un correo con un enlace; haz clic para continuar con el proceso.

Ese es el primer paso.

Luego, como parte del plan de cuenta, te sugiero elegir el plan de pago para desarrollar cargas de trabajo listas para producción. No te preocupes: recibirás hasta $200 en créditos, al menos en el momento en que se grabó esta clase. Esta oferta incluye uso gratuito de algunos servicios seleccionados. Esta es la opción que recomiendo. Con el otro plan no tendrás acceso a ciertos servicios.

Después, en la información de contacto, ingresa tu nombre, número de teléfono, dirección, etc. Escoge “Personal”, no “Business”, ya que es para tus propios proyectos.

Luego, en la información de pago, deberás ingresar los datos de tu tarjeta de crédito o débito.

Después verificarán tu identidad (quinto paso).

En el plan de soporte, elegiremos el plan gratuito. No necesitamos otro. Selecciona “Basic Support (Free)”.

Después de esto, tu cuenta estará lista.

Pausa esta clase y crea tu cuenta de AWS. Cuando termines, vuelve para continuar con los siguientes pasos.

Una vez que hayas iniciado sesión, lo primero que quiero que hagamos es configurar la autenticación multifactor para la cuenta root. Eso es súper importante.

La interfaz puede verse ligeramente diferente para ti. No pasa nada. Busquemos el servicio IAM (“Manage access to AWS resources”). Haz clic allí. Probablemente veas una recomendación de seguridad indicando activar MFA en el usuario root. Haremos eso.

Para el nombre del dispositivo puedes dar cualquier nombre, como tu nombre + “AWS”. En las opciones del dispositivo selecciona “Authenticator App”. Es una app común que funciona en Android y iOS. Selecciona esa opción.

Luego haz clic para mostrar el código QR. En tu dispositivo móvil abre la aplicación Google Authenticator. Si no la tienes, instálala. Escanea el código QR. Aparecerá una entrada nueva en la app. Ingresa los códigos que genere en AWS y haz clic en “Add MFA”. Con eso quedará configurado.

Luego haz clic en tu nombre de cuenta → “Billing and Cost Management”.

No te preocupes por mi pantalla; yo he usado servicios adicionales no relacionados con el curso, así que verás más cargos. Pero el costo real del curso fue menos de $3 USD.

Lo siguiente que quiero que hagamos es crear un presupuesto. En el menú lateral, ve a “Budgets” → “Create Budget”.

Selecciona la plantilla simplificada. Dado que en el curso usaremos servicios que pueden generar costos (poco, pero algo), seleccionamos “Monthly Cost Budget”.

El presupuesto por defecto es $100 USD, pero es demasiado. Cámbialo a algo razonable como $5 USD. Luego ingresa tu correo para recibir alertas cuando se alcance el límite.

AWS enviará un correo incluso antes de llegar al límite, así que no hay riesgo.

Crea el presupuesto.

Además, cada vez que inicies sesión durante el curso, revisa la página principal de “Billing and Cost Management”. Ahí verás cuánto has gastado hasta ahora. Revisa esto diariamente para tener tranquilidad y evitar sorpresas.

---

# 📝 **Resumen completo del documento**

El documento explica los pasos iniciales necesarios antes de comenzar un curso práctico de AWS. Su objetivo es garantizar que cada estudiante tenga su cuenta configurada correctamente y pueda trabajar sin interrupciones, con seguridad y control de costos.

### **1. Configuración inicial de la cuenta AWS (6 pasos)**

- Crear la cuenta ingresando correo y datos personales.
    
- Verificar correo electrónico.
    
- Escoger el plan recomendado (plan de pago con créditos promocionales de ~$200).
    
- Ingresar información personal y de contacto (modo “Personal”).
    
- Registrar tarjeta de crédito o débito para validación.
    
- Seleccionar el plan de soporte gratuito (“Basic Support”).
    

### **2. Habilitar Autenticación Multifactor (MFA) en la cuenta root**

- Acceder a IAM (“Manage access to AWS resources”).
    
- Seleccionar la recomendación de seguridad para activar MFA.
    
- Usar una aplicación autenticadora como Google Authenticator.
    
- Escanear el QR y agregar los códigos generados para completar la configuración.  
    **Motivo:** proteger la cuenta root, que tiene acceso completo.
    

### **3. Configurar un presupuesto y alarmas de facturación**

- Ir a Billing → Budgets → Create Budget.
    
- Escoger “Monthly cost budget”.
    
- Cambiar el límite por defecto ($100) a uno más razonable como $5 USD.
    
- Agregar el correo para recibir alertas antes de exceder el límite.  
    **Motivo:** evitar cargos inesperados al usar los servicios de AWS.
    

### **4. Recomendación de práctica diaria**

- Monitorear el panel de costos cada día para llevar control del gasto y evitar sorpresas.
    

En resumen, el documento guía a los estudiantes paso a paso para crear una cuenta AWS segura, con MFA activado, control de costos y lista para ejecutar todos los ejercicios prácticos del curso sin riesgos.

---
