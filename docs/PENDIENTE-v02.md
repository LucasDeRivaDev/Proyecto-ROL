# RolMaster Kit v0.2 — Pendiente y contexto de sesión

## Estado actual

**v0.2 implementado y pusheado a GitHub.** Todos los commits están en `main`.
Deploy automático en Vercel debería estar activo si el repo está conectado.

---

## Tareas pendientes

### 🧪 Testing manual en el navegador (PRIORITARIO)

Los reviewers hicieron análisis estático. Falta correr el kit a mano y verificar:

- [ ] **Mesa VE → Personaje**
  - Tirar 3d6 ×7 → aparecen 7 valores, el más bajo tachado
  - Asignar los 6 atributos via selectores → botón "Siguiente →" aparece al completar todos
  - "Asignar automáticamente" distribuye por prioridad de clase
  - Elegir raza → card se resalta
  - Elegir clase → card se resalta
  - Distribuir 4 puntos de habilidad (máx 1 por habilidad)
  - Escribir trasfondo o usar "Trasfondo aleatorio"
  - Paso 6: ver PV, DEF, ATQ, INS, POD, MOV calculados correctamente
  - Exportar ficha HTML → se descarga un `.html` que abre bien en el navegador
  - "Copiar TXT" → texto copiado al portapapeles

- [ ] **Mesa VE → Dados**
  - Seleccionar d4/d6/d8/d10/d12/d20/d% → el botón se resalta
  - Tirar cantidad 3, dado d6 → muestra suma y desglose "(X + Y + Z)"
  - Ventaja 2d20 → muestra los dos dados y el mayor
  - Desventaja 2d20 → muestra los dos dados y el menor
  - Tirada de Instintos: bono 3, mod +1, dificultad 14+ → ÉXITO o FALLO con desglose
  - Historial muestra las últimas tiradas, máximo 10
  - "Limpiar historial" vacía la lista

- [ ] **Mesa VE → Combate**
  - Agregar 3 combatientes con distintos DES (ej: Guerrero DES 14, Bribón DES 16, Goblin DES 8)
  - "Iniciar combate" → se ordenan por DES (Bribón primero)
  - Indicador de turno muestra nombre y asalto
  - Botones −1/−5 aplican daño, barra de HP baja
  - Input ±dmg + OK aplica daño/curación personalizada
  - "+1d4 ⛑" recupera PV (1 a 4)
  - Cuando PV llega a 0 → fila queda como "💀 CAÍDO" y se salta en "Siguiente turno"
  - Si TODOS caen → "Siguiente turno" no hace nada (no crashea)
  - "🎯 Crítico (20)" → muestra efecto aleatorio VE
  - "💥 Pifia (1)" → muestra aviso de guardia abierta

- [ ] **Mesa VE → Bestiario**
  - Las 20 tarjetas de monstruos aparecen al entrar
  - Clic en "Araña Gigante" → toast "Araña Gigante agregado al combate (PV X)"
  - Ir al tab Combate → la Araña está en la lista
  - Generador de monstruo: nivel 5, "Generar" → nombre único, DEF 12, PV tirado, 1-2 talentos
  - "Agregar al combate" lleva al tab Combate con el monstruo generado

- [ ] **Bonus → Equipo VE**
  - Hacer scroll hasta el final de Bonus
  - Las tablas de armas cuerpo a cuerpo, proyectiles y armaduras se ven con todos los datos
  - "Generar objeto" alterna entre poción / pergamino / objeto hechizado
  - "Copiar" copia el texto
  - "Exportar TXT" descarga un archivo

- [ ] **NPCs mejorados**
  - Pestaña NPCs → "Generar" varias veces
  - El campo Nivel debe mostrar valores entre 3 y 13
  - Guerrero nivel 10 debe tener ATQ +7, INS +10
  - Hechicero nivel 7 debe tener POD base 10

### 📦 Opcionales

- [ ] **Tag de versión git**
  ```bash
  git tag v0.2 && git push origin v0.2
  ```

- [ ] **README del repo** — Actualizar con las features del v0.2 para el portfolio

---

## Contexto técnico para próxima sesión

### Arquitectura del proyecto

- **Un solo archivo:** `index.html` (~5500 líneas)
- Sin build tools, sin npm, sin frameworks. HTML + CSS + JS Vanilla puro.
- Dependencias externas solo por CDN: Google Fonts (Cinzel), Font Awesome 6

### Patrones clave del código

**Tabs principales** (npcs/dungeons/quests/mesa/bonus):
- Botones con `data-tab="..."` en el nav
- Secciones con `id="..."` en el body
- CSS: `.tabs { grid-template-columns: repeat(5, ...) }` ← ya está en 5 columnas
- JS: `function activateTab(targetId)` maneja todo

**Sub-tabs de Mesa VE** (pj/dados/combate/bestiario):
- Botones con `data-mesa="..."` en `.mesa-tabs`
- Secciones con `id="mesa-..."` y clase `.mesa-section`
- JS: `function activateMesaTab(targetId)` + listener de click delegado

**Registro global de funciones:**
- TODAS las funciones llamadas desde `onclick="..."` en el HTML deben estar en `Object.assign(window, { ... })` al final del script
- Si agregás una función nueva y no la registrás ahí, el onclick va a tirar `ReferenceError`

**Resultados en secciones Bonus:**
- Usar `setBonusResult(elementId, text)` para mostrar resultados
- NO usar `setResult(section, text)` — esa es solo para npcs/dungeons/quests
- Si el elemento puede quedar vacío, registrar su markup vacío en el objeto `emptyMarkup` (línea ~2677)

**Estado global:**
```javascript
let pjState = { rolledValues, attributes, race, className, skills, trasfondo, monedas }
let combatState = { combatants, currentIndex, round, started }
let selectedDiceSides = 4
let diceHistoryArr = []
let lastGeneratedMonster = null
```

### Datos VE disponibles (constantes)

```javascript
VE_RACES       // Elfo, Enano, Mediano, Humano con MOV y talentos
VE_CLASSES     // Guerrero, Hechicero, Bribón con DA, talentos, armas
VE_SKILLS_LIST // ["Alerta", "Comunicación", "Manipulación", "Erudición", "Subterfugio", "Supervivencia"]
VE_BESTIARY    // 20 monstruos con nivel, DEF, daño, talentos
VE_WEAPONS_MEC // 10 armas cuerpo a cuerpo
VE_WEAPONS_PRY // 5 armas de proyectiles
VE_ARMOR       // 5 armaduras + escudo
VE_SPELLS      // 18 conjuros
VE_POTION_EFFECTS  // 10 efectos de pociones
VE_ITEM_NAMES      // 12 nombres de objetos hechizados
VE_CRITICAL_EFFECTS // 6 efectos de crítico
VE_MONSTER_PREFIXES / NOUNS / TALENTS_POOL  // para generador aleatorio
VE_TRASFONDOS  // 12 trasfondos de personaje
```

### Tabla de niveles VE (getClassTable)

```
Guerrero:  ATQ [0,1,1,2,2,3,4,5,6,7,7,8,9,9]  INS [1..11]
Hechicero: ATQ [0,0,0,1,1,2,2,3,3,3,4,4,4,5]  POD [1,2,4,5,7,8,10,12,14,15,17,19,20,22]
Bribón:    ATQ [0,0,1,1,2,2,3,3,4,5,5,6,7,7]  INS [0..11]
```

### Commits del v0.2 (referencia)

```
4b7800e feat: completar RolMaster Kit v0.2 con mecánicas Vieja Escuela completas
06b5d0a fix: registrar magicItemResult en emptyMarkup
b4fc548 feat: equipo VE y generador de objetos mágicos
ba15582 fix: eliminar funciones duplicadas
721bd7e feat: bestiario VE con 20 monstruos y generador aleatorio
a13181d fix: nextTurn() edge case todos caídos
cfdc8b3 feat: rastreador de combate
a435cae feat: tirador de dados VE con ventaja/desventaja e Instintos
d04bffb feat: exportar ficha de PJ en HTML
6097bee fix: bug isLowest/used en pjRollAttributes
0160ef4 feat: generador de PJ en 6 pasos
33e27b0 feat: constantes VE y navegación sub-tabs
e878a78 feat: pestaña Mesa VE HTML completo
87a99be feat: CSS Mesa VE
192ce9f feat: NPCs nivel 3-13
3a9b318 feat: getClassTable() extendida a 14 niveles
```

---

## Ideas para v0.3 (sin compromisos)

- Contador de XP por combate (VE tiene tabla de XP por nivel de monstruo)
- Tirada de iniciativa automática al "Iniciar combate" (1d6 por grupo)
- Guardado de PJ en localStorage para retomar entre sesiones
- Generador de tesoro completo (Apéndice T del manual)
- Hoja de campaña: notar lugares visitados, NPCs conocidos, misiones activas
