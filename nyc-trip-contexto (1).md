# NYC Trip Planner — Contexto del Proyecto

## El viaje
- **Grupo:** 8 adultos + 8 ninos de 5 a 15 anos
- **Destino:** Nueva York
- **Cuando:** Semana Santa 2025
- **Fechas exactas:** ___
- **Alojamiento:** ___
- **Presupuesto:** ___

## Restricciones importantes
- Ninos pequenos necesitan descanso de tarde
- Grupo grande: logistica de 16 personas en metro, restaurantes, etc.
- Priorizar actividades aptas para ninos de 5 anos en adelante

---

## Estado actual de la app (nyc-trip-planner.html)

### Funcionalidades implementadas
- **Sitios:** buscar con IA (campo de busqueda, la IA devuelve sitios reales de NYC, seleccionar uno, confirmar), filtrar por zona, votar, eliminar. Detecta duplicados y los marca como "ya existe". Fallback a busqueda local en NYC_DB si la IA no responde.
- **Importar:** pegar texto libre, IA extrae sitios, detecta duplicados, confirmar importacion
- **Itinerario IA:** configura dias / ritmo / prioridad / fecha inicio, genera plan dia a dia
- **Zonas:** vista agrupada por barrio ordenada por votos

### Stack tecnico
- HTML + CSS + JS vanilla, archivo unico .html
- Sin frameworks. Dependencias: Google Fonts + Firebase SDK (compat v10.12.0)
- API: claude-sonnet-4-20250514 para extraccion de sitios e itinerarios
- Storage: Firebase Realtime Database (sync en tiempo real) + localStorage como fallback offline
- Hosting: GitHub Pages (archivo index.html)
- Firebase config embebida en el HTML (proyecto nyc-trip-2025)

### Decisiones tecnicas clave
- JS en ES5 puro (var, function) sin arrow functions ni emojis en JS
- Extraccion de sitios con formato pipe-delimited: NOMBRE|ZONA|CATEGORIA|NOTA
- Deduplicacion por similitud de texto, umbral 0.75
- Navegacion: barra inferior fija en movil (como app nativa)
- safe-area-inset para notch y home bar de iPhone

---

## Proximos pasos

### Prioritarios
- [x] Backend real con base de datos — Firebase Realtime Database integrado, sync en tiempo real
- [x] URL publica compartible — GitHub Pages desplegado
- [ ] Posiblemente PWA instalable

### Backlog
- [ ] Sistema de comentarios por sitio
- [ ] Fotos adjuntas a cada sitio
- [ ] Notificaciones cuando alguien anade un sitio nuevo
- [ ] Exportar itinerario a PDF o calendario

---

## Sitios propuestos por el grupo
*(Actualizar con cada sesion de desarrollo)*

| Nombre | Zona | Categoria | Votos | Notas |
|--------|------|-----------|-------|-------|
| Natural History Museum | Manhattan | Museo | 7 | Dinosaurios y ballena azul |
| Intrepid Museum | Manhattan | Museo | 6 | Portaaviones y transbordador espacial |
| Top of the Rock | Manhattan | Foto | 5 | Mejores vistas, menos cola que Empire State |
| Coney Island | Brooklyn | Actividad | 4 | Playa y parque de atracciones vintage |
| DUMBO + Brooklyn Bridge | Brooklyn | Foto | 4 | Cruzar el puente a pie |
| Central Park | Manhattan | Parque | 3 | Perfecto para ninos |
| Katz Delicatessen | Manhattan | Restaurante | 3 | El pastrami mas famoso de NYC |
| High Line | Manhattan | Parque | 2 | Parque elevado sobre antigua via de tren |

---

## Textos pendientes de importar
*(Pegar aqui bloques de WhatsApp o notas que aun no se hayan procesado)*

(vacio — pegar texto aqui)

---

## Historial de decisiones de diseno
- Tabs abajo en movil (como Instagram/WhatsApp), arriba en desktop
- Color amarillo #F7E733 como color principal
- Fondo negro #0A0A0A
- Fuentes: Bebas Neue (titulos) + DM Sans (texto)
- Sin emojis en el codigo JS por incompatibilidad con Chrome en file://

---

## Notas de desarrollo
*(Anadir aqui problemas encontrados, soluciones, ideas)*

- El error "Invalid response format" en la importacion era por pedir JSON a la IA — solucionado cambiando a formato pipe-delimited
- Los artifacts de React no se ven en la app movil de Claude — usar HTML vanilla
- En iOS Safari los tabs superiores quedan tapados por la barra del navegador — solucionado con barra de navegacion inferior fija
- El formulario manual de "Anadir sitio" fue reemplazado por busqueda con IA: el usuario escribe lo que busca, la IA devuelve sitios reales de NYC, se selecciona uno y se confirma. Formato pipe-delimited NOMBRE|ZONA|CATEGORIA|DIRECCION|DESCRIPCION (5 campos). Fallback local busca en NYC_DB.
- Firebase Realtime Database integrado: save() escribe en Firebase + localStorage, load() se suscribe a cambios en tiempo real via on('value'). Prevencion de eco con timestamp (2s ventana). Si Firebase esta vacio al iniciar, sube los datos seed.
- NYC_DB ampliada a ~80 sitios con campos cat, zone, desc, tags. Diccionario SEARCH_ALIASES para busquedas por categoria en espanol/ingles.
- Indicador de conexion (punto verde/naranja) en header via .info/connected de Firebase.
- Hosting en GitHub Pages: archivo renombrado a index.html, deploy desde rama main.
- Reglas de Firebase en modo de prueba (30 dias). Pendiente: configurar reglas de seguridad basicas.
