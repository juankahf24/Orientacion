# Estudio técnico de MILITOPO · Orientación V1

## Resumen ejecutivo

La aplicación original es una **SPA** (Single Page Application) en un único HTML. La he separado sin modificar la lógica en:

- `index.html`: estructura y carga ordenada de dependencias.
- `assets/css/styles.css`: todos los estilos originales, incluidos los añadidos de paneles, marcadores, popups y progreso ZIP.
- `assets/js/qr-offline.js`: generador QR offline que antes estaba embebido en el `<head>`.
- `assets/js/app.js`: lógica principal de la aplicación.
- `original/`: copia intacta del HTML recibido.
- `MANIFEST.json`: métricas técnicas y control de la separación.

No he convertido el JavaScript a módulos ni he cambiado nombres de funciones, porque la app usa muchos manejadores inline tipo `onclick="..."`. Cambiarlo a módulos ahora podría romper llamadas globales desde el HTML.

## Arquitectura detectada

### Dependencias externas

La app carga por CDN:

- `https://unpkg.com/leaflet@1.9.4/dist/leaflet.css`
- `https://unpkg.com/leaflet@1.9.4/dist/leaflet.js`
- `https://cdnjs.cloudflare.com/ajax/libs/proj4js/2.9.2/proj4.js`
- `https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js`
- `https://cdnjs.cloudflare.com/ajax/libs/FileSaver.js/2.0.5/FileSaver.min.js`

Además, `app.js` carga bajo demanda `jsPDF` y `html2canvas` cuando se generan PDFs, usando funciones dinámicas de carga de scripts.

### Bloques funcionales principales

1. **Configuración del ejercicio**: nombre, escala, equidistancia, número de participantes, controles y reutilización máxima.
2. **Gestión de puntos de orientación**: salida, llegada y balizas; edición por UTM, mapa, importación CSV/texto, GPX/KML/ATAK y autocompletado de prueba.
3. **Mapa**: Leaflet con capas Mapant, IGN y PNOA; marcadores específicos para salida, control y llegada.
4. **Descripción IOF**: editor de descripciones, símbolos integrados y exportación CSV/HTML.
5. **Generación de recorridos**: cálculo de recorridos equilibrados para participantes, métricas y avisos.
6. **Exportación del ejercicio**: creación de ZIP con recursos, PDFs/HTML, QR, CSV y verificación previa.
7. **Modo participante/resultados**: escaneo QR, registro de controles, importación de resultados y clasificación XLSX/CSV.
8. **Persistencia local**: autoguardado en `localStorage` y restauración de estado.

## Métricas de código

- Tamaño original: 1,211,655 bytes.
- JavaScript principal: 1,114,997 caracteres.
- QR offline: 33,159 caracteres.
- CSS separado: 42,826 caracteres.
- Funciones nombradas detectadas en `app.js`: 272.
- IDs únicos en el DOM principal: 69.
- Manejadores inline detectados: 65.
- Usos de `localStorage`: 15.

## Puntos delicados para futuras modificaciones

1. **No romper el orden de carga**: Leaflet, proj4, JSZip y FileSaver deben cargarse antes de `assets/js/app.js`.
2. **Mantener funciones globales**: los botones usan `onclick`, `onchange` y similares. Si se modulariza, habría que exponer funciones en `window`.
3. **GitHub Pages y HTTPS**: cámara, geolocalización y algunas APIs funcionan correctamente en HTTPS. GitHub Pages proporciona HTTPS.
4. **Dependencia de CDNs**: si el dispositivo no tiene internet, los mapas y librerías CDN no cargarán. El QR offline está embebido localmente, pero Leaflet/proj4/JSZip/FileSaver siguen externos.
5. **Mapas externos y CORS**: exportaciones con mapas o imágenes remotas pueden depender de permisos CORS de los servidores.
6. **`localStorage` por dominio**: al pasar de archivo local a GitHub Pages, el autoguardado será nuevo porque cambia el origen/dominio.
7. **Tamaño de `app.js`**: conviene refactorizar en módulos por zonas: estado, mapa, IOF, rutas, exportaciones, resultados y utilidades.
8. **Posible detalle a revisar en QR**: en el generador QR aparece una asignación `QRCode.PA` donde normalmente existirían constantes de padding `PAD0/PAD1`. No la he tocado para preservar comportamiento, pero es candidata a revisión controlada.

## Plan recomendado para pulir la app

### Fase 1 · Seguridad funcional

- Crear una lista de pruebas manuales: configurar ejercicio, añadir puntos, generar rutas, exportar ZIP, escanear/importar resultados.
- Probar en GitHub Pages con móvil y ordenador.
- Documentar qué datos se guardan en navegador y cómo resetearlos.

### Fase 2 · Refactor sin cambios visuales

- Separar `app.js` en módulos lógicos manteniendo compatibilidad global.
- Sustituir progresivamente manejadores inline por `addEventListener`.
- Centralizar validaciones de UTM, GPX/KML, IOF y rutas.

### Fase 3 · Robustez

- Añadir pruebas automáticas de funciones puras: UTM, importación de puntos, generación de rutas, clasificación.
- Añadir control de errores para CDNs caídos.
- Preparar una versión realmente offline con librerías descargadas dentro de `assets/vendor`.

### Fase 4 · Pulido UX/UI

- Mejorar mensajes de error.
- Añadir confirmaciones antes de borrar/restablecer.
- Crear modo oscuro/claro o temas.
- Reducir peso inicial cargando partes bajo demanda.

## Qué he cambiado

Solo he hecho separación física de archivos y limpieza de estructura de carga. No he alterado la lógica de negocio, ni textos, ni selectores, ni nombres de funciones.

## Comprobaciones realizadas

- Extracción del HTML original mediante parser HTML.
- Conservación de dependencias externas en el mismo orden relativo.
- Separación de CSS, QR offline y JS principal.
- Validación sintáctica con Node.js sobre `qr-offline.js` y `app.js`.
- Generación de un paquete ZIP listo para subir.

