# 🚀 Quick Start - AIBA Website

## ⚡ Inicio Rápido (5 minutos)

### 1️⃣ Ver el Sitio Localmente

```bash
# Opción 1: Abrir directamente
# Doble click en index.html

# Opción 2: Con servidor local
npx serve .
# Abre: http://localhost:3000
```

### 2️⃣ Personalizar Contenido Básico

**Cambiar colores:** Edita `styles.css` línea 10-20
**Cambiar textos:** Edita `index.html` 
**Cambiar precios:** Busca "pricing-price" en `index.html`

### 3️⃣ Conectar Formulario (Más Fácil)

1. Crea cuenta en [formspree.io](https://formspree.io) (gratis)
2. Crea un form, copia tu ID
3. En `script.js` línea ~160, reemplaza:
   ```javascript
   fetch('https://formspree.io/f/TU_FORM_ID', {
   ```

### 4️⃣ Desplegar (2 minutos)

**Netlify (Recomendado):**
1. Ve a [netlify.com](https://netlify.com)
2. Arrastra la carpeta del proyecto
3. ¡Listo! Tu sitio está en vivo

**Más opciones:** Ver `DEPLOY.md`

---

## 📁 Estructura del Proyecto

```
PROYECTO AIBA/
├── 📄 index.html          # Página principal (EMPIEZA AQUÍ)
├── 🎨 styles.css          # Todos los estilos
├── ⚡ script.js           # JavaScript interactivo
│
├── 📚 README.md           # Documentación completa
├── 🎨 CUSTOMIZATION.md    # Guía de personalización
├── 🚀 DEPLOY.md           # Guía de despliegue
├── 🔧 BACKEND.md          # Conectar formulario
├── ✅ LAUNCH-CHECKLIST.md # Lista pre-lanzamiento
│
├── ⚙️ netlify.toml        # Configuración Netlify
├── ⚙️ vercel.json         # Configuración Vercel
├── 📦 package.json        # Scripts útiles (opcional)
├── 🤖 robots.txt          # SEO
└── 🚫 .gitignore          # Git ignore
```

---

## 🎯 ¿Qué Hacer Primero?

### Si eres Developer 👨‍💻
1. ✅ Lee este archivo (QUICK-START.md)
2. ✅ Abre `index.html` en tu navegador
3. ✅ Revisa `README.md` para detalles técnicos
4. ✅ Personaliza en `CUSTOMIZATION.md`
5. ✅ Despliega según `DEPLOY.md`

### Si eres Marketing/Diseño 🎨
1. ✅ Abre `index.html` en navegador
2. ✅ Revisa el contenido y diseño
3. ✅ Lee `CUSTOMIZATION.md` para cambios
4. ✅ Coordina con developer para deploy

### Si eres Project Manager 📊
1. ✅ Revisa todas las secciones en el navegador
2. ✅ Lee `LAUNCH-CHECKLIST.md`
3. ✅ Asigna tareas al equipo
4. ✅ Define timeline de lanzamiento

---

## 🛠️ Tareas Comunes

### ✏️ Cambiar Textos

1. Abre `index.html`
2. Busca el texto que quieres cambiar (Ctrl+F)
3. Edita directamente
4. Guarda y recarga navegador

### 🎨 Cambiar Colores

1. Abre `styles.css`
2. Busca `:root` (primeras líneas)
3. Cambia las variables de color:
   ```css
   --primary-color: #6366f1;  /* Tu color aquí */
   ```
4. Guarda y recarga

### 💰 Cambiar Precios

1. Abre `index.html`
2. Busca "pricing" (Ctrl+F)
3. Edita los valores:
   ```html
   <span class="amount">297</span>
   ```

### 📸 Agregar Logo

1. Crea carpeta `images/`
2. Guarda tu logo como `logo.png`
3. En `index.html`, busca `.logo` y reemplaza:
   ```html
   <div class="logo">
       <img src="images/logo.png" alt="AIBA" width="150">
   </div>
   ```

### 🔗 Conectar Calendly

1. Busca en `index.html`: "Agendar Diagnóstico"
2. Reemplaza `href="#contacto"` por:
   ```html
   href="https://calendly.com/TU_USUARIO/diagnostico"
   ```

---

## 🐛 Solución de Problemas

### ❌ El sitio no se ve bien
- Limpia caché del navegador (Ctrl+Shift+R)
- Verifica que `styles.css` esté en la misma carpeta
- Abre consola (F12) y busca errores

### ❌ El formulario no funciona
- Por defecto, es simulación (console.log)
- Sigue pasos en `BACKEND.md` para conectar backend
- O usa Formspree/Netlify Forms

### ❌ No funciona en mobile
- Verifica que uses viewport meta tag (ya incluido)
- Prueba en navegador real, no solo dev tools
- Revisa CSS responsive (media queries)

### ❌ JavaScript no funciona
- Verifica consola (F12) para errores
- Asegúrate que `script.js` esté antes de `</body>`
- Verifica que Font Awesome esté cargando

---

## 📱 Probar en Dispositivos

### Desktop
- ✅ Chrome (Ctrl+Shift+I para dev tools)
- ✅ Firefox
- ✅ Safari (si tienes Mac)
- ✅ Edge

### Mobile (Simulado)
```
Chrome Dev Tools (F12) > Toggle Device Toolbar (Ctrl+Shift+M)
Probar: iPhone 12, Galaxy S20, iPad
```

### Mobile (Real)
```
1. Despliega a Netlify
2. Escanea QR o envía link a tu teléfono
3. Prueba navegación, formulario, etc.
```

---

## 🚀 Flujo de Trabajo Recomendado

```mermaid
1. Personalizar Contenido
   ↓
2. Probar Localmente
   ↓
3. Conectar Formulario
   ↓
4. Deploy a Staging
   ↓
5. Probar en Staging
   ↓
6. Deploy a Producción
   ↓
7. Configurar Analytics
   ↓
8. Monitorear y Optimizar
```

---

## 🎓 Recursos de Aprendizaje

### Si necesitas aprender...

**HTML/CSS:**
- [MDN Web Docs](https://developer.mozilla.org/)
- [CSS Tricks](https://css-tricks.com/)
- [W3Schools](https://www.w3schools.com/)

**JavaScript:**
- [JavaScript.info](https://javascript.info/)
- [FreeCodeCamp](https://www.freecodecamp.org/)

**Deployment:**
- [Netlify Docs](https://docs.netlify.com/)
- [Vercel Docs](https://vercel.com/docs)

**Git/GitHub:**
- [GitHub Guides](https://guides.github.com/)
- [Git Handbook](https://guides.github.com/introduction/git-handbook/)

---

## 💡 Tips Pro

### Performance
```bash
# Minificar archivos para producción
npm run minify

# Resultado: styles.min.css y script.min.js
# Actualiza referencias en index.html
```

### SEO
```html
<!-- Agrega estas meta tags en <head> -->
<meta name="description" content="Tu descripción única">
<meta name="keywords" content="aiba, automatización, ia">
```

### Analytics
```html
<!-- Antes de </head> -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'TU_GA_ID');
</script>
```

---

## ✅ Checklist Rápido

Antes de lanzar, verifica:

- [ ] ✏️ Contenido revisado (sin typos)
- [ ] 🎨 Colores y diseño OK
- [ ] 💰 Precios correctos
- [ ] 📧 Formulario funcionando
- [ ] 🔗 Todos los links funcionan
- [ ] 📱 Se ve bien en mobile
- [ ] 🚀 Velocidad > 90 (PageSpeed)
- [ ] 📊 Analytics configurado
- [ ] 🔒 HTTPS activado
- [ ] 🌐 Dominio configurado

---

## 🆘 ¿Necesitas Ayuda?

### Documentación Incluida
- **README.md** - Documentación técnica completa
- **CUSTOMIZATION.md** - Cómo personalizar todo
- **DEPLOY.md** - Cómo desplegar
- **BACKEND.md** - Cómo conectar formulario
- **LAUNCH-CHECKLIST.md** - Lista pre-lanzamiento

### Recursos Online
- Stack Overflow (preguntas técnicas)
- GitHub Issues (si encuentras bugs)
- Discord/Slack de Netlify/Vercel (deployment)

### Herramientas Útiles
- **PageSpeed Insights** - Probar velocidad
- **GTmetrix** - Análisis de performance
- **Lighthouse** - En Chrome DevTools (F12)
- **WAVE** - Probar accesibilidad

---

## 🎉 ¡Siguiente Paso!

1. **Ahora mismo:** Abre `index.html` en tu navegador
2. **Siguiente:** Lee `CUSTOMIZATION.md` y personaliza
3. **Después:** Sigue `DEPLOY.md` para publicar
4. **Finalmente:** Usa `LAUNCH-CHECKLIST.md` antes de lanzar

---

## 📞 Contacto

**Sitio Web:** [aibalabs.ai](https://aibalabs.ai)  
**Email:** hello@aibalabs.ai  
**Documentación:** Ver archivos .md en este proyecto

---

**¡Bienvenido a AIBA! Construyamos el futuro juntos. 🚀**

---

*Creado con ❤️ para ayudarte a lanzar rápido y bien.*
