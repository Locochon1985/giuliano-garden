---
titulo: pi-learn
type: project
status: En curso
alcance:
  - IA
  - Bóveda
  - Aprendizaje
stack:
  - Pi Agent v1.5.2
  - TypeScript
  - Obsidian
  - FSRS-6
  - Mermaid
  - SVG
creada: 2026-11-13
retocada: 2026-11-13
tags:
  - Proyecto/Sistemas
  - IA
temas:
  - "[[Aprendizaje]]"
  - "[[Conocimiento]]"
---

# ⚙️ pi-learn

> [!abstract] **Objetivo del Sistema**
> Documentar y articular la arquitectura completa del entorno local de aprendizaje construido sobre **Pi Coding Agent (`v1.5.2`)**, integrando grafo de conocimiento, enseñanza basada en dependencias, práctica intercalada, visualización verificada, repetición espaciada con FSRS-6 y espejado en tiempo real hacia Obsidian.

Para el flujo operativo paso a paso de cómo estudiar con esta configuración, ver [[how to learn with pi]].

---

## 1. Arquitectura General del Entorno

El sistema combina extensiones TypeScript locales en `~/.pi/agent/extensions/`, paquetes npm declarados en `~/.pi/agent/settings.json`, skills especializadas, subagentes de apoyo, plantillas de prompt y almacenamiento persistente dentro de esta misma bóveda.

---

## 2. Extensiones Locales (`/home/giuliano/.pi/agent/extensions/`)

### 2.1. `learnharness/` (13 tools + `/flashcards`)
Ubicada en `/home/giuliano/.pi/agent/extensions/learnharness/`, constituye el núcleo pedagógico y de persistencia:

* **`index.ts`**: Punto de entrada que registra los cuatro módulos de herramientas (`registerGraphTools`, `registerRecallAndPlanTools`, `registerPracticeTools`, `registerFlashcardTools`).
* **`storage.ts`**: Gestiona la resolución de rutas y lectura/escritura atómica en `DEFAULT_STORAGE_DIR` (`/home/giuliano/Documents/Vault/03 Recursos/LearnHarness`).
* **`graph.ts` (4 tools)**:
  * `graph_record` — Persiste proposiciones confirmadas y cadenas de razonamiento (`premiseIds` → `conclusionId`).
  * `graph_list_sessions` — Lista sesiones de aprendizaje registradas.
  * `graph_list_propositions` — Consulta proposiciones y dependencias existentes en el Knowledge Graph.
  * `graph_import_note` — Importa notas Markdown / de la bóveda al grafo de conocimiento.
* **`recall-plan.ts` (4 tools)**:
  * `plan_init` — Inicializa el DAG Mermaid de la sesión y activa el seguimiento en la barra de estado de Pi.
  * `plan_update` — Actualiza el estado de cada nodo (`upcoming`, `current`, `visited`).
  * `recall` — Evalúa recuerdo libre abierto con `expectedAnswer` oculto.
  * `explain` — Evalúa reconstrucción conceptual en palabras propias ligada a un `nodeId` y `expectedAnswer` oculto.
* **`practice.ts` (2 tools)**:
  * `problem` — Plantea un problema con `solution` y `hints` progresivos ocultos, bajo `PracticeGate` (un único problema activo a la vez).
  * `conclude` — Cierra el problema activo (`solved`, `solvedWithHints`, `movedOn`, `surrendered`, `dismissed`), revela la solución y libera `PracticeGate`.
* **`flashcards.ts` (3 tools + comando `/flashcards`)**:
  * Implementa repetición espaciada con el algoritmo **FSRS-6**.
  * Tools: `flashcard_create`, `flashcard_review`, `flashcard_stats`.
  * Comando TUI: `/flashcards` para sesiones interactivas de repaso.

### 2.2. `quiz.ts`
* **Ruta:** `/home/giuliano/.pi/agent/extensions/quiz.ts`
* **Tool:** `quiz` — Preguntas calificadas de selección simple o múltiple, con opción explícita *"I don't know"* para evitar adivinación forzada y soporte para nota de pensamiento/razonamiento del usuario.

### 2.3. `md-log.ts`
* **Ruta:** `/home/giuliano/.pi/agent/extensions/md-log.ts`
* **Comandos:**
  * `/md-log <filepath>` — Vincula una nota Markdown existente en Obsidian y espeja en vivo la sesión (incluyendo `backfill` inicial).
  * `/md-unlog` — Detiene el espejado activo.
* **Garantía pedagógica:** Filtra y evita filtrar respuestas ocultas (`expectedAnswer`, `solution`, `hints`) hacia el archivo espejado antes de que el estudiante las resuelva o concluya.

### 2.4. `visual-tools/` (6 tools para subagentes)
* **Ruta:** `/home/giuliano/.pi/agent/extensions/visual-tools/` (`index.ts`, `tools/mermaid_tools.ts`, `tools/svg_tools.ts`)
* Registra herramientas personalizadas en `interactive-subagents` para que los subagentes visuales generen, editen y rendericen diagramas a PNG (Mermaid vía `@mermaid-js/mermaid-cli` + Chrome; SVG vía `rsvg-convert` / ImageMagick) y los inspeccionen visualmente antes de publicarlos:
  * **Mermaid:** `write_mermaid`, `edit_mermaid`, `render_mermaid`
  * **SVG:** `write_svg`, `edit_svg`, `render_svg`

---

## 3. Paquetes (`/home/giuliano/.pi/agent/settings.json`)

En `/home/giuliano/.pi/agent/settings.json` se encuentran instalados y activos los siguientes paquetes relevantes para estudio, investigación, memoria y flujo de trabajo:

* **`pi-btw`**: Habilita conversaciones laterales con `/btw` para preguntar dudas rápidas sin contaminar ni interrumpir el hilo principal de enseñanza.
* **`pi-web-access`**: Búsqueda web y extracción de contenido externo.
* **`pi-perplexity`**: Consultas de investigación web mediante Perplexity.
* **`gentle-engram`**: Integración nativa con **Engram** para memoria persistente entre sesiones y compactaciones.
* **`gentle-pi`**: Arnés de ingeniería y disciplina de trabajo (ODD, subagentes y flujos estructurados).
* *(Proveedor base adicional: `@estebanforge/pi-antigravity-bridge` para modelos vía ACP).*

---

## 4. Skills de Aprendizaje y Curación

### 4.1. Skills en `/home/giuliano/.pi/agent/skills/`
* **`teach`** (`/home/giuliano/.pi/agent/skills/teach/SKILL.md`): Pedagogía basada en verdades incondicionales y grafos de dependencia (*Motivate → Establish → Connect → Check & Persist*).
* **`practice`** (`/home/giuliano/.pi/agent/skills/practice/SKILL.md`): Sesiones de práctica deliberada e intercalada usando el Knowledge Graph y `PracticeGate`.
* **`visualize`** (`/home/giuliano/.pi/agent/skills/visualize/SKILL.md`): Orquestación de diagramas verificados visualmente mediante `mermaid-maker` y `svg-maker` para embeber en `md-log`.

### 4.2. Skills en `/home/giuliano/.agents/skills/`
* **`concept-visualizer`** (`/home/giuliano/.agents/skills/concept-visualizer/SKILL.md`): Transformación de notas o modelos mentales complejos en *explainers* HTML interactivos autocontenidos.
* **`notebooklm-bridge`** (`/home/giuliano/.agents/skills/notebooklm-bridge/SKILL.md`): Puente entre investigación web, fuentes en Google Gemini Notebook MCP (`gemini-notebook`) y síntesis en la bóveda.
* **`vault-curator`** (`/home/giuliano/.agents/skills/vault-curator/SKILL.md`): Curación, enrutamiento, formato YAML y auditoría de notas en `/home/giuliano/Documents/Vault/`.

---

## 5. Subagentes (`/home/giuliano/.pi/agent/agents/`)

* **`mermaid-maker`** (`mermaid-maker.md`): Autoría, renderizado e inspección visual de diagramas estructurales/relacionales en Mermaid.
* **`svg-maker`** (`svg-maker.md`): Autoría, renderizado e inspección visual de gráficos espaciales o geométricos en SVG.
* **`vault-agent`** (`vault-agent.md`): Operaciones especializadas sobre la bóveda de Obsidian.
* **`researcher`** (`researcher.md`): Mapeo previo de un dominio o bibliografía antes de planificar una lección.

---

## 6. Prompts (`/home/giuliano/.pi/agent/prompts/`)

* **`practice.md`** (`/practice`): Plantilla de prompt que carga la skill `practice`, consulta `flashcard_stats` y `graph_list_propositions` / `graph_list_sessions`, e inicia una sesión de práctica intercalada (`problem` → coaching → `conclude`).

---

## 7. Almacenamiento en la Bóveda

* **Directorio:** `/home/giuliano/Documents/Vault/03 Recursos/LearnHarness/` (creado automáticamente por `storage.ts` al persistir datos):
  * `graph.json` — Estado estructurado del Knowledge Graph (sesiones, proposiciones y aristas).
  * `Knowledge Graph.md` — Vista legible en Obsidian del grafo de conocimiento.
  * `flashcards.json` — Base de tarjetas y estado FSRS-6.
  * `Flashcards.md` — Espejo Markdown de las tarjetas en la bóveda.

---

## Referencias y Documentación Vinculada
* [[how to learn with pi]]
* [[Configuración y Ecosistema de Pi Agent]]
* [[Guía de Uso y Adaptabilidad de Pi Agent]]
