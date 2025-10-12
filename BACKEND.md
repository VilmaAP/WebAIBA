# 🔧 Guía de Backend - AIBA

Esta guía te muestra cómo conectar el formulario de contacto con diferentes backends.

## 📋 Opciones de Backend

### 1. 🚀 Formspree (Más Fácil - Recomendado)

**Ventajas:**
- ✅ Sin código backend
- ✅ Setup en 2 minutos
- ✅ Gratis hasta 50 submissions/mes
- ✅ Anti-spam incluido

**Implementación:**

1. Crear cuenta en [formspree.io](https://formspree.io)
2. Crear nuevo form, copiar el endpoint
3. Actualizar `script.js`:

```javascript
// En la función submit del formulario (línea ~160)
contactForm.addEventListener('submit', async (e) => {
    e.preventDefault();
    
    const submitBtn = contactForm.querySelector('button[type="submit"]');
    const originalText = submitBtn.innerHTML;
    submitBtn.disabled = true;
    submitBtn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Enviando...';
    
    const formData = new FormData(contactForm);
    
    try {
        const response = await fetch('https://formspree.io/f/TU_FORM_ID', {
            method: 'POST',
            body: formData,
            headers: {
                'Accept': 'application/json'
            }
        });
        
        if (response.ok) {
            showNotification('¡Gracias! Te contactaremos pronto.', 'success');
            contactForm.reset();
            
            // Opcional: Redirigir a Calendly
            setTimeout(() => {
                window.open('https://calendly.com/TU_USUARIO', '_blank');
            }, 2000);
        } else {
            throw new Error('Error en el envío');
        }
    } catch (error) {
        console.error('Error:', error);
        showNotification('Hubo un error. Por favor, intenta nuevamente.', 'error');
    } finally {
        submitBtn.disabled = false;
        submitBtn.innerHTML = originalText;
    }
});
```

---

### 2. 📨 Netlify Forms

**Ventajas:**
- ✅ Incluido gratis en Netlify
- ✅ Sin JavaScript necesario
- ✅ Submissions en el dashboard

**Implementación:**

1. Actualizar el form en `index.html`:

```html
<form id="contactForm" 
      class="contact-form" 
      name="contacto" 
      method="POST" 
      data-netlify="true"
      data-netlify-honeypot="bot-field">
    
    <!-- Campo anti-spam oculto -->
    <input type="hidden" name="form-name" value="contacto">
    <p style="display:none;">
        <label>Don't fill this out: <input name="bot-field"></label>
    </p>
    
    <!-- Resto de campos normales -->
    <div class="form-group">
        <label for="nombre">Nombre completo</label>
        <input type="text" id="nombre" name="nombre" required>
    </div>
    <!-- ... más campos -->
</form>
```

2. Las submissions aparecerán en: Netlify Dashboard > Forms

3. Configurar notificaciones por email en Netlify

---

### 3. 📧 EmailJS

**Ventajas:**
- ✅ Envía emails directamente
- ✅ Sin backend necesario
- ✅ Gratis hasta 200 emails/mes

**Implementación:**

1. Crear cuenta en [emailjs.com](https://www.emailjs.com/)
2. Configurar servicio de email (Gmail, Outlook, etc.)
3. Crear email template
4. Agregar SDK antes de `</body>` en `index.html`:

```html
<script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>
<script>
    emailjs.init("TU_PUBLIC_KEY");
</script>
```

5. Actualizar `script.js`:

```javascript
contactForm.addEventListener('submit', async (e) => {
    e.preventDefault();
    
    const submitBtn = contactForm.querySelector('button[type="submit"]');
    submitBtn.disabled = true;
    submitBtn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Enviando...';
    
    const templateParams = {
        nombre: document.getElementById('nombre').value,
        empresa: document.getElementById('empresa').value,
        email: document.getElementById('email').value,
        telefono: document.getElementById('telefono').value,
        industria: document.getElementById('industria').value,
        mensaje: document.getElementById('mensaje').value
    };
    
    try {
        await emailjs.send('TU_SERVICE_ID', 'TU_TEMPLATE_ID', templateParams);
        showNotification('¡Mensaje enviado con éxito!', 'success');
        contactForm.reset();
    } catch (error) {
        console.error('Error:', error);
        showNotification('Error al enviar. Intenta nuevamente.', 'error');
    } finally {
        submitBtn.disabled = false;
        submitBtn.innerHTML = originalText;
    }
});
```

---

### 4. 🔥 Firebase (Google)

**Ventajas:**
- ✅ Backend completo de Google
- ✅ Base de datos en tiempo real
- ✅ Autenticación incluida

**Setup:**

1. Crear proyecto en [Firebase Console](https://console.firebase.google.com/)
2. Crear Firestore Database
3. Agregar Firebase SDK antes de `</body>`:

```html
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
<script>
    const firebaseConfig = {
        apiKey: "TU_API_KEY",
        authDomain: "TU_PROYECTO.firebaseapp.com",
        projectId: "TU_PROYECTO",
        storageBucket: "TU_PROYECTO.appspot.com",
        messagingSenderId: "TU_ID",
        appId: "TU_APP_ID"
    };
    
    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();
</script>
```

4. Actualizar `script.js`:

```javascript
contactForm.addEventListener('submit', async (e) => {
    e.preventDefault();
    
    const submitBtn = contactForm.querySelector('button[type="submit"]');
    submitBtn.disabled = true;
    submitBtn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Enviando...';
    
    const formData = {
        nombre: document.getElementById('nombre').value,
        empresa: document.getElementById('empresa').value,
        email: document.getElementById('email').value,
        telefono: document.getElementById('telefono').value,
        industria: document.getElementById('industria').value,
        mensaje: document.getElementById('mensaje').value,
        timestamp: firebase.firestore.FieldValue.serverTimestamp()
    };
    
    try {
        await db.collection('contactos').add(formData);
        showNotification('¡Gracias por contactarnos!', 'success');
        contactForm.reset();
        
        // Enviar email via Cloud Function (opcional)
        // await fetch('https://us-central1-TU_PROYECTO.cloudfunctions.net/sendEmail', {
        //     method: 'POST',
        //     body: JSON.stringify(formData)
        // });
        
    } catch (error) {
        console.error('Error:', error);
        showNotification('Error al enviar. Intenta nuevamente.', 'error');
    } finally {
        submitBtn.disabled = false;
        submitBtn.innerHTML = originalText;
    }
});
```

---

### 5. 🌐 Backend Propio (Node.js + Express)

**Para mayor control y funcionalidades avanzadas.**

**Estructura del proyecto:**
```
backend/
├── server.js
├── package.json
└── .env
```

**1. Crear `backend/package.json`:**

```json
{
  "name": "aiba-backend",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "cors": "^2.8.5",
    "dotenv": "^16.3.1",
    "nodemailer": "^6.9.7"
  },
  "devDependencies": {
    "nodemon": "^3.0.2"
  }
}
```

**2. Crear `backend/server.js`:**

```javascript
require('dotenv').config();
const express = require('express');
const cors = require('cors');
const nodemailer = require('nodemailer');

const app = express();
const PORT = process.env.PORT || 3001;

// Middleware
app.use(cors());
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Configurar transporte de email
const transporter = nodemailer.createTransport({
    service: 'gmail',
    auth: {
        user: process.env.EMAIL_USER,
        pass: process.env.EMAIL_PASS
    }
});

// Endpoint para formulario de contacto
app.post('/api/contact', async (req, res) => {
    try {
        const { nombre, empresa, email, telefono, industria, mensaje } = req.body;
        
        // Validación básica
        if (!nombre || !email || !telefono) {
            return res.status(400).json({ 
                error: 'Campos requeridos faltantes' 
            });
        }
        
        // Email al equipo de ventas
        const mailOptions = {
            from: process.env.EMAIL_USER,
            to: 'ventas@aibalabs.ai',
            subject: `Nuevo Lead: ${nombre} - ${empresa}`,
            html: `
                <h2>Nuevo contacto desde la web</h2>
                <p><strong>Nombre:</strong> ${nombre}</p>
                <p><strong>Empresa:</strong> ${empresa}</p>
                <p><strong>Email:</strong> ${email}</p>
                <p><strong>Teléfono:</strong> ${telefono}</p>
                <p><strong>Industria:</strong> ${industria}</p>
                <p><strong>Mensaje:</strong></p>
                <p>${mensaje || 'Sin mensaje'}</p>
                <hr>
                <p><small>Enviado el ${new Date().toLocaleString('es-AR')}</small></p>
            `
        };
        
        await transporter.sendMail(mailOptions);
        
        // Email de confirmación al cliente
        const confirmationMail = {
            from: process.env.EMAIL_USER,
            to: email,
            subject: '¡Gracias por contactar a AIBA!',
            html: `
                <div style="font-family: Arial, sans-serif; max-width: 600px; margin: 0 auto;">
                    <h1 style="color: #6366f1;">¡Hola ${nombre}!</h1>
                    <p>Gracias por tu interés en AIBA. Hemos recibido tu solicitud y nos pondremos en contacto contigo dentro de las próximas 24 horas.</p>
                    <p>Mientras tanto, puedes:</p>
                    <ul>
                        <li><a href="https://calendly.com/aiba/diagnostico">Agendar tu diagnóstico gratuito</a></li>
                        <li><a href="https://aibalabs.ai">Explorar más sobre AIBA</a></li>
                    </ul>
                    <p>¡Saludos!</p>
                    <p><strong>Equipo AIBA</strong></p>
                    <hr>
                    <p style="font-size: 12px; color: #666;">
                        Este es un email automático. No respondas a este mensaje.
                    </p>
                </div>
            `
        };
        
        await transporter.sendMail(confirmationMail);
        
        // Integración con CRM (opcional)
        // await integrarConHubSpot({ nombre, email, telefono, empresa });
        
        res.json({ 
            success: true, 
            message: 'Contacto enviado exitosamente' 
        });
        
    } catch (error) {
        console.error('Error:', error);
        res.status(500).json({ 
            error: 'Error al procesar la solicitud' 
        });
    }
});

// Health check
app.get('/api/health', (req, res) => {
    res.json({ status: 'OK', timestamp: new Date() });
});

app.listen(PORT, () => {
    console.log(`🚀 Servidor corriendo en puerto ${PORT}`);
});
```

**3. Crear `backend/.env`:**

```env
PORT=3001
EMAIL_USER=tu-email@gmail.com
EMAIL_PASS=tu-app-password
```

**4. Actualizar frontend `script.js`:**

```javascript
contactForm.addEventListener('submit', async (e) => {
    e.preventDefault();
    
    const submitBtn = contactForm.querySelector('button[type="submit"]');
    submitBtn.disabled = true;
    submitBtn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Enviando...';
    
    const formData = {
        nombre: document.getElementById('nombre').value,
        empresa: document.getElementById('empresa').value,
        email: document.getElementById('email').value,
        telefono: document.getElementById('telefono').value,
        industria: document.getElementById('industria').value,
        mensaje: document.getElementById('mensaje').value
    };
    
    try {
        const response = await fetch('http://localhost:3001/api/contact', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(formData)
        });
        
        const data = await response.json();
        
        if (response.ok) {
            showNotification('¡Gracias! Revisa tu email.', 'success');
            contactForm.reset();
        } else {
            throw new Error(data.error);
        }
    } catch (error) {
        console.error('Error:', error);
        showNotification('Error al enviar. Intenta nuevamente.', 'error');
    } finally {
        submitBtn.disabled = false;
        submitBtn.innerHTML = originalText;
    }
});
```

**5. Desplegar backend:**

**En Heroku:**
```bash
cd backend
heroku create aiba-backend
git push heroku main
heroku config:set EMAIL_USER=tu-email@gmail.com
heroku config:set EMAIL_PASS=tu-password
```

**En Railway:**
```bash
# Conectar repo en railway.app
# Variables de entorno se configuran en el dashboard
```

**En Vercel (Serverless):**
Crear `api/contact.js`:
```javascript
// Igual que server.js pero exportando handler
module.exports = async (req, res) => {
    // código del endpoint
};
```

---

## 📊 Integraciones CRM

### HubSpot

```javascript
async function enviarAHubSpot(datos) {
    const response = await fetch('https://api.hsforms.com/submissions/v3/integration/submit/TU_PORTAL_ID/TU_FORM_ID', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            fields: [
                { name: 'firstname', value: datos.nombre },
                { name: 'email', value: datos.email },
                { name: 'phone', value: datos.telefono },
                { name: 'company', value: datos.empresa }
            ]
        })
    });
    return response.json();
}
```

### Zapier Webhook

```javascript
async function enviarAZapier(datos) {
    await fetch('https://hooks.zapier.com/hooks/catch/TU_WEBHOOK_ID/', {
        method: 'POST',
        body: JSON.stringify(datos)
    });
}
```

---

## 🔒 Seguridad

### Protección contra spam

```javascript
// Honeypot (campo oculto)
<input type="text" name="website" style="display:none" tabindex="-1" autocomplete="off">

// En el backend
if (req.body.website) {
    return res.status(400).json({ error: 'Spam detectado' });
}

// Rate limiting
const rateLimit = require('express-rate-limit');
const limiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutos
    max: 5 // 5 requests por IP
});
app.use('/api/contact', limiter);
```

---

## 📈 Analytics

Trackear submissions:

```javascript
// Después de envío exitoso
if (typeof gtag !== 'undefined') {
    gtag('event', 'form_submit', {
        event_category: 'engagement',
        event_label: 'Contact Form'
    });
}

if (typeof fbq !== 'undefined') {
    fbq('track', 'Lead');
}
```

---

## ✅ Checklist de Implementación

- [ ] Elegir solución de backend
- [ ] Configurar credenciales/API keys
- [ ] Actualizar código frontend
- [ ] Probar en desarrollo
- [ ] Configurar email de confirmación
- [ ] Configurar notificaciones al equipo
- [ ] Integrar con CRM (opcional)
- [ ] Implementar protección anti-spam
- [ ] Configurar analytics tracking
- [ ] Probar en producción
- [ ] Documentar para el equipo

---

¿Preguntas? Consulta la documentación de cada servicio o contacta al equipo técnico. 🚀
