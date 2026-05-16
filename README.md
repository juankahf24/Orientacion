# MILITOPO · Orientación V1

Proyecto separado desde el HTML original para poder subirlo a GitHub/GitHub Pages y mantenerlo de forma más limpia.

## Estructura

```text
.
├── index.html
├── assets/
│   ├── css/
│   │   └── styles.css
│   └── js/
│       ├── qr-offline.js
│       └── app.js
├── ESTUDIO_TECNICO.md
├── MANIFEST.json
└── original/
    └── Base index tabla completa blanco y negro(1).html
```

## Cómo abrirlo en local

Puedes abrir `index.html` directamente, pero para simular mejor GitHub Pages conviene servirlo con un servidor local:

```bash
python3 -m http.server 8000
```

Luego abre:

```text
http://localhost:8000
```

## Cómo subirlo a GitHub

1. Crea un repositorio nuevo en GitHub.
2. Sube **todo el contenido de esta carpeta**, no la carpeta comprimida.
3. El archivo `index.html` debe quedar en la raíz del repositorio.
4. En GitHub, entra en `Settings` → `Pages`.
5. En `Build and deployment`, elige `Deploy from a branch`.
6. Selecciona la rama `main` y la carpeta `/root`.
7. Guarda. GitHub generará una URL tipo `https://usuario.github.io/repositorio/`.

## Notas importantes

- La app usa CDNs externos para Leaflet, proj4, JSZip y FileSaver. Si quieres una versión 100% offline, hay que descargar esas librerías y cambiar las rutas.
- Cámara y geolocalización necesitan HTTPS; GitHub Pages lo proporciona.
- Los datos de autoguardado dependen del dominio. Al mover la app a GitHub Pages, el navegador la tratará como una app nueva.
- Para cambios futuros, empieza por `assets/js/app.js` y `assets/css/styles.css`.

