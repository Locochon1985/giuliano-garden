---
titulo: Pi Hub - Centro de Control y Configuración de Pi Agent
type:
  - recurso
  - sistema
author:
  - Giuliano
creada: 2026-11-12
retocada: 2026-11-14
tags:
  - IA
  - Sistema
status:
  - Procesado
temas:
---

# Pi Hub - Centro de Control y Configuración de Pi Agent

> [!IMPORTANT] Fuente Única de Configuración desde Obsidian
> Este directorio (`03 Recursos/Pi Hub/`) centraliza toda la configuración viva de **Pi Coding Agent**: ajustes globales, subagentes activos, skills locales, prompts del orquestador y servidores MCP. Podés editar cualquier archivo directamente desde Obsidian y sincronizarlo con el motor de Pi mediante `pi-ctl sync` o usando el comando `/config` dentro del propio chat de Pi.

---

## 1. Estructura del Directorio (`03 Recursos/Pi Hub/`)

```text
03 Recursos/Pi Hub/
├── Pi Hub - Panel de Control.md       # Este documento maestro de control y referencia
├── config/                            # Archivos de configuración base de Pi y sus puentes
│   ├── settings.json                  # Ajustes centrales de Pi TUI (~/.pi/agent/settings.json)
│   ├── subagents.json                 # Registro y ruteo de subagentes activos
│   ├── mcp.json                       # Servidores MCP configurados (ej. gemini-notebook)
│   ├── bridge.json                    # Configuración de Antigravity Bridge (modelos y ACP)
│   └── config-prompt.md               # Prompt de plantilla para el comando /config
├── agents/                            # Definiciones de los 4 subagentes activos
│   ├── gentle-ai-explore.md           # Exploración rápida del codebase y búsqueda de contexto
│   ├── gentle-ai-worker.md            # Ejecución de cambios, implementación y refactorización
│   ├── gentle-ai-verify.md            # Verificación, pruebas y validación de entregables
│   └── vault-agent.md                 # Especialista en la bóveda de Obsidian y Gemini NotebookLM
├── skills/                            # Skills activas del ecosistema local
│   ├── vault-curator/                 # Curación, formato YAML y enrutamiento en la bóveda
│   ├── notebooklm-bridge/             # Sincronización e ingesta con Google Gemini NotebookLM
│   ├── concept-visualizer/            # Generación de explicadores visuales interactivos en HTML
│   ├── pi-hygiene/                    # Auditoría, limpieza y optimización de Pi, skills y Engram
│   ├── omarchy/                       # Configuración del entorno de escritorio Omarchy / Hyprland
│   ├── omarchy-app/                   # Creación de aplicaciones nativas C++/Qt Quick para Omarchy
│   ├── diagnose-crash/                # Diagnóstico de volcados de memoria (coredumpctl)
│   ├── skill-creator/                 # Creación de skills LLM-first independientes
│   └── skill-improver/                # Auditoría y mejora de skills existentes
└── orchestrator/                      # Reglas históricas (reemplazadas por ~/.pi/agent/AGENTS.md)
    ├── orchestrator.md
    └── orchestrator-delegation.md
```

---

## 2. Subagentes Activos (`agents/`)

El ecosistema mantiene **4 subagentes especializados**, accesibles y editables desde Obsidian:

| Subagente | Archivo en la Bóveda | Rol y Responsabilidad Principal |
| :--- | :--- | :--- |
| **Explore** | [[03 Recursos/Pi Hub/agents/gentle-ai-explore\|gentle-ai-explore]] | Investigación de lectura rápida, mapeo de directorios y recolección de evidencia sin modificar archivos. |
| **Worker** | [[03 Recursos/Pi Hub/agents/gentle-ai-worker\|gentle-ai-worker]] | Implementación de código, edición de archivos y ejecución de tareas acotadas bajo directrices del orquestador. |
| **Verify** | [[03 Recursos/Pi Hub/agents/gentle-ai-verify\|gentle-ai-verify]] | Auditoría de calidad, ejecución de tests y comprobación de criterios de aceptación tras los cambios. |
| **Vault Agent** | [[03 Recursos/Pi Hub/agents/vault-agent\|vault-agent]] | Especialista dedicado a la arquitectura de 3 capas de `~/Documents/Vault` (según [[Vault Map]]) y sincronización con Gemini NotebookLM. |

> [!NOTE] Subagentes locales adicionales (`~/.pi/agent/agents/`)
> Fuera de `Pi Hub/agents/`, en `~/.pi/agent/agents/` residen **3 subagentes especialistas locales** (no gestionados por `pi-ctl sync`):
> - **`mermaid-maker.md`**: genera, renderiza y valida diagramas Mermaid en `<cwd>/viz/`.
> - **`svg-maker.md`**: diseña, renderiza y valida figuras geométricas/espaciales en SVG.
> - **`researcher.md`**: subagente de investigación rápida.

---

## 3. Skills Activas (`skills/`)

Las **7 skills locales** extienden las capacidades de Pi bajo demanda cuando la tarea coincide con sus disparadores (*triggers*):

1. **`vault-curator`** (`skills/vault-curator/SKILL.md`): Curación, enrutamiento en las 3 capas (`02 Referencias/`, `AI OS/wiki/`, `01 Notas/`), validación de frontmatter YAML y auditoría de salud de la bóveda.
2. **`notebooklm-bridge`** (`skills/notebooklm-bridge/SKILL.md`): Orquestación de investigación web, ingesta de fuentes en cuadernos de Google Gemini NotebookLM vía MCP y síntesis en la bóveda.
3. **`concept-visualizer`** (`skills/concept-visualizer/SKILL.md`): Transformación de notas complejas o modelos mentales en infografías y explicadores HTML interactivos autocontenidos.
4. **`pi-hygiene`** (`skills/pi-hygiene/SKILL.md`): Mantenimiento integral de Pi Agent, poda de subagentes/skills redundantes y consolidación de memoria en Engram.
5. **`omarchy`** (`skills/omarchy/SKILL.md`): Personalización de usuario para el escritorio Linux Omarchy, Hyprland, Waybar, terminales y temas del sistema.
6. **`omarchy-app`** (`skills/omarchy-app/SKILL.md`): Construcción y empaquetado (`PKGBUILD`) de aplicaciones de escritorio en C++ y Qt Quick integradas con el tema activo de Omarchy.
7. **`diagnose-crash`** (`skills/diagnose-crash/SKILL.md`): Análisis forense de cierres inesperados (`SIGSEGV`, `SIGABRT`) mediante `coredumpctl` y simbolización de trazas.

> [!TIP] Consulta de Operaciones de la Bóveda
> Para ver cómo se integran estas skills con los protocolos internos del compilador LLM (`ingest`, `query`, `lint`, `index-and-log`), consultá [[Skills Map]].

### Skills locales en `~/.pi/agent/skills/` y `~/.agents/skills/`
- **Skills de creación/auditoría (`~/.agents/skills/`):**
  - `skill-creator/SKILL.md`: creación de skills LLM-first independientes.
  - `skill-improver/SKILL.md`: auditoría y refactorización de skills existentes.
- **Skills locales de aprendizaje y análisis (`~/.pi/agent/skills/`):**
  - `teach/SKILL.md`: enseñanza estructurada (*unconditional truths*, DAG Mermaid, `plan_init`/`plan_update`, `recall`/`explain`/`quiz`, `graph_record`).
  - `practice/SKILL.md`: práctica deliberada e intercalada (`graph_list_propositions` + `problem`/`conclude`).
  - `visualize/SKILL.md`: orquestación de diagramas Mermaid/SVG embebidos en `md-log`.
  - `SKILL.md` (`analyze-sessions`): análisis de costos y transcripts de sesiones pasadas.

> [!NOTE] Migración fuera de `gentle-pi` / `gentle-ai` (2026-11-14)
> Se desinstalaron `gentle-pi` y el binario `gentle-ai` (junto con el flujo ODD/RDD) manteniendo `gentle-engram` intacto. Las reglas globales ahora viven en `~/.pi/agent/AGENTS.md` y las utilidades clave se reemplazaron por extensiones locales autónomas.

---

## 4. Archivos de Configuración (`config/` y `orchestrator/`)

### `config/`
- **`settings.json`**: Controla el comportamiento del binario base de Pi (`tuiMode`, `theme`, `quietStartup`, extensiones habilitadas o bloqueadas y paquetes npm activos).
- **`subagents.json`**: Define qué subagentes están registrados, qué modelo utiliza cada uno por defecto y sus permisos de herramientas.
- **`mcp.json`**: Declara los servidores Model Context Protocol externos (como `gemini-notebook-mcp`).
- **`bridge.json`**: Ajustes del puente `@estebanforge/pi-antigravity-bridge` (modelo predeterminado, nivel de pensamiento y conexión con `agy`).
- **`config-prompt.md`**: Plantilla de prompt que habilita el comando `/config` dentro de Pi para modificar esta misma infraestructura mediante lenguaje natural.

### `orchestrator/` y `~/.pi/agent/AGENTS.md`
- **`~/.pi/agent/AGENTS.md`**: Reemplaza al orquestador de `gentle-pi` con reglas globales livianas (idioma, exploración previa, `todo` para 3+ pasos, verificación antes de cerrar y seguridad en Git).
- **`orchestrator/`**: Conservado solo como referencia histórica del antiguo contrato de `gentle-pi` (ya no se sincroniza al runtime).

### Extensiones, Prompts y Datos Locales (`~/.pi/agent/`)
Además de lo sincronizado en `Pi Hub/`, el runtime local incluye:
- **Paquetes activos (`settings.json`):** `npm:gentle-engram`, `npm:pi-web-access`, `npm:pi-btw`, `npm:@estebanforge/pi-antigravity-bridge`, `npm:pi-perplexity` y `git:github.com/amosblomqvist/pi-dictate`.
- **`~/.pi/agent/extensions/`:**
  - `todo.ts`: lista de tareas en TUI con persistencia por sesión y comando `/todo clear`.
  - `ask-user.ts`: herramientas interactivas `ask_user_question` y `ask_user_choice`.
  - `codegraph.ts`: indexación y consulta de símbolos (`init`, `query`, `explore`) sin depender de `gentle-pi`.
  - `learnharness/` (`index.ts`, `storage.ts`, `graph.ts`, `recall-plan.ts`, `practice.ts`, `flashcards.ts`): grafo de conocimiento, *free-recall* (`recall`/`explain`), tracker DAG (`plan_init`/`plan_update`), práctica (`problem`/`conclude`) y flashcards FSRS-6 (`/flashcards`), con persistencia en `03 Recursos/LearnHarness/`.
  - `quiz.ts`: evaluación múltiple calificada con *"I don't know"* y nota libre.
  - `md-log.ts`: espejado en vivo a Markdown (`/md-log`, `/md-unlog`) compatible con Obsidian.
  - `visual-tools/`: herramientas `write/edit/render_mermaid` y `write/edit/render_svg`.
  - `custom-header.ts`, `prompt-snippets/` (`alt+s` / `/snippets`) y `tts.ts`.
- **`~/.pi/agent/prompts/`:** `config.md` (symlink a `Pi Hub/config/config-prompt.md`) y `practice.md` (`/practice`).

---

## 5. CLI de Control Unificado: `pi-ctl` (`~/.local/bin/pi-ctl`)

La herramienta de línea de comandos `pi-ctl` permite inspeccionar y administrar todo el ecosistema desde cualquier terminal sin editar rutas ocultas a mano:

```bash
# 1. Estado general del ecosistema
pi-ctl status
```
- **`pi-ctl status`**: Muestra el estado completo de Pi, el modelo activo, los subagentes registrados, las skills habilitadas, los servidores MCP y el estado del servicio de memoria persistente **Engram**.

```bash
# 2. Sincronización Obsidian <-> Motor de Pi
pi-ctl sync
```
- **`pi-ctl sync`**: Sincroniza los cambios realizados desde Obsidian en `03 Recursos/Pi Hub/` con los directorios de ejecución de Pi (`~/.pi/agent/`, `~/.agents/skills/`) y recalcula los hashes de integridad (como el guardián de prompts de `gentle-pi`).

```bash
# 3. Conmutación rápida de perfil de modelo
pi-ctl mode fast
pi-ctl mode deep
```
- **`pi-ctl mode fast | deep`**: Cambia instantáneamente el perfil de inferencia entre el modo rápido (`flash`, ideal para consultas ágiles, edición ligera y bajo consumo de cuota) y el modo profundo (`argon`, diseñado para arquitectura compleja, razonamiento profundo y refactorizaciones críticas).

```bash
# 4. Gestión de skills empaquetadas
pi-ctl skill list
pi-ctl skill enable <skill>
pi-ctl skill disable <skill>
```
- **`pi-ctl skill list | enable <skill> | disable <skill>`**: Lista todas las skills empaquetadas disponibles y permite activar o desactivar skills específicas en caliente para mantener limpio el contexto del agente.

---

## 6. Modificación Interactiva desde el Chat (`/config`)

Además de editar los archivos en Obsidian o usar `pi-ctl` en la terminal, podés pedirle directamente a Pi que modifique su propia configuración desde adentro de cualquier sesión de chat usando el comando `/config`:

```text
/config <instrucción>
```

### ¿Cómo funciona?
1. Al escribir `/config <instrucción>`, Pi carga el contexto de `03 Recursos/Pi Hub/config/config-prompt.md`.
2. El agente localiza el archivo exacto dentro de `03 Recursos/Pi Hub/` (`config/`, `agents/`, `skills/` u `orchestrator/`) que corresponde a tu pedido.
3. Aplica la modificación respetando el esquema del sistema y ejecuta automáticamente `pi-ctl sync` para impactar los cambios y actualizar los hashes.

### Ejemplos de uso práctico:
- `/config cambiá el modelo del subagente explore a flash y sincronizá`
- `/config agregá una regla en vault-agent para que siempre verifique el frontmatter con Vault Map antes de guardar`
- `/config desactivá las skills de diseño UI que no esté usando`

---

## 7. Referencias y Conexiones en la Bóveda

- **Arquitectura técnica y auditoría del sistema:** [[Configuración y Ecosistema de Pi Agent]]
- **Manual operativo, atajos TUI y adaptabilidad:** [[Guía de Uso y Adaptabilidad de Pi Agent]]
- **Esquema estructural y reglas de enrutamiento de la bóveda:** [[Vault Map]]
- **Mapa de habilidades y protocolos del AI OS:** [[Skills Map]]
- **Sistema de aprendizaje en Pi:** [[pi-learn]] · [[how to learn with pi]]
