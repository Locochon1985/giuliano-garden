---
titulo: Configuración y Ecosistema de Pi Agent
type:
  - literature
author:
  - Giuliano
source_title: Auditoría y arquitectura de la configuración local de Pi Agent, extensiones, subagentes e integraciones externas
media_type:
  - especificacion
published: 2026-11-12
creada: 2026-11-12
retocada: 2026-11-14
description: Referencia integral y jerarquizada de toda la configuración actual de Pi Agent en el sistema (core, Antigravity Bridge, extensiones locales, AGENTS.md, MCP, Engram, Web Access y Omarchy), ordenada de mayor a menor relevancia con análisis de componentes prescindibles.
tags:
  - IA
status:
  - Procesado
temas:
  - Inteligencia Artificial
valor:
  - 5 - Bueno
---

# Configuración y Ecosistema de Pi Agent — Arquitectura y Auditoría del Sistema

## Síntesis
Este documento constituye la referencia técnica completa del ecosistema de **Pi Coding Agent (`v1.5.2`)** instalado en este sistema Linux (Omarchy). No se limita al binario base de Pi, sino que abarca **todas las capas que lo rodean, lo extienden o dependen de él**: el proveedor de modelos vía Google Antigravity, las reglas globales livianas (`~/.pi/agent/AGENTS.md`) y extensiones autónomas locales (`todo.ts`, `ask-user.ts`, `codegraph.ts`, `learnharness/`), el puente **MCP para Gemini Notebook**, la memoria persistente **Engram**, las herramientas de investigación web y la integración nativa con el entorno de escritorio **Omarchy**.

*(Actualizado 2026-11-14: se desinstalaron `gentle-pi` y el binario `gentle-ai` para reducir consumo de prompt y fricción operativa, manteniendo `gentle-engram` intacto).*

El contenido está estructurado **en orden descendente de relevancia** (desde el núcleo indispensable hasta los complementos estéticos o inactivos) y cierra con una **matriz de prescindibilidad** para saber exactamente qué se puede desinstalar, apagar o podar sin romper el flujo de trabajo.

---

## 1. Mapa Arquitectónico del Ecosistema

```
┌────────────────────────────────────────────────────────────────────────────┐
│                        USUARIO / TERMINAL (Omarchy)                        │
│              Pi TUI v1.5.2 (fullscreen, tema system / Omarchy)             │
└─────────────────────┬──────────────────────────────────┬───────────────────┘
                      │                                  │
       [Reglas y Extensiones Locales]         [Protocolos y Herramientas]
                      ▼                                  ▼
┌─────────────────────────────────────────┐ ┌────────────────────────────────┐
│   ~/.pi/agent/ (Sin gentle-pi/gentle-ai)│ │   Servidores, Paquetes y Ext.  │
│  • AGENTS.md global liviano             │ │  • gemini-notebook-mcp (stdio) │
│  • todo.ts + ask-user.ts + codegraph.ts │ │  • pi-web-access + pi-perplexity│
│  • Subagentes locales limpios (4 + 3)   │ │  • gentle-engram + pi-btw + dict│
│  • Skills propias (~/.agents/skills/)   │ │  • learnharness/quiz/md-log/viz│
└─────────────────────┬───────────────────┘ └────────────────┬───────────────┘
                      │                                      │
                      └──────────────────┬───────────────────┘
                                         ▼
┌────────────────────────────────────────────────────────────────────────────┐
│          @estebanforge/pi-antigravity-bridge v1.10.6 (Motor ACP/stream)    │
│     • Levanta servidor MCP interno (pi-bridge) con las tools de Pi         │
│     • Conecta con Google Antigravity CLI (agy) con aislamiento por sesión  │
│     • Provee modelos: Gemini 4, Gemini 3.8 Flash, Claude 5.5, etc.         │
└────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Componentes Ordenados de Mayor a Menor Relevancia

### Nivel 1: Núcleo de Ejecución y Proveedor de Modelos (Crítico / Indispensable)

#### 1.1. Pi Coding Agent Core (`v1.5.2`)
Es el motor base de consola interactiva (TUI) instalado mediante `mise` en `~/.local/share/mise/installs/pi/1.5.2/pi/`.
* **Archivo de configuración maestro:** `~/.pi/agent/settings.json` (symlink a `03 Recursos/Pi Hub/config/settings.json`)
* **Ajustes activos:**
  * `defaultProvider: "antigravity"` y `defaultModel: "gemini-4-hydrogen"`.
  * `tuiMode: "fullscreen"`: Ejecución a pantalla completa en la terminal.
  * `theme: "system"`: Tema sincronizado con el esquema activo del sistema/Omarchy.
  * `quietStartup: "header"`: Arranque limpio mostrando solo el encabezado.
  * `extensions: ["-builtin:codemode"]`: **Importante:** la extensión integrada `codemode` está explícitamente deshabilitada para evitar colisiones con el arnés de subagentes y herramientas de `gentle-pi`.
  * `packages`: 6 fuentes activas (`gentle-engram`, `pi-web-access`, `pi-btw`, `@estebanforge/pi-antigravity-bridge`, `pi-perplexity`, `git:github.com/amosblomqvist/pi-dictate`). *(Se removió `npm:gentle-pi`).*
* **Gestión de paquetes:** Centralizada en `~/.pi/agent/npm/package.json`, donde Pi instala sus extensiones como paquetes de Node.js (`node >= 22.19.0`).

#### 1.2. Antigravity Bridge (`@estebanforge/pi-antigravity-bridge v1.10.4`)
Es la pieza más importante después del binario de Pi, ya que **actúa como proveedor de LLMs y puente de ejecución**. Sin este paquete, Pi requeriría API keys directas pagas (OpenAI, Anthropic, etc.) en `auth.json`.
* **Ruta de configuración:** `~/.pi/agent/antigravity-bridge/config.json`
* **Cómo funciona:**
  1. Conecta Pi con el servidor oficial **Google Antigravity ACP** (`engine: "acp"`, alojado en hogares efímeros bajo `~/.pi/agent/antigravity-bridge/acp-homes/`).
  2. Levanta un servidor MCP interno llamado `pi-bridge` (`bridgeTools: "all"`) que inyecta todas las herramientas de Pi (`read`, `write`, `edit`, `grep`, `find`, `subagent_run`, `todo`, etc.) dentro de la sesión de Antigravity.
* **Parámetros clave configurados:**
  * `mode: "accept-edits"` y `skipPermissions: true`: Agiliza la edición de archivos sin bloqueos constantes, delegando la seguridad en las reglas del orquestador (`approvals.gateMode: "auto"`, `approvals.mode: "ask"`).
  * `defaultModel: "flash"` / `defaultThinking: "medium"`.
  * `webTools: false`: Desactiva las herramientas web nativas del bridge para cederle el control a `pi-web-access` y a los controles de red del sistema.
* **Catálogo de modelos habilitados (`models-cache.json`):**
  * `Gemini 4 Argon` (Low / Medium / High)
  * `Gemini 3.8 Flash` (Low / Medium / High)
  * `Claude Opus 5.5` (Low / Medium / High)
  * `Claude Sonnet 5.5` (Low / Medium / High)
  * `GPT-OSS 120B` (Medium)

---

### Nivel 2: Flujo Liviano, Extensiones Autónomas y Subagentes (Alta Relevancia)

#### 2.1. Reglas Globales y Extensiones Locales (`~/.pi/agent/`)
En lugar del arnés pesado `gentle-pi` (que fue desinstalado junto con el binario `gentle-ai` de Homebrew el 2026-11-14), Pi opera con primitivas propias y livianas:
* **Workflow global (`~/.pi/agent/AGENTS.md`):** Reglas breves de disciplina (mantener consultas en solo lectura, explorar antes de editar, usar `todo` en tareas de 3+ pasos, verificar con tests/checks antes de cerrar, y responder en español rioplatense con voseo).
* **Extensiones autónomas (`~/.pi/agent/extensions/`):**
  * `todo.ts`: Lista de tareas en TUI con persistencia por sesión y comando `/todo clear`.
  * `ask-user.ts`: Cuestionarios interactivos (`ask_user_question`) y selección única (`ask_user_choice`) en TUI.
  * `codegraph.ts`: Wrapper autónomo para indexar y consultar símbolos con el binario `codegraph` (`init`, `query`, `explore`).
* **Subagentes limpios (`~/.pi/agent/agents/` y `Pi Hub/agents/`):**
  * `gentle-ai-explore.md`, `gentle-ai-worker.md`, `gentle-ai-verify.md` (recreados sin dependencias de `gentle-ai` ni RDD) y `vault-agent.md`, más los especialistas `mermaid-maker`, `svg-maker` y `researcher`.
* **Skills propias (`~/.agents/skills/`):** Se incorporaron `skill-creator` y `skill-improver` independientes (en reemplazo de las empaquetadas en `gentle-pi`).

---

### Nivel 3: Integraciones Externas, Memoria y Herramientas de Contexto (Relevancia Media-Alta)

#### 3.1. Memoria Persistente Engram (`gentle-engram v0.7.0` + `engram v3.7.0`)
Sistema de memoria persistente local (SQLite + FTS5) compartido entre sesiones y compactaciones.
* **Arquitectura:**
  * **Extensión en Pi (`gentle-engram v0.7.0`):** Registra 21 herramientas nativas (`mem_search`, `mem_save`, `mem_update`, `mem_context`, `mem_doctor`, `mem_session_summary`, etc.) e inyecta el protocolo canónico de memoria por tópicos (`topic_key`).
  * **Servidor Go (`engram v3.7.0`):** Instalado globalmente vía `mise` (`~/.local/share/mise/shims/engram`, declarado en `~/.config/mise/config.toml`) y levantado automáticamente en `http://127.0.0.1:7437`.
  * **Base de datos local:** `~/.engram/engram.db` (modo WAL, proyectos registrados: `giuliano` y `giuliano-garden`).

#### 3.2. Servidor MCP Externo: Gemini Notebook (`gemini-notebook-mcp`)
Puente local desarrollado a medida que conecta Pi con la API oficial de **Google Gemini Notebook (NotebookLM)**.
* **Registro en Pi:** `~/.pi/agent/mcp.json` con `exposure: "direct"`.
* **Implementación externa:** `~/Projects/gemini-notebook-mcp/index.js` (Node.js sobre `stdio`).
* **Credenciales:** `~/.config/gemini-notebook/credentials.json` (OAuth 2.0 con auto-refresh transparente sin depender de `gcloud`).
* **7 Herramientas expuestas directamente:** `create_notebook`, `get_notebook`, `list_notebooks`, `update_notebook`, `create_source`, `get_source`, `list_sources`.
* *(Ver referencia detallada en [[Gemini Notebook MCP]])*.

#### 3.3. Skills del Sistema Operativo Omarchy (`~/.agents/skills/`)
Habilidades externas a Pi pero inyectadas en su registro global para que el agente comprenda y gobierne el sistema Linux anfitrión:
1. **`omarchy`**: **Crítica para tocar dotfiles.** Reglas obligatorias para modificar `~/.config/hypr/`, `alacritty`, `ghostty`, `kitty`, `foot`, `omarchy-shell`, temas, barras, monitores y atajos de Hyprland.
2. **`diagnose-crash`**: Flujo forense para diagnosticar cierres inesperados (`SIGSEGV`, `SIGABRT`) usando `coredumpctl` y `systemd-coredump`.
3. **`omarchy-app`**: Plantilla y guía para construir aplicaciones de escritorio nativas para Omarchy en C++ y Qt Quick (`qmake6`), integradas en vivo con el tema del sistema y empaquetadas con `PKGBUILD`.
* **Integración visual adicional:** `~/.pi/agent/themes/omarchy-system.json` sincroniza la paleta de colores de Omarchy con Pi.

#### 3.4. Investigación y Acceso Web (`pi-web-access v0.47.0`)
Extensión encargada de darle acceso a internet, lectura de documentación externa y análisis multimedia a Pi.
* **Capacidades:** Búsqueda multi-proveedor (`web_search`), verificación de afirmaciones con citas (`source_check`), extracción limpia de páginas web en Markdown, lectura de PDFs (`unpdf`), clonado e inspección de repositorios de GitHub y análisis de videos de YouTube o locales (`fetch_content`, `get_search_content`).
* **También expone binario MCP propio:** `pi-web-access-mcp`.

#### 3.5. Skills de Creación y Auditoría (`~/.agents/skills/`)
Se incorporaron versiones independientes y limpias de:
* `skill-creator`: creación de skills LLM-first con frontmatter válido.
* `skill-improver`: auditoría y mejora de `SKILL.md` existentes.

---

### Nivel 4: Complementos Secundarios o Legacy (Baja Relevancia)

#### 4.1. Conversaciones Paralelas (`pi-btw v0.8.1`)
* **Qué hace:** Agrega el comando `/btw` y la skill `btw` para abrir un hilo lateral superpuesto (overlay TUI) donde hacer preguntas rápidas al modelo sin ensuciar ni interrumpir el contexto de la conversación principal.

#### 4.2. Cadena Manual 4R (`~/.pi/agent/chains/4r-review.chain.md`)
* Archivo residual de compatibilidad manual; el propio encabezado aclara que el flujo moderno de captura de `gentle-ai` no utiliza esta cadena.

---

## 3. Análisis de Prescindibilidad: ¿Qué se puede quitar y qué no?

Para mantener el sistema limpio, rápido y con menor consumo de tokens en el prompt inicial, esta tabla clasifica cada componente según qué tan prescindible es:

| Componente / Herramienta | Categoría | ¿Es Prescindible? | Impacto de Eliminarlo y Recomendación |
|---|---|---|---|
| **Pi Core (`v1.5.2`) + `@estebanforge/pi-antigravity-bridge`** | Núcleo / Motor | ❌ **Indispensable** | **No tocar.** Sin el bridge perdés la conexión ACP a los modelos de Google/Claude y la ejecución de herramientas locales. |
| **Skills de Omarchy (`omarchy`, `diagnose-crash`)** | Sistema Anfitrión | ❌ **Imprescindible en Omarchy** | **Conservar.** Si usás Pi para configurar Hyprland, la barra o diagnosticar fallos del sistema, la skill `omarchy` evita que el agente rompa archivos autogenerados. (`omarchy-app` sí es prescindible si no creás apps en C++/Qt). |
| **MCP `gemini-notebook`** | Integración Externa | ⚠️ **Condicional (Según uso)** | **Conservar si usás NotebookLM.** Inyecta solo 7 herramientas ligeras. Si dejás de usar cuadernos de Gemini Notebook, podés quitarlo de `~/.pi/agent/mcp.json` para ahorrar un proceso Node.js en segundo plano. |
| **`pi-web-access`** | Investigación Web | ⚠️ **Poco prescindible** | **Recomendado conservar.** Como el bridge tiene `webTools: false`, quitar `pi-web-access` dejaría a Pi ciego frente a documentación actualizada en la web, PDFs o repos de GitHub. |
| **`AGENTS.md` + Extensiones Locales (`todo.ts`, `ask-user.ts`, `codegraph.ts`)** | Flujo Liviano | ⚖️ **Modular y bajo consumo** | **Reemplazo de `gentle-pi`.** Aporta tracking `todo`, cuestionarios en TUI y `codegraph` sin inyectar miles de tokens de ODD/RDD ni dependencias externas. |
| **`gentle-engram` (`v0.7.0`) + `engram` (`v3.7.0`)** | Memoria Persistente | ⚖️ **Prescindible según flujo** | **Operativo (`127.0.0.1:7437`, DB en `~/.engram/engram.db`).** Conserva contexto entre sesiones y compactaciones. Si en algún momento preferís usar únicamente tu Vault de Obsidian como memoria y ahorrar las ~21 herramientas del system prompt, podés quitar `npm:gentle-engram` de `settings.json`. |
| **Subagentes `jd-*` (Judgment Day) y `review-*` (RDD 4R)** | Auditoría Pesada | ✅ **Prescindibles para uso personal** | Están pensados para equipos grandes o código crítico de producción. Para proyectos personales, notas en el Vault o scripts locales, el ciclo completo de 4 lentes + doble juez ciego consume mucha cuota de modelos y tiempo. Podés mantener el switch de RDD apagado (`gentle-ai review mode disable`). |
| **Skills de Diseño (`gentle-design`, `gentle-ui-design`, `tui-design`)** | Diseño UI/HTML | ✅ **Prescindibles si no diseñás UI** | Si no usás el lienzo de artboards HTML de Gentle Design ni diseñás interfaces TUI, no aportan valor (aunque como las skills se leen bajo demanda, su costo en reposo es mínimo). |
| **`pi-btw` (`/btw`)** | Utilidad TUI | ✅ **Prescindible** | Es un *nice-to-have* cómodo para preguntas rápidas al margen sin ensuciar el chat actual, pero 100% opcional. Si no usás el comando `/btw`, se puede remover de `settings.json`. |
| **`chains/4r-review.chain.md`** | Compatibilidad Legacy | ✅ **Totalmente Prescindible** | Solo existe por compatibilidad manual; el flujo nativo no lo usa. |

---

## 4. Mapa de Archivos Clave en el Sistema

| Ruta | Propósito |
|---|---|
| `~/.pi/agent/settings.json` | Configuración global de Pi (paquetes activos, tema `Gentle`, modo `fullscreen`). |
| `~/.pi/agent/mcp.json` | Declaración de servidores MCP externos (actualmente `gemini-notebook`). |
| `~/.pi/agent/npm/package.json` | Manifiesto NPM de las 5 extensiones instaladas en el entorno de Pi. |
| `~/.pi/agent/antigravity-bridge/config.json` | Ajustes del motor ACP, permisos de edición (`accept-edits`) y modelo por defecto. |
| `~/.pi/agent/antigravity-bridge/models-cache.json` | Lista cacheada de modelos disponibles en Google Antigravity (`Gemini 4 Argon`, `Claude 5.5`, etc.). |
| `~/.pi/agent/AGENTS.md` | Reglas globales livianas de comportamiento y workflow (sin ODD/RDD). |
| `~/.pi/agent/extensions/*.ts` | Extensiones locales (`todo.ts`, `ask-user.ts`, `codegraph.ts`, `learnharness/`, `quiz.ts`, `md-log.ts`, `visual-tools/`). |
| `~/.pi/agent/agents/*.md` | Definiciones limpias de los subagentes globales (`gentle-ai-*`, `vault-agent`, `mermaid-maker`, `svg-maker`, `researcher`). |
| `~/.pi/agent/themes/omarchy-system.json` | Tema visual generado por Omarchy para integrarse con la terminal. |
| `~/.agents/skills/` | Skills nativas del sistema operativo (`omarchy`, `omarchy-app`, `diagnose-crash`). |
| `~/.engram/engram.db` | Base de datos SQLite + FTS5 de memoria persistente de Engram (`engram v3.7.0` vía `mise`). |
| `~/Projects/gemini-notebook-mcp/` | Código fuente del servidor MCP local para Google Gemini Notebook. |

---

## Reflexiones y Recomendaciones Prácticas

1. **Estado de `gentle-engram` (Reparado y Activo):** Se instaló el binario `engram v3.7.0` de forma global mediante `mise` (`mise use -g engram@latest`), reconectando automáticamente la extensión de Pi con tu base de datos preexistente en `~/.engram/engram.db` (proyectos `giuliano` y `giuliano-garden`, 10/10 chequeos en `mem_doctor` OK).
2. **El verdadero valor diferencial de tu setup:** La combinación de **Pi + Antigravity Bridge + Engram + MCP de Gemini Notebook + Vault de Obsidian + Skills de Omarchy** convierte al agente en un operador integral: puede administrar tu escritorio Linux (`omarchy`), recordar decisiones técnicas entre sesiones (`engram`), investigar en la web (`pi-web-access`), sincronizar fuentes con tus cuadernos de estudio en la nube (`gemini-notebook`) y consolidar el conocimiento a largo plazo en tu wiki local (`~/Documents/Vault/`).

---

## Referencias Relacionadas
* [[Gemini Notebook MCP]]
* [[Pi Hub - Panel de Control]]
* [[pi-learn]]
* [[how to learn with pi]]
* [[llm-wiki]]
* [[Second brain setup for doing]]
* [[Solución Audio Omarchy]]
* [[Guia de git]]
