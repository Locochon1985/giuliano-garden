---
titulo: How to Learn with Pi
type:
  - literature
author:
  - Giuliano
source_title: Flujo de trabajo integral de aprendizaje con Pi Agent, LearnHarness, Obsidian y NotebookLM
media_type:
  - especificacion
published: 2026-11-13
creada: 2026-11-13
retocada: 2026-11-13
description: Guía operativa end-to-end del flujo de aprendizaje con Pi Agent (preparación, diagnóstico, sesión activa con teach, apoyo visual, práctica deliberada e intercalada con /practice y retención con FSRS-6 en /flashcards).
tags:
  - IA
status:
  - Procesado
temas:
  - "[[Aprendizaje]]"
  - "[[Conocimiento]]"
valor:
  - 6 - Excelente
---
q
# How to Learn with Pi - Flujo Completo de Aprendizaje

> [!IMPORTANT] Propósito de esta guía
> Este documento describe el **flujo de trabajo completo de extremo a extremo** para estudiar, comprender en profundidad, practicar y retener cualquier tema usando el entorno local de Pi Agent documentado en [[pi-learn]].

---

## Visión General del Ciclo

```text
1. Preparación e Ingesta (/md-log, graph_*, notebooklm-bridge + vault-agent)
        │
        ▼
2. Diagnóstico Inicial y Mapa de Ruta (recall, quiz, plan_init)
        │
        ▼
3. Sesión de Estudio Activa (teach: Motivate → Establish → Connect → Check & Persist + /btw)
        │
        ▼
4. Apoyo Visual y Síntesis (visualize, concept-visualizer, vault-curator)
        │
        ▼
5. Práctica Deliberada e Intercalada (/practice: problem → pistas → conclude)
        │
        ▼
6. Retención a Largo Plazo (/flashcards con FSRS-6)
```

---

## Etapa 1: Preparación e Ingesta de Contexto

Antes de empezar a explicar o resolver problemas, se prepara el espacio de lectura en Obsidian y se recupera lo que ya sabes o tienes en tus fuentes:

1. **Activar el espejo en Obsidian (`/md-log`)**:
   * Crea o abre una nota en tu bóveda (por ejemplo para la sesión del día) y ejecuta `/md-log <ruta-al-archivo.md>`.
   * A partir de ese momento, Pi espeja la conversación en tiempo real en la nota para que leas con renderizado nativo de LaTeX, callouts, Mermaid e imágenes, **sin filtrar respuestas ocultas** de evaluaciones en curso. Al terminar puedes desconectarlo con `/md-unlog`.
1. **Revisar el Knowledge Graph previo**:
   * El agente consulta `graph_list_sessions` y `graph_list_propositions` para ver qué proposiciones y cadenas de razonamiento ya consolidaste en `/home/giuliano/Documents/Vault/03 Recursos/LearnHarness/`.
3. **Importar notas o bibliografía existente**:
   * Si ya tienes apuntes o bibliografía de cátedra en Obsidian, se incorporan al grafo con `graph_import_note`.
   * Si necesitas cruzar fuentes externas, PDFs o cuadernos de estudio, se combina la skill `notebooklm-bridge` (para ingestar y consultar fuentes en Gemini Notebook) con el subagente `vault-agent` (para localizar y leer notas relevantes de la bóveda).

---

## Etapa 2: Diagnóstico Inicial y Mapa de Ruta

El objetivo de esta fase es **ubicar exactamente tu frontera de conocimiento** en cada hilo necesario antes de enseñar nada:

1. **Calibración con `recall` y `quiz`**:
   * `recall` evalúa recuerdo libre abierto (sin pistas de opción múltiple) contra un `expectedAnswer` pre-comprometido.
   * `quiz` realiza sondeos rápidos de selección simple o múltiple (con la opción *"I don't know"* para no forzar adivinación y un campo opcional para tu razonamiento).
   * Se busca acotar piso (qué dominas) y techo (dónde aparece el límite o un modelo mental erróneo).
2. **Definición del objetivo y mapa de dependencias (`plan_init`)**:
   * Se aclara qué quieres lograr exactamente en la sesión.
   * Opcionalmente, el agente puede mapear el campo primero con `researcher` para no omitir principios fundamentales.
   * Con tu visto bueno al plan y al DAG Mermaid propuesto, se llama a `plan_init`, que activa el seguimiento en vivo (`upcoming` / `current` / `visited`) en la barra de estado de Pi.

---

## Etapa 3: Sesión de Estudio Activa (`teach`)

La skill `teach` evita la memorización de hechos aislados y construye un **grafo de dependencias** partiendo de verdades incondicionales hasta llegar a tu objetivo. Para cada nodo del DAG se ejecuta el mismo bucle:

1. **Motivate**: Se marca el nodo en `plan_update` y se enmarca *por qué* hace falta ese concepto ahora mismo y qué problema resuelve.
2. **Establish**: Si es una verdad incondicional base, se enuncia limpia y sin salvedades; si es un paso derivado, se construye desde lo ya aceptado (de forma socrática o expositiva).
3. **Connect**: Se explicita la arista de dependencia con los nodos anteriores para que el nuevo concepto quede sostenido por su estructura.
4. **Check & Persist**:
   * Se verifica que el nodo realmente cerró usando `quiz` (distinciones precisas) o `explain` (reconstrucción en tus propias palabras asociada al `nodeId` y comparada contra `expectedAnswer`).
   * Una vez confirmado, `plan_update` marca el nodo como `visited` y `graph_record` guarda las proposiciones y aristas (`premiseIds` → `conclusionId`) en el Knowledge Graph.

> [!TIP] Dudas laterales sin romper el flujo (`/btw`)
> Si en medio de una explicación surge una pregunta tangencial o de vocabulario, usa `/btw <tu duda>` (`pi-btw`). Te permite abrir una consulta paralela sin desviar ni ensuciar el hilo principal de la lección.

---

## Etapa 4: Apoyo Visual y Síntesis

Cuando una estructura, flujo o geometría se entiende mejor viéndola que leyéndola:

1. **Diagramas verificados inline (`visualize`)**:
   * La skill `visualize` delega la creación a `mermaid-maker` (grafos, secuencias, árboles, flujos) o `svg-maker` (geometría, coordenadas, rectas numéricas, vectores).
   * El subagente escribe, renderiza a PNG con `visual-tools` (`write_*` / `edit_*` / `render_*`), **inspecciona la imagen resultante** para verificar que no haya errores ni solapamientos, y devuelve el archivo para embeberlo como `![[viz-...png|500]]` directamente en el log de `md-log`.
2. **Explainers interactivos (`concept-visualizer`)**:
   * Para modelos mentales complejos que ameritan exploración interactiva, `concept-visualizer` transforma notas Markdown en un archivo HTML autocontenido.
3. **Paso a la bóveda permanente (`vault-curator`)**:
   * Al cerrar un bloque de estudio, `vault-curator` ayuda a destilar las ideas propias y conexiones interdisciplinarias hacia notas atómicas en `01 Notas/` o actualizar referencias y wiki siguiendo el schema de `Vault Map.md`.

---

## Etapa 5: Práctica Deliberada e Intercalada (`/practice`)

Entender una explicación no equivale a saber reconocer qué principio aplicar frente a un problema nuevo. Para entrenar la transferencia se ejecuta `/practice` (prompt `practice.md` + skill `practice`):

1. **Selección intercalada**: El agente consulta `flashcard_stats` y `graph_list_propositions` para elegir problemas que mezclen distintas ramas del grafo en lugar de repetir mecánicamente un solo patrón.
2. **Bucle `problem` → Coaching → `conclude`**:
   * `problem` presenta el enunciado manteniendo ocultos `solution` y `hints` progresivos. Gracias a `PracticeGate`, solo puede haber un problema activo a la vez.
   * Si intentas resolverlo (`attempted`), el agente evalúa tu razonamiento y da feedback socrático; si eliges no saber (`dontKnow`), diagnostica la brecha y reconstruye la derivación desde el grafo.
   * `conclude` cierra el intento (`solved`, `solvedWithHints`, `movedOn`, `surrendered` o `dismissed`), revela la solución pre-comprometida en el log y habilita el siguiente problema.
3. **Retroalimentación al grafo**: Si la práctica revela una nueva conexión o un punto ciego recurrente, se registra con `graph_record` y puede originar tarjetas con `flashcard_create`.

---

## Etapa 6: Retención a Largo Plazo (`/flashcards` con FSRS-6)

Para consolidar distinciones finas, definiciones o intuiciones clave sin saturarte de tarjetas triviales:

1. **Creación selectiva (`flashcard_create`)**: Al finalizar una sesión de estudio o práctica, se crean pocas tarjetas atómicas vinculadas a los `propositionId`s que ya comprendiste.
2. **Consulta de carga (`flashcard_stats`)**: Permite ver cuántas tarjetas hay pendientes por tema o estado.
3. **Repaso espaciado (`flashcard_review` / `/flashcards`)**: Ejecuta la sesión interactiva de repaso en Pi TUI programando los intervalos con el algoritmo **FSRS-6** y persistiendo el historial en `flashcards.json` y `Flashcards.md`.

---

## Notas Relacionadas
* [[pi-learn]]
* [[Configuración y Ecosistema de Pi Agent]]
* [[Guía de Uso y Adaptabilidad de Pi Agent]]
