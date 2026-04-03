# RolMaster Kit: La Forja del Dungeon Master

Kit offline en una sola pagina HTML para Dungeon Masters de juegos de rol medievales y fantasticos.

El proyecto esta pensado para abrirse directamente en el navegador y ofrecer herramientas rapidas de mesa sin depender de frameworks, backend ni instalacion.

## Estado

Version actual: candidata a testing integral.

El proyecto ya cuenta con:

- flujo jugable principal completo
- exportaciones multiples
- favoritos persistentes
- importacion y exportacion de campanas en JSON
- visuales procedurales offline
- compatibilidad corregida para uso con Live Server

## Caracteristicas principales

### Generadores base

- NPCs con nombre, raza, oficio, personalidad, secreto y objetivo
- Mazmorras con nombre, salas, enemigo, trampa y tesoro
- Misiones con gancho, escenas, giro y recompensa

### Herramientas de mesa

- historial reciente por modulo
- favoritos persistentes con `localStorage`
- fijado de resultados para evitar sobreescrituras
- reroll parcial por campos
- nombre de campana editable y persistente

### Bonus incluidos

- encuentros rapidos
- rumores de taberna
- tabernas
- asentamientos
- facciones
- tesoros menores
- recursos simulados descargables

### Capa visual

- retrato procedural para NPCs
- minimapa procedural para mazmorras
- escena procedural para misiones
- portada procedural de campana
- galeria visual de campana

### Exportaciones

- TXT por generador
- TXT de campana completa
- SVG individuales de retrato, mapa y escena
- exportacion conjunta de visuales
- ficha de sesion en HTML
- modo impresion
- guardado y carga de campana en JSON

## Tecnologias

- HTML
- CSS
- JavaScript
- Google Fonts: `Cinzel` y `MedievalSharp`
- Font Awesome 6

## Uso

1. Descarga o clona el repositorio.
2. Abre [index.html](./index.html) en tu navegador.
3. Tambien podes usar Live Server si preferis recarga automatica.
4. Usa las pestanas para cambiar entre NPCs, mazmorras, misiones y bonus.
5. Genera contenido, fijalo, guardalo en favoritos o exportalo.

No requiere dependencias ni build.

## Archivos clave

- `index.html`: aplicacion completa en un solo archivo
- `README.md`: descripcion general del proyecto
- `QA-CHECKLIST.md`: guia de prueba manual recomendada

## Testing manual sugerido

La checklist recomendada esta en [QA-CHECKLIST.md](./QA-CHECKLIST.md).

Incluye pruebas para:

- generacion de contenido
- copiar y exportar
- importacion y exportacion JSON
- impresion
- responsive
- persistencia local
- uso con Live Server

## Filosofia del proyecto

RolMaster Kit busca ser:

- rapido de abrir
- facil de compartir
- util en mesa
- visualmente tematico
- mantenible como archivo unico

## Roadmap posible

- exportacion PNG de portada y visuales
- semillas reproducibles para resultados
- ajustes finos de UX y accesibilidad
- release etiquetada `v0.1`
