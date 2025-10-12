# 🚀 Guía de Despliegue - AIBA Website

Esta guía te ayudará a publicar tu sitio web AIBA en diferentes plataformas.

## 📋 Pre-requisitos

Antes de desplegar, asegúrate de:
- ✅ Probar el sitio localmente abriendo `index.html` en tu navegador
- ✅ Verificar que todos los links funcionen
- ✅ Probar en diferentes dispositivos y navegadores
- ✅ Configurar tus IDs de analytics si los usas

## 🌐 Opciones de Despliegue

### 1. Netlify (Recomendado) ⭐

**Ventajas:** Gratis, CDN global, HTTPS automático, deploys continuos

**Método A: Drag & Drop**
1. Ve a [netlify.com](https://www.netlify.com/)
2. Crea una cuenta gratis
3. Arrastra la carpeta del proyecto al área de "Drop"
4. ¡Listo! Tu sitio estará en línea en segundos

**Método B: Git Deploy**
```bash
# 1. Sube tu código a GitHub (ver sección GitHub)
# 2. En Netlify: New site from Git
# 3. Selecciona tu repositorio
# 4. Deploy settings:
#    - Build command: (dejar vacío)
#    - Publish directory: /
# 5. Click "Deploy site"
```

**Configuración de Dominio Personalizado:**
```
1. En Netlify: Site settings > Domain management
2. Add custom domain: aibalabs.ai
3. Configurar DNS según instrucciones
```

---

### 2. Vercel

**Ventajas:** Gratis, muy rápido, excelente DX

```bash
# Instalar Vercel CLI
npm i -g vercel

# En la carpeta del proyecto
vercel

# Seguir las instrucciones interactivas
# Para producción:
vercel --prod
```

**O usar la interfaz web:**
1. Ve a [vercel.com](https://vercel.com/)
2. Importa tu repositorio de GitHub
3. Deploy automático

---

### 3. GitHub Pages

**Ventajas:** Gratis, integrado con GitHub

```bash
# 1. Crear repositorio en GitHub
# 2. Subir código:
git init
git add .
git commit -m "Initial commit: AIBA website"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/aiba-website.git
git push -u origin main

# 3. Activar GitHub Pages:
# Settings > Pages > Source: main branch > Save

# Tu sitio estará en: https://TU_USUARIO.github.io/aiba-website/
```

**Dominio Personalizado:**
1. Crear archivo `CNAME` con tu dominio: `aibalabs.ai`
2. Configurar DNS en tu proveedor:
   ```
   Type: CNAME
   Name: www
   Value: TU_USUARIO.github.io
   ```

---

### 4. Cloudflare Pages

**Ventajas:** Gratis, CDN de Cloudflare, muy rápido

1. Ve a [pages.cloudflare.com](https://pages.cloudflare.com/)
2. Conecta tu repositorio de GitHub
3. Build settings:
   - Build command: (vacío)
   - Build output directory: `/`
4. Deploy

---

### 5. Firebase Hosting

**Ventajas:** Gratis, de Google, rápido

```bash
# Instalar Firebase CLI
npm install -g firebase-tools

# Login
firebase login

# Inicializar proyecto
firebase init hosting

# Seleccionar:
# - Public directory: .
# - Configure as single-page app: No
# - Setup automatic builds: No

# Desplegar
firebase deploy
```

---

### 6. Servidor Propio (VPS)

**Para servidores con Nginx/Apache:**

**Nginx:**
```nginx
server {
    listen 80;
    server_name aibalabs.ai www.aibalabs.ai;
    root /var/www/aiba;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    # Habilitar compresión
    gzip on;
    gzip_types text/css application/javascript image/svg+xml;
}
```

**Apache (.htaccess):**
```apache
# Force HTTPS
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

# Compresión
<IfModule mod_deflate.c>
    AddOutputFilterByType DEFLATE text/html text/css application/javascript
</IfModule>

# Caché
<IfModule mod_expires.c>
    ExpiresActive On
    ExpiresByType text/css "access plus 1 year"
    ExpiresByType application/javascript "access plus 1 year"
    ExpiresByType image/svg+xml "access plus 1 year"
</IfModule>
```

---

## 🔧 Configuración DNS

Para dominio `aibalabs.ai`:

### En Cloudflare / Proveedor DNS:

```
# Para Netlify/Vercel
Type: CNAME
Name: www
Value: [TU-SITIO].netlify.app (o vercel.app)
Proxy: Activado

Type: A
Name: @
Value: [IP del servicio]
Proxy: Activado
```

### Verificación:
```bash
# Verificar DNS
nslookup aibalabs.ai
dig aibalabs.ai

# Verificar HTTPS
curl -I https://aibalabs.ai
```

---

## 📊 Analytics y Tracking

### Google Analytics 4

1. Crear propiedad en [analytics.google.com](https://analytics.google.com)
2. Obtener ID (ej: G-XXXXXXXXXX)
3. Agregar antes de `</head>` en `index.html`:

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

### Facebook Pixel

```html
<!-- Facebook Pixel -->
<script>
  !function(f,b,e,v,n,t,s)
  {if(f.fbq)return;n=f.fbq=function(){n.callMethod?
  n.callMethod.apply(n,arguments):n.queue.push(arguments)};
  if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
  n.queue=[];t=b.createElement(e);t.async=!0;
  t.src=v;s=b.getElementsByTagName(e)[0];
  s.parentNode.insertBefore(t,s)}(window, document,'script',
  'https://connect.facebook.net/en_US/fbevents.js');
  fbq('init', 'YOUR_PIXEL_ID');
  fbq('track', 'PageView');
</script>
```

---

## ⚡ Optimización para Producción

### 1. Minificar CSS y JS

```bash
# Instalar herramientas
npm install -g clean-css-cli uglify-js html-minifier

# Minificar CSS
cleancss -o styles.min.css styles.css

# Minificar JS
uglifyjs script.js -c -m -o script.min.js

# Minificar HTML
html-minifier --collapse-whitespace --remove-comments index.html -o index.min.html

# Actualizar referencias en HTML:
# <link rel="stylesheet" href="styles.min.css">
# <script src="script.min.js"></script>
```

### 2. Optimizar Imágenes

Si agregas imágenes:
```bash
# Instalar imagemin
npm install -g imagemin-cli

# Optimizar
imagemin images/* --out-dir=images/optimized
```

### 3. Generar Sitemap

Crear `sitemap.xml`:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://aibalabs.ai/</loc>
    <lastmod>2025-10-12</lastmod>
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://aibalabs.ai/#precios</loc>
    <changefreq>monthly</changefreq>
    <priority>0.8</priority>
  </url>
  <!-- Agregar más URLs si tienes más páginas -->
</urlset>
```

---

## 🔒 Seguridad

### Headers de Seguridad (en Netlify)

Crear `netlify.toml`:
```toml
[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-XSS-Protection = "1; mode=block"
    X-Content-Type-Options = "nosniff"
    Referrer-Policy = "strict-origin-when-cross-origin"
    Permissions-Policy = "geolocation=(), microphone=(), camera=()"
```

### Certificado SSL

- **Netlify/Vercel/Cloudflare**: Automático ✅
- **Servidor propio**: Usar [Let's Encrypt](https://letsencrypt.org/)

```bash
# En servidor con certbot
sudo certbot --nginx -d aibalabs.ai -d www.aibalabs.ai
```

---

## 📈 Performance

### Lighthouse Score Target

- Performance: > 90
- Accessibility: > 95
- Best Practices: > 90
- SEO: > 90

### Herramientas de Testing:
- [Google PageSpeed Insights](https://pagespeed.web.dev/)
- [GTmetrix](https://gtmetrix.com/)
- [WebPageTest](https://www.webpagetest.org/)

---

## ✅ Checklist Pre-Despliegue

- [ ] Probar en Chrome, Firefox, Safari, Edge
- [ ] Probar en mobile, tablet, desktop
- [ ] Verificar todos los links
- [ ] Configurar Analytics
- [ ] Configurar dominio personalizado
- [ ] Habilitar HTTPS
- [ ] Verificar formulario de contacto
- [ ] Optimizar imágenes (si las hay)
- [ ] Crear sitemap.xml
- [ ] Configurar robots.txt
- [ ] Verificar meta tags y Open Graph
- [ ] Probar velocidad (PageSpeed > 90)
- [ ] Configurar backup automático

---

## 🆘 Troubleshooting

### El sitio no carga
```bash
# Verificar archivos
ls -la

# Verificar que index.html esté en la raíz
# Verificar permisos
chmod 644 index.html
```

### CSS/JS no funciona
- Verificar rutas de archivos (relativas, no absolutas)
- Limpiar caché del navegador (Ctrl+Shift+R)
- Verificar en consola de desarrollador (F12)

### Formulario no funciona
- Implementar backend (ver sugerencias en README.md)
- O usar servicios como Formspree, Basin, Getform

---

## 📞 Soporte Adicional

**Documentación:**
- [Netlify Docs](https://docs.netlify.com/)
- [Vercel Docs](https://vercel.com/docs)
- [GitHub Pages Docs](https://docs.github.com/pages)

**Comunidad:**
- Stack Overflow
- Discord de cada plataforma
- Reddit r/webdev

---

¡Tu sitio AIBA estará en línea en minutos! 🚀
