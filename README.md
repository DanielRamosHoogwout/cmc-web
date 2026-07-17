# cmc-web

Web del **Centro Melani Costa** (fisioterapia, rehabilitación y entrenamiento en Palma).
Sitio estático sin build: HTML + CSS + JS vanilla, pensado para servirse desde GitHub Pages.

## Estructura

```
index.html        Página principal en español (una sola página con secciones)
en/index.html     Versión en inglés
de/index.html     Versión en alemán
css/styles.css    Estilos (compartidos por los tres idiomas)
js/main.js        Menú móvil, sombra del header, animaciones de scroll, año del footer
assets/img/       Imágenes optimizadas para la web (las que usa la página)
assets/brand/     Material original: logo PNG/EPS, fotos sin comprimir, banners, infografías
```

### Idiomas

- Español en la raíz (`/`), inglés en `/en/` y alemán en `/de/`, con selector
  ES · EN · DE en el menú y etiquetas `hreflang` en el `<head>` de las tres
  páginas para que Google muestre a cada usuario su idioma.
- **Al cambiar contenido de `index.html`, replicar el cambio en `en/index.html`
  y `de/index.html`** (mismo HTML, textos traducidos). El CSS y el JS son
  compartidos, esos solo se tocan una vez.
- Las páginas de idiomas usan rutas relativas (`../assets/...`), así que en
  local con `file://` el selector de idioma puede abrir el listado de la
  carpeta en vez del `index.html`; servido (GitHub Pages o `python -m
  http.server`) funciona bien.

### Secciones de `index.html`

1. **Hero** — fondo azul marino con ondas (SVG inline) imitando el branding, logo en blanco y lema "Cada cuerpo tiene un potencial. Descúbrelo."
2. **Bienvenida** — texto introductorio + 3 puntos fuertes.
3. **Servicios** — 3 tarjetas: fisioterapia deportiva, entrenamiento personalizado y rehabilitación.
4. **Técnicas y aparatología** — foto de la consulta + 8 tratamientos: punción seca, electropunción, neuromodulación, magnetoterapia, presoterapia, diatermia/Winback, ondas de choque y ecografía.
5. **Frase** — franja azul con "No es solo ejercicio. Es evolución."
6. **El centro** — galería del gimnasio.
7. **Sobre mí** — presentación de Melani.
8. **Contacto** — horario, widget de reservas de Doctoralia, tarjetas de contacto y botón de WhatsApp para dudas.

## Decisiones importantes

### Reserva de citas: Doctoralia primero, WhatsApp para dudas

- **Doctoralia es el canal principal de reserva.** El widget está en la sección de
  contacto, justo debajo del horario, y todos los botones de "Pide/Reserva tu cita"
  (menú, hero y tarjetas de servicios) llevan a esa sección (`#contacto`).
- **WhatsApp es el canal secundario**, para dudas y preguntas (botón "¿Dudas?
  Escríbenos por WhatsApp"), aunque por ahí también se puede reservar.

### Widget de Doctoralia

- El script se carga desde `platform.docplanner.com` con una URL relativa al
  protocolo (`//...`), por lo que **no funciona abriendo `index.html` con doble
  clic** (`file://`): en local solo se ve el enlace de respaldo. En la web
  publicada (`https://`) se sustituye por el panel de reservas grande.
- Si algún día no se muestra en producción, comprobar en Doctoralia que el
  dominio de la web está autorizado para el widget.

### Caché: parámetro `?v=` en CSS y JS

`index.html` enlaza `css/styles.css?v=N` y `js/main.js?v=N`. GitHub Pages sirve
todo con caché de 10 minutos, así que sin esto un visitante puede recibir el HTML
nuevo con el CSS viejo (pasó: iconos SVG gigantes y fotos sin maquetar).

> **Al modificar `styles.css` o `main.js`, subir el número `?v=` en `index.html`.**

### Imágenes

- Las fotos originales (hasta ~2 MB) viven en `assets/brand/` y **no** se enlazan
  desde la web. Las versiones optimizadas (≤ ~300 KB, máx. 1600 px) están en
  `assets/img/`.
- Para optimizar una foto nueva se puede usar PowerShell (System.Drawing,
  redimensionar + JPEG calidad ~80) o cualquier herramienta similar; mantener
  nombres en **minúsculas y sin espacios** (GitHub Pages distingue mayúsculas,
  Windows no: una ruta con mayúsculas mal puestas funciona en local y da 404
  publicada).
- El logo blanco del hero y el footer es el mismo `logo.png` azul con filtro CSS
  `brightness(0) invert(1)`; no hace falta un segundo archivo.

### Paleta y tipografías

- Colores tomados del logo: azul marino `#1d3e6f` (variables CSS en `:root` de
  `styles.css`), azules claros de las ondas para acentos.
- Tipografías de Google Fonts: Poppins (títulos) e Inter (texto).

## Datos del centro (usados en la web)

- **Dirección:** Calle Camamil·la 6, Local 7-8, 07011 Palma
- **Horario:** lunes a viernes, 9:00–14:00 y 15:30–20:30
- **Teléfono fijo:** 971 659 902
- **Móvil / WhatsApp:** 652 220 988 (enlaces `wa.me/34652220988`)
- **Instagram:** [@centromelanicosta](https://www.instagram.com/centromelanicosta)
- **Doctoralia:** [perfil de Melani Costa Schmid](https://www.doctoralia.es/melani-costa-schmid/fisioterapeuta/palma-de-mallorca)
- **Dominio previsto:** www.centromelanicosta.com

Estos datos están también en el bloque JSON-LD (`schema.org/Physiotherapy`) del
`<head>` de `index.html`, que ayuda a Google a mostrar ficha, horario y dirección
en los resultados. **Si cambia algún dato, actualizarlo en los dos sitios:**
en la sección de contacto y en el JSON-LD.

## Desarrollo en local

No necesita build. Se puede abrir `index.html` directamente (todo funciona menos
el widget de Doctoralia) o servirlo:

```
python -m http.server 8000
```

y abrir http://localhost:8000

## Publicación

GitHub Pages sirviendo la rama `main` desde la raíz. Al hacer push, el deploy
tarda 1-2 minutos; si un cambio no se ve, probar con recarga forzada
(`Ctrl+F5`) o ventana privada antes de buscar otro problema — casi siempre es caché.

## Pendiente

- Texto definitivo de la sección «Sobre mí» (que lo revise Melani).
- Cuando se configure el dominio propio: crear archivo `CNAME` y configurar DNS.
