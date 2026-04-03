# QA Checklist

Checklist manual sugerida para probar `RolMaster Kit: La Forja del Dungeon Master`.

## Entorno

- Abrir `index.html` directamente en navegador
- Abrir `index.html` con Live Server
- Probar al menos en Chrome o Edge
- Probar una vista movil o ventana angosta

## Flujo principal

- Cambiar entre las 4 pestanas sin errores visuales
- Generar un NPC
- Generar una mazmorra
- Generar una mision
- Confirmar que cada modulo muestra resultado, estado, historial y favoritos

## NPCs

- Generar NPC
- Rehacer nombre, raza, oficio, rasgo, secreto y objetivo
- Fijar resultado y comprobar que no deja regenerar hasta desfijar
- Guardar favorito
- Restaurar favorito
- Restaurar un item del historial
- Exportar TXT
- Copiar resultado
- Exportar retrato SVG

## Mazmorras

- Generar mazmorra
- Rehacer nombre, salas, enemigo, trampa y tesoro
- Verificar mapa procedural visible
- Fijar y desfijar
- Guardar favorito y restaurarlo
- Exportar TXT
- Copiar resultado
- Exportar mapa SVG

## Misiones

- Generar mision
- Rehacer gancho, escenas, giro y recompensa
- Verificar escena procedural visible
- Fijar y desfijar
- Guardar favorito y restaurarlo
- Exportar TXT
- Copiar resultado
- Exportar escena SVG

## Bonus

- Generar encuentro rapido
- Generar rumor
- Generar taberna
- Generar asentamiento
- Generar faccion
- Generar tesoro menor
- Exportar TXT en cada bloque bonus
- Copiar resultado en cada bloque bonus
- Pulsar los recursos simulados y verificar el alert esperado

## Exportaciones generales

- Exportar campana completa en TXT
- Exportar ficha de sesion en HTML
- Activar modo impresion
- Exportar los 3 SVG desde la galeria visual
- Guardar campana JSON

## Importacion JSON

- Guardar una campana JSON con contenido generado
- Recargar la pagina
- Importar el JSON guardado
- Verificar restauracion de:
  - nombre de campana
  - pestana activa
  - resultados principales
  - visuales
  - historial
  - favoritos
  - bonus generados

## Persistencia local

- Guardar favoritos en NPCs, mazmorras y misiones
- Cambiar el nombre de campana
- Recargar la pagina
- Confirmar que favoritos y nombre de campana siguen presentes

## Responsive

- Revisar vista cerca de 800px
- Revisar vista cerca de 520px
- Confirmar que no haya botones cortados
- Confirmar que el contenido no desborde horizontalmente

## Consola

- Abrir la consola del navegador
- Confirmar que no haya `SyntaxError`
- Confirmar que los botones no disparen `ReferenceError`

## Criterio de cierre

Si todo lo anterior funciona sin romper layout, sin errores de consola relevantes y sin perder datos clave, el proyecto puede considerarse listo para una primera release manual.
