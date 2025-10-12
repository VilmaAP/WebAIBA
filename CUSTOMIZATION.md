# 🎨 Guía de Personalización - AIBA

Esta guía te ayudará a personalizar el sitio web de AIBA según tus necesidades específicas.

## 🎨 Cambiar Colores

### Colores Principales

Edita las variables CSS en `styles.css` (líneas 10-35):

```css
:root {
    /* Colores principales */
    --primary-color: #6366f1;       /* Azul principal */
    --primary-dark: #4f46e5;        /* Azul oscuro */
    --primary-light: #818cf8;       /* Azul claro */
    --secondary-color: #ec4899;     /* Rosa */
    --accent-color: #14b8a6;        /* Turquesa */
    --success-color: #10b981;       /* Verde */
    --warning-color: #f59e0b;       /* Naranja */
    --danger-color: #ef4444;        /* Rojo */
}
```

### Paletas de Colores Sugeridas

**Opción 1: Azul Corporativo**
```css
--primary-color: #2563eb;
--primary-dark: #1e40af;
--primary-light: #3b82f6;
```

**Opción 2: Verde Tecnológico**
```css
--primary-color: #059669;
--primary-dark: #047857;
--primary-light: #10b981;
```

**Opción 3: Morado Premium**
```css
--primary-color: #7c3aed;
--primary-dark: #6d28d9;
--primary-light: #8b5cf6;
```

**Opción 4: Naranja Energético**
```css
--primary-color: #ea580c;
--primary-dark: #c2410c;
--primary-light: #f97316;
```

---

## ✍️ Cambiar Tipografía

### Usar Otra Fuente de Google Fonts

1. Ve a [Google Fonts](https://fonts.google.com/)
2. Selecciona tu fuente (ej: Poppins, Montserrat, Roboto)
3. Reemplaza en `index.html` (línea ~25):

```html
<!-- Cambiar esto: -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">

<!-- Por esto (ejemplo con Poppins): -->
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
```

4. Actualiza en `styles.css` (línea ~72):

```css
body {
    font-family: 'Poppins', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}
```

---

## 📝 Cambiar Contenido

### Hero Section

Edita el título principal en `index.html` (línea ~68):

```html
<h1 class="hero-title">
    ¿Pierdes <span class="gradient-text">reservas y citas</span> por no responder al instante?
</h1>
```

### Estadísticas del Hero

Cambia los números en `index.html` (línea ~78):

```html
<div class="hero-stats">
    <div class="stat">
        <div class="stat-number">24/7</div>
        <div class="stat-label">Atención continua</div>
    </div>
    <!-- Modifica según tus métricas -->
</div>
```

### Precios

Actualiza los planes en `index.html` (sección de precios, línea ~400+):

```html
<div class="pricing-price">
    <span class="currency">$</span>
    <span class="amount">297</span>  <!-- Cambia el precio -->
    <span class="period">/mes</span>
</div>
```

### Testimonios

Agrega o edita testimonios en `index.html` (sección testimonios, línea ~480+):

```html
<div class="testimonial-card">
    <!-- Edita el contenido aquí -->
    <p class="testimonial-text">"Tu testimonio aquí..."</p>
    <div class="author-info">
        <h4>Nombre Cliente</h4>
        <p>Cargo/Empresa</p>
    </div>
</div>
```

---

## 🖼️ Agregar Imágenes

### Logos

1. Crea carpeta `images/`
2. Agrega tu logo: `images/logo.png`
3. Reemplaza en `index.html`:

```html
<!-- En lugar del icono, usa imagen -->
<div class="logo">
    <img src="images/logo.png" alt="AIBA Logo" width="150">
</div>
```

### Imágenes de Fondo

Agregar imagen al Hero:

```css
/* En styles.css */
.hero {
    background-image: url('../images/hero-bg.jpg');
    background-size: cover;
    background-position: center;
}

/* Agregar overlay oscuro */
.hero::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.5);
    z-index: -1;
}
```

### Avatares en Testimonios

```html
<div class="author-avatar">
    <img src="images/cliente-1.jpg" alt="Felipe Acosta">
</div>
```

```css
.author-avatar img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    border-radius: 50%;
}
```

---

## 🔗 Integrar Calendly

### Opción 1: Popup Modal

En `index.html`, reemplaza el botón de CTA:

```html
<a href="#" class="btn btn-primary btn-lg" 
   onclick="Calendly.initPopupWidget({url: 'https://calendly.com/TU_USUARIO/diagnostico'});return false;">
    <i class="fas fa-calendar-check"></i>
    Agendar Diagnóstico Gratuito
</a>

<!-- Agregar antes de </body> -->
<link href="https://assets.calendly.com/assets/external/widget.css" rel="stylesheet">
<script src="https://assets.calendly.com/assets/external/widget.js" type="text/javascript" async></script>
```

### Opción 2: Widget Embebido

Reemplaza el formulario por widget de Calendly:

```html
<div class="calendly-inline-widget" 
     data-url="https://calendly.com/TU_USUARIO/diagnostico" 
     style="min-width:320px;height:700px;">
</div>
<script type="text/javascript" src="https://assets.calendly.com/assets/external/widget.js" async></script>
```

---

## 📧 Conectar Formulario

### Opción 1: Formspree (Recomendado)

1. Crear cuenta en [formspree.io](https://formspree.io)
2. Crear form, obtener endpoint
3. Actualizar en `script.js`:

```javascript
contactForm.addEventListener('submit', async (e) => {
    e.preventDefault();
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
            showNotification('¡Solicitud enviada!', 'success');
            contactForm.reset();
        }
    } catch (error) {
        showNotification('Error al enviar', 'error');
    }
});
```

### Opción 2: Netlify Forms

1. Agregar `netlify` al form en `index.html`:

```html
<form id="contactForm" class="contact-form" name="contacto" method="POST" data-netlify="true">
    <input type="hidden" name="form-name" value="contacto">
    <!-- resto del formulario -->
</form>
```

2. Las submissions llegarán a tu panel de Netlify

### Opción 3: Google Forms

1. Crear form en Google Forms
2. Obtener action URL
3. Actualizar form:

```html
<form action="TU_GOOGLE_FORM_URL" method="POST" target="_blank">
```

---

## 🎬 Agregar Videos

### Video de Fondo en Hero

```html
<section class="hero">
    <video autoplay muted loop playsinline class="hero-video">
        <source src="videos/hero-bg.mp4" type="video/mp4">
    </video>
    <!-- contenido del hero -->
</section>
```

```css
.hero-video {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    z-index: -1;
    opacity: 0.3;
}
```

### Video de Demostración

Reemplazar la simulación de chat por video:

```html
<div class="hero-image">
    <div class="video-container">
        <iframe 
            src="https://www.youtube.com/embed/TU_VIDEO_ID" 
            frameborder="0" 
            allow="autoplay; encrypted-media" 
            allowfullscreen>
        </iframe>
    </div>
</div>
```

```css
.video-container {
    position: relative;
    padding-bottom: 56.25%; /* 16:9 */
    height: 0;
    overflow: hidden;
}

.video-container iframe {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
}
```

---

## 🌐 Traducción (Multi-idioma)

### Estructura Básica

Crear `js/i18n.js`:

```javascript
const translations = {
    es: {
        hero_title: "¿Pierdes reservas y citas por no responder al instante?",
        hero_cta: "Agendar Diagnóstico Gratuito"
    },
    en: {
        hero_title: "Losing bookings by not responding instantly?",
        hero_cta: "Schedule Free Diagnosis"
    }
};

function setLanguage(lang) {
    document.querySelectorAll('[data-i18n]').forEach(el => {
        const key = el.getAttribute('data-i18n');
        el.textContent = translations[lang][key];
    });
}
```

En HTML:

```html
<h1 class="hero-title" data-i18n="hero_title">
    ¿Pierdes reservas...?
</h1>

<!-- Selector de idioma -->
<select onchange="setLanguage(this.value)">
    <option value="es">🇪🇸 Español</option>
    <option value="en">🇬🇧 English</option>
</select>
```

---

## 🎯 Personalización Avanzada

### Cambiar Animaciones

Modificar velocidad en `styles.css`:

```css
:root {
    --transition-base: all 0.3s ease;  /* Cambiar duración */
    --transition-fast: all 0.15s ease;
}
```

### Agregar Nueva Sección

1. Copiar estructura de sección existente
2. Modificar contenido
3. Agregar link en navegación
4. Agregar estilos personalizados

Ejemplo - Sección de Blog:

```html
<!-- En index.html, después de FAQ -->
<section class="blog" id="blog">
    <div class="container">
        <div class="section-header">
            <h2 class="section-title">Blog</h2>
        </div>
        <div class="blog-grid">
            <!-- Artículos aquí -->
        </div>
    </div>
</section>
```

```css
/* En styles.css */
.blog {
    padding: var(--section-padding);
    background: var(--bg-secondary);
}

.blog-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
}
```

### Modificar Breakpoints Responsive

En `styles.css`, buscar media queries (línea ~1800+):

```css
/* Cambiar de 768px a 900px para tablet */
@media (max-width: 900px) {
    /* estilos */
}
```

---

## 🎨 Temas Predefinidos

### Tema Oscuro (Dark Mode)

Agregar toggle en navbar:

```html
<button id="themeToggle" class="theme-toggle">
    <i class="fas fa-moon"></i>
</button>
```

En `script.js`:

```javascript
const themeToggle = document.getElementById('themeToggle');
const html = document.documentElement;

themeToggle.addEventListener('click', () => {
    html.classList.toggle('dark-theme');
    localStorage.setItem('theme', 
        html.classList.contains('dark-theme') ? 'dark' : 'light'
    );
});

// Cargar tema guardado
if (localStorage.getItem('theme') === 'dark') {
    html.classList.add('dark-theme');
}
```

En `styles.css`:

```css
.dark-theme {
    --text-primary: #f9fafb;
    --text-secondary: #d1d5db;
    --bg-primary: #111827;
    --bg-secondary: #1f2937;
    --bg-tertiary: #374151;
}
```

---

## 📱 Personalizar Mobile

### Cambiar Comportamiento del Menú

En `script.js`, modificar hamburger behavior:

```javascript
// Cambiar de slide a fade
navMenu.style.transition = 'opacity 0.3s';
navMenu.style.opacity = navMenu.classList.contains('active') ? '1' : '0';
```

### Ajustar Tamaños para Mobile

```css
@media (max-width: 768px) {
    html {
        font-size: 14px; /* Reducir tamaño base */
    }
    
    .hero-title {
        font-size: 2rem; /* Títulos más pequeños */
    }
}
```

---

## 🔧 Herramientas Útiles

### Generadores de Color
- [Coolors.co](https://coolors.co/) - Paletas de colores
- [ColorHunt](https://colorhunt.co/) - Inspiración
- [Adobe Color](https://color.adobe.com/) - Armonías

### Generadores de Gradientes
- [CSS Gradient](https://cssgradient.io/)
- [Gradient Hunt](https://gradienthunt.com/)

### Iconos
- [Font Awesome](https://fontawesome.com/) - Ya incluido
- [Heroicons](https://heroicons.com/)
- [Feather Icons](https://feathericons.com/)

### Fuentes
- [Google Fonts](https://fonts.google.com/)
- [Font Squirrel](https://www.fontsquirrel.com/)

### Imágenes
- [Unsplash](https://unsplash.com/) - Fotos gratis
- [Pexels](https://www.pexels.com/)
- [Undraw](https://undraw.co/) - Ilustraciones

---

## 💡 Consejos Pro

1. **Mantén la consistencia**: Usa las variables CSS para colores
2. **Optimiza imágenes**: Usa formatos modernos (WebP)
3. **Prueba en real devices**: No solo en dev tools
4. **Mantén accesibilidad**: Contraste de colores adecuado
5. **Documenta cambios**: Anota qué modificaste

---

¿Necesitas ayuda con algo específico? ¡Consulta la documentación o contacta al equipo! 🚀
