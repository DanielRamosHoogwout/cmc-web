# cmc-web

Web para Centro Melani Costa (fisioterapia, rehabilitación y entrenamiento).

## Estructura

- `index.html` — página principal (una sola página con secciones)
- `css/styles.css` — estilos (paleta corporativa azul marino del logo)
- `js/main.js` — menú móvil, animaciones de scroll
- `assets/img/` — imágenes optimizadas para la web
- `assets/brand/` — material original de marca (logo PNG/EPS, fotos sin comprimir, banners)

## Desarrollo

No necesita build. Para verla en local:

```
python -m http.server 8000
```

y abrir http://localhost:8000

## Datos pendientes de confirmar

- Texto definitivo de la sección «Sobre mí»

## Integraciones

- Widget de reserva de citas de Doctoralia (sección Contacto). El script se carga
  desde `platform.docplanner.com`, por lo que solo se ve con la web servida
  (no abriendo el archivo directamente con `file://` en algunos navegadores).
