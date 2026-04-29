# RolMaster Kit v0.2 — Diseño

**Fecha:** 2026-04-29  
**Sistema de referencia:** Vieja Escuela (VE) — Javier "cabohicks" García, grapas&mapas  
**Archivo objetivo:** `index.html` (archivo único, sin build, sin backend)

---

## Resumen

Evolución del kit de herramientas para Dungeon Masters de v0.1 a v0.2, incorporando las mecánicas del sistema Vieja Escuela (retroclón minimalista basado en el juego de rol más famoso de todos los tiempos).

Se agregan 6 mejoras que convierten el kit en una herramienta completa para jugar Vieja Escuela en mesa, sin necesidad de tener el PDF a mano.

---

## Estructura de pestañas

| Pestaña | Estado | Descripción |
|---|---|---|
| 👤 NPCs | Mejorado | Usa razas/clases VE + stats reales |
| 🏚 Mazmorras | Sin cambios | Funciona bien, se mantiene |
| 📜 Misiones | Sin cambios | Funciona bien, se mantiene |
| ⚔️ Mesa VE | Nuevo | 4 secciones: PJ, Dados, Combate, Bestiario |
| 💎 Bonus | Ampliado | Se agregan Equipo VE y Objetos Mágicos |

---

## Mejora 1 — NPCs con stats Vieja Escuela

### Qué cambia
Los NPCs ya generan nombre, raza, clase, personalidad, secreto y objetivo. Se mantiene todo eso y se agregan stats reales del sistema VE.

### Datos nuevos por NPC

**Raza** (reemplaza lista genérica):
- Elfo — talentos: Vista Aguda, Infravisión. MOV 12.
- Enano — talentos: Afín a la Piedra, Infravisión. MOV 9.
- Mediano — talentos: Escurridizo, Certero. MOV 9.
- Humano — talentos: Ímpetu Emprendedor, Adaptable. MOV 12.

**Clase** (reemplaza lista genérica):
- Guerrero — DA: d8. Talentos: Lucha con X, Ataques Múltiples.
- Hechicero — DA: d4. Talentos: Sensibilidad Mágica, Transferir Esencia.
- Bribón — DA: d6. Talentos: Emboscar, Dedos Ágiles.

**Atributos generados** (3d6 cada uno):
- FUE, DES, CON, INT, SAB, CAR
- Modificador: 3=−2, 4-6=−1, 7-14=0, 15-17=+1, 18=+2

**Stats derivados:**
- Nivel: 1d6 (NPCs comunes) o elegible 1–14
- PV: DA máximo clase + mod CON
- DEF: 10 + mod DES + bono armadura aleatoria (sin armadura, cuero, anillos, mallas)
- ATQ: según tabla VE para nivel y clase
- INS: según tabla VE para nivel y clase
- POD: solo para Hechiceros, según tabla VE + mod INT
- MOV: según raza

**Tabla de avance VE (referencia interna):**
```
Nivel | Guerrero ATQ | Hechicero ATQ | Bribón ATQ | Guerrero INS | Hechicero INS | Bribón INS
1     | +0           | +0            | +0         | +1           | +0            | +0
2     | +1           | +0            | +0         | +2           | +1            | +1
3     | +1           | +0            | +1         | +3           | +2            | +2
4     | +2           | +1            | +1         | +4           | +3            | +3
5     | +2           | +1            | +2         | +5           | +4            | +4
6     | +3           | +2            | +2         | +6           | +5            | +5
```
(tabla completa hasta nivel 14 se codifica en el JS)

### Reroll parcial
Se mantienen los botones de reroll por campo. Se agrega un botón "Rehacer stats" que regenera solo los atributos.

---

## Mejora 2 — Mesa VE (pestaña nueva)

Cuarta pestaña del kit. Contiene 4 secciones accesibles por sub-pestañas o scroll: Personaje, Dados, Combate, Bestiario.

### Sección 2a — Generador de Personaje Jugador (PJ)

Flujo guiado en 6 pasos. El usuario puede ir hacia adelante y atrás.

**Paso 1 — Atributos**
- Botón "Tirar 3d6 × 7" genera 7 valores.
- El usuario descarta el menor (se resalta el menor automáticamente).
- Los 6 valores restantes se asignan via selectores (dropdown por atributo). Cada valor puede asignarse una sola vez.
- Botón "Asignar automáticamente" ordena de mayor a menor y los asigna en prioridad por clase: Guerrero prioriza FUE/CON/DES; Hechicero prioriza INT/SAB/CON; Bribón prioriza DES/INT/SAB.
- Muestra el modificador de cada atributo en tiempo real.

**Paso 2 — Raza**
- Cuatro botones: Elfo / Enano / Mediano / Humano.
- Al elegir muestra: talentos de raza, MOV.

**Paso 3 — Clase**
- Tres botones: Guerrero / Hechicero / Bribón.
- Al elegir muestra: DA, talentos de clase, restricciones de armadura y armas.

**Paso 4 — Habilidades**
- 6 habilidades: Alerta, Comunicación, Manipulación, Erudición, Subterfugio, Supervivencia.
- 4 puntos a distribuir. Máximo 1 punto por habilidad. Botones +/− por habilidad.
- Contador de puntos restantes visible.

**Paso 5 — Trasfondo**
- Campo de texto libre para escribir el trasfondo (ej: "soldado en las guerras del norte").
- Botón "Trasfondo aleatorio" para inspirarse (usa lista narrativa).

**Paso 6 — Resumen**
- Muestra la ficha completa calculada:
  - PV = DA máximo clase + mod CON
  - DEF = 10 + mod DES (sin armadura por defecto)
  - ATQ = tabla VE nivel 1 clase
  - INS = tabla VE nivel 1 clase
  - POD = tabla VE nivel 1 clase + mod INT (solo Hechicero)
  - MOV = según raza
  - Monedas iniciales: 3d6 × 10 mo (con botón para tirar)
- Botón "Exportar ficha HTML" genera una hoja de personaje imprimible al estilo VE.
- Botón "Copiar resumen TXT".
- Botón "Guardar PJ en campaña" (persiste en localStorage).

### Sección 2b — Tirador de Dados VE

**Dados disponibles:** d4, d6, d8, d10, d12, d20, d100

**Modos:**
1. **Tirada simple:** elegí dado + cantidad (1–6). Muestra cada resultado y el total.
2. **Ventaja:** tira 2d20, muestra ambos, resalta el mayor.
3. **Desventaja:** tira 2d20, muestra ambos, resalta el menor.
4. **Tirada de Instintos:** 1d20 + bono INS (campo numérico) + mod atributo (selector FUE/DES/CON/INT/SAB/CAR). Compara contra dificultad elegida (11/14/17/20). Muestra ÉXITO o FALLO en grande.

**Historial:** últimas 10 tiradas de la sesión, con timestamp. Botón "Limpiar historial".

### Sección 2c — Rastreador de Combate

**Agregar combatiente:**
- Campos: Nombre, DES (para iniciativa), DEF, PV máx.
- Botón "Agregar". Se puede agregar múltiples.
- Tipo: PJ (color diferente) o Monstruo.

**Iniciativa:**
- Botón "Iniciar combate" ordena la lista por DES descendente.
- El combatiente activo se resalta. Botón "Siguiente turno" avanza.
- Contador de asaltos visible.

**En combate:**
- Cada combatiente muestra: nombre, PV actual / PV máx, barra de vida.
- Botones: `−1`, `−5`, campo libre de daño/curación, `+1`, `+1d4` (vendas).
- Si PV llega a 0 → estado "Caído" (visual diferente, contador de asaltos hasta muerte).
- Botón "×" para eliminar combatiente.

**Crítico (20 natural):**
- Botón que genera efecto aleatorio de VE: daño máx +1, pieza de equipo perdida, armadura dañada (−1 DEF resto del combate), próximo ataque con ventaja, blanco pierde próximo ataque, desplazamiento de posición.

**Pifia (1 natural):**
- Aplica estado "Guardia abierta" hasta el inicio del próximo turno (todos los ataques contra él con ventaja).

**Instintos rápido:**
- Botón pequeño por combatiente: abre mini-modal con 1d20 + mod elegido vs dificultad.

**Exportar:** resumen del combate como TXT.

### Sección 2d — Bestiario VE

**Bestiario fijo (14 monstruos del PDF):**

| Monstruo | Niv | ATQ | DEF | PV | Talentos |
|---|---|---|---|---|---|
| Araña Gigante | 1 | +1 | 11 | 1d8 | Veneno mortal (INS CON 11+) |
| Arpía | 3 | +3 | 12 | 3d8 | Hechizar persona con canción |
| Cieno Gris | 3 | +3 | 11 | 3d8 | Secreciones ácidas |
| Cubo Gelatinoso | 4 | +4 | 11 | 4d8 | Inmovilizar y disolver |
| Doppelganger | 4 | +4 | 14 | 4d8 | Copiar apariencia |
| Dragón Negro | 6 | +6 | 17 | 6d8 | Aliento ácido 4d6 (INS DES mitad) |
| Dragón Verde | 7 | +7 | 17 | 7d8 | Aliento gaseoso 3d8 (INS DES mitad) |
| Dragón Blanco | 8 | +8 | 17 | 8d8 | Aliento escarcha 2d12 (INS DES mitad) |
| Dragón Azul | 9 | +9 | 17 | 9d8 | Aliento rayo 4d8 (INS DES mitad) |
| Dragón Rojo | 10 | +10 | 17 | 10d8 | Aliento fuego 6d6 (INS DES mitad) |
| Esqueleto | 1 | +1 | 11 | 1d8 | Inmune a efectos mentales |
| Goblin | 1 | +1 | 12 | 1d8 | — |
| Kobold | 0 | +0 | 13 | 1d4 | — |
| Necrófago | 2 | +2 | 13 | 2d8 | Paralizar 1d6 turnos (INS FUE 11+) |
| Ogro | 4 | +4 | 14 | 4d8 | — |
| Orco | 2 | +2 | 13 | 2d8 | — |
| Rata Gigante | 0 | +0 | 10 | 1d4 | Enfermedad (1 en 10) |
| Trol | 6 | +6 | 15 | 6d8 | Regenerar 3 PV/asalto (salvo fuego/ácido) |
| Vampiro | 7 | +7 | 17 | 7d8 | Absorbe 1d4 CON permanente; muere a la luz solar |
| Zombi | 2 | +2 | 11 | 2d8 | Inmune a efectos mentales |

**Generador de monstruo aleatorio:**
- Selector de nivel (0–10).
- Genera: nombre compuesto aleatorio, PV = Niv × d8 tirado, ATQ = +Niv, INS = +Niv, DEF = 10 + Niv/2.
- 1–2 talentos únicos aleatorios de lista (veneno, regeneración, inmunidad, aliento, invisibilidad, parálisis, etc.).
- Botón "Copiar TXT" y "Guardar en favoritos".

---

## Mejora 3 — Equipo y Objetos Mágicos (en Bonus)

### Tabla de Equipo VE

Sección nueva dentro de Bonus con las tablas del Apéndice T:

**Armas cuerpo a cuerpo:**
Hacha de Batalla (1d8, 5mo), Porra (1d4, 0mo), Daga (1d4, 2mo), Martillo de Guerra (1d4+1, 1mo), Maza Pesada (1d6, 10mo), Lanza (1d6, 1mo), Bastón (1d6, 0mo), Espada Larga (1d8, 15mo), Espada Corta (1d6, 8mo), Espada a dos manos (1d10, 30mo).

**Armas de proyectiles:**
Arco (1d6, 2 disp/asalto, 60m, 15mo), Ballesta ligera (1d4+1, 1 disp, 70m, 12mo), Daga (1d4, 1 disp, 4m, 2mo), Honda (1d4, 1 disp, 20m, 2mp), Lanza (1d6, 1 disp, 8m, 1mo).

**Armaduras:**
Cuero (+2 DEF, 5mo), Anillos (+3, 30mo), Mallas (+4, 75mo), Placas (+6, 100mo, MOV máx 6), Escudo (+1, 15mo).

**Economía:** 1 mo = 10 mp = 100 mc.

### Generador de Objetos Mágicos

Tres tipos:

**Poción:** nombre evocador + efecto (curación Xd6, veneno, resistencia, invisibilidad, etc.) + duración.

**Pergamino:** conjuro de lista VE (detectar magia, curar heridas, dormir, escudo, hechizar persona, luz, bola de fuego, etc.) + nivel mínimo para activar.

**Objeto hechizado:** nombre único, poder principal, dificultad de sintonización (11+/14+/17+), posible sacrificio de atributo, posible palabra de mando. Ejemplos del estilo Talismán de la Mente en Blanco, Espada Danzarina.

---

## Arquitectura técnica

### Principios (sin cambios)
- Archivo único `index.html` — sin dependencias externas nuevas
- JavaScript vanilla en `<script>` al final del archivo
- CSS en `<style>` en el `<head>`
- Sin build, sin backend, funciona con doble clic o Live Server

### Tablas de datos nuevas (arrays/objetos JS)
```
VE_RACES       — 4 razas con talentos y MOV
VE_CLASSES     — 3 clases con DA, restricciones y talentos
VE_TALENTS     — tabla completa de 13 talentos
VE_SKILLS      — 6 habilidades con descripción
VE_LEVEL_TABLE — tabla de avance nivel 1–14 por clase
VE_BESTIARY    — 16 entradas de monstruos
VE_WEAPONS_MEC — armas cuerpo a cuerpo
VE_WEAPONS_PRY — armas de proyectiles
VE_ARMOR       — armaduras
VE_EQUIPMENT   — equipo diverso
VE_SPELLS      — lista de conjuros
VE_ATTR_MOD    — función modificador de atributo
```

### localStorage (nueva clave)
- `rolmaster_pj` — personaje jugador guardado
- `rolmaster_combat` — estado del rastreador de combate
- Las claves existentes no se tocan

### Exportación de ficha PJ
HTML generado en memoria con estilo inline, listo para imprimir. Estructura similar a la hoja de personaje del PDF (nombre, raza, clase, nivel, atributos con modificadores, habilidades, talentos, trasfondos, stats derivados, armas, equipo).

---

## Lo que NO cambia

- Generador de NPCs: campos narrativos (nombre, personalidad, secreto, objetivo, trasfondo)
- Generador de Mazmorras: completo sin modificaciones
- Generador de Misiones: completo sin modificaciones
- Todas las exportaciones existentes (TXT, SVG, HTML, JSON)
- Favoritos y historial de todos los módulos
- Visuales procedurales (retrato NPC, minimapa mazmorra, escena misión)
- Sistema de campaña (importar/exportar JSON)

---

## Criterios de éxito

- [ ] Generar un NPC con stats VE válidos (PV, DEF, ATQ, INS coherentes con nivel y clase)
- [ ] Crear un PJ completo en 6 pasos y exportar su ficha en HTML
- [ ] Tirar dados con ventaja/desventaja y ver historial
- [ ] Agregar 3 combatientes, iniciar combate, aplicar daño, manejar crítico y pifia
- [ ] Ver los 16 monstruos del bestiario y generar uno aleatorio
- [ ] Ver tablas de equipo VE y generar un objeto mágico
- [ ] Todo funciona sin conexión a internet (sin CDN nuevo)
- [ ] El archivo sigue abriendo con doble clic
