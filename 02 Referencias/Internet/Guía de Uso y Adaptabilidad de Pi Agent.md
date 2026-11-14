---
titulo: Guía de Uso y Adaptabilidad de Pi Agent
type:
  - literature
author:
  - Giuliano
source_title: Guía operativa, personalización y adaptación de uso de Pi Coding Agent según su filosofía minimalista
media_type:
  - especificacion
published: 2026-11-12
creada: 2026-11-12
retocada: 2026-11-14
description: Manual práctico y de referencia sobre cómo operar Pi Agent en el día a día con la configuración actual del sistema, cómo modificar sus ajustes y cómo adaptar su uso desde un asistente minimalista hasta un arnés de ingeniería complejo.
tags:
  - IA
status:
  - Procesado
temas:
  - Inteligencia Artificial
  - Herramientas de Desarrollo
valor:
  - 5 - Bueno
---

# Guía de Uso y Adaptabilidad de Pi Agent

## Síntesis y Filosofía de Diseño
**Pi (`v1.5.2`)** no fue concebido como un entorno monolítico cerrado ni como un software con opiniones rígidas preinstaladas. A diferencia de herramientas que imponen interfaces pesadas o modos fijos de planificación, **Pi se define como un arnés de agentes deliberadamente minimalista, extensible y hackeable**.

> *"Adaptá Pi a tus flujos de trabajo, no al revés."*
> — Manifiesto de diseño de Pi.

El núcleo de Pi se limita a ofrecer primitivas robustas:
1. Una interfaz de terminal interactiva (TUI) ligera y reactiva con autocompletado y gestión de contexto.
2. Un motor de ejecución de herramientas esenciales (`read`, `write`, `edit`, `bash`).
3. Una arquitectura de extensión modular en capas: **Extensiones (npm)**, **Habilidades (Skills)**, **Servidores de Protocolo de Contexto (MCP)**, **Plantillas de Prompt** y **Temas**.

En este sistema Linux (Omarchy), Pi tiene montado un ecosistema completo (el arnés `gentle-pi`, memoria persistente `engram`, puente `antigravity-bridge`, acceso web y herramientas para Gemini Notebook). Sin embargo, comprender su naturaleza modular permite **escalar o reducir su complejidad** según la tarea: desde una tubería de consola minimalista de una sola línea hasta un entorno de desarrollo guiado por pruebas y subagentes.

---

## 1. Operación Diaria: Cómo Usar Pi con la Configuración Actual

### 1.1. Inicio y Descubrimiento de Contexto
Pi siempre debe ejecutarse situado en el directorio de trabajo donde se va a operar:
```bash
cd ~/Projects/mi-proyecto   # o ~/Documents/Vault
pi
```
Pi utiliza el directorio actual de la shell para:
* Detectar reglas locales del proyecto (archivos `AGENTS.md` o `.pi/`).
* Agrupar las sesiones guardadas bajo ese contexto específico.
* Resolver rutas relativas para lecturas, búsquedas y autocompletado con `@`.

---

### 1.2. Dominio de la Interfaz Interactiva (TUI) y Atajos

| Atajo | Acción | Para qué sirve en la práctica |
|---|---|---|
| `Enter` | Enviar mensaje | Despacha el prompt actual al modelo. |
| `Shift + Enter` | Salto de línea | Permite redactar mensajes multilínea sin enviar. |
| `@` | Selector de archivos | Abre un buscador interactivo para adjuntar archivos o imágenes al prompt. |
| `Ctrl + G` | Editor externo | Abre tu editor de texto de terminal (`$EDITOR`) para redactar prompts largos y estructurados. |
| `Ctrl + L` | Selector de modelo | Cambia al vuelo el LLM activo (`Gemini 3.8 Flash`, `Gemini 4 Argon`, `Claude 5.5`, etc.). |
| `Shift + Tab` | Ciclar razonamiento | Alterna el nivel de *Thinking* (`off` → `low` → `medium` → `high`). |
| `Ctrl + O` | Expandir/Colapsar tools | Oculta o muestra el detalle y la salida de las herramientas ejecutadas. |
| `Ctrl + T` | Mostrar/Ocultar pensamiento | Colapsa o despliega los bloques de razonamiento interno del modelo. |
| `Ctrl + X` | Copiar respuesta | Copia al portapapeles del sistema la última respuesta del asistente. |
| `Escape` | Interrumpir / Cancelar | Detiene inmediatamente la generación o ejecución de herramientas en curso. |
| `Alt + Enter` | Encolar mensaje | Envía una instrucción que se ejecutará secuencialmente cuando termine la tarea actual. |
| `Alt + Up` | Recuperar cola | Devuelve al editor los mensajes que estaban encolados pendientes. |

---

### 1.3. Comandos Barra (`/`) y Atajos de Shell (`!`)

Escribiendo `/` se despliega el menú de comandos integrados y de extensiones:

#### Gestión de Sesiones y Contexto
* `/new`: Inicia una nueva sesión limpia en el mismo directorio.
* `/resume`: Lista y restaura sesiones anteriores del proyecto actual.
* `/compact`: Resume y compacta el historial de la conversación para liberar ventana de contexto cuando el chat se vuelve extenso.
* `/tree`, `/fork`, `/clone`: Permiten ramificar la conversación para probar un enfoque alternativo sin perder el hilo original.
* `/name <nombre>`: Asigna un nombre descriptivo a la sesión actual para encontrarla fácilmente en el historial.
* `/session`: Muestra estadísticas detalladas (tokens consumidos, costo acumulado, archivos de log).

#### Consultas Rápidas en Paralelo (`/btw`)
La extensión `pi-btw` provee el comando `/btw`:
```text
/btw ¿cómo era la expresión regular para filtrar IPs en grep?
```
Abre un hilo secundario instantáneo para resolver una duda puntual sin contaminar ni agrandar el historial de contexto de la tarea principal.

#### Comandos Rápidos de Shell (`!` y `!!`)
* `!comando`: Ejecuta un comando en tu sistema e **inyecta su salida directamente en la conversación** para que el modelo la analice (ej: `!git status`, `!cargo check`, `!ls -la`).
* `!!comando`: Ejecuta el comando en tu terminal local **sin compartir la salida con el modelo** (ideal para tareas privadas o comandos ruidosos).

---

### 1.4. Flujo de Trabajo Liviano (`AGENTS.md` + Extensiones Locales)
Con la configuración actual (sin `gentle-pi` ni `gentle-ai`), Pi opera con un flujo directo y sin burocracia de ODD/RDD:

1. **Consultas e investigación:** Se mantienen en solo lectura sin disparar escrituras ni tareas innecesarias.
2. **Tareas de 3+ pasos:** El agente organiza y actualiza el plan mediante la extensión local `todo.ts` (visible arriba del editor; podés limpiarla con `/todo clear`).
3. **Decisiones abiertas:** Cuando hace falta definir un criterio de diseño o preferencia, usa `ask-user.ts` (`ask_user_question` / `ask_user_choice`) en la TUI.
4. **Memoria Persistente con Engram:** El sistema recuerda decisiones técnicas pasadas a través de `gentle-engram` (`mem_search`, `mem_save`, `mem_context`, `mem_doctor`).
5. **Acceso Web y Cuadernos de Gemini:**
   * `pi-web-access`: búsqueda web, descarga de documentación, PDFs o repositorios de GitHub.
   * `gemini-notebook-mcp`: consulta e ingesta de fuentes en tus cuadernos de Google NotebookLM.

---

## 2. Cómo Cambiar la Configuración

La configuración de Pi sigue un orden de precedencia claro: **Opciones de CLI > Configuración local del proyecto (`.pi/`) > Configuración global del usuario (`~/.pi/agent/`) > Valores por defecto del motor**.

```
┌──────────────────────────────────────────────────────────┐
│                   Nivel 1: Flags de CLI                  │
│       (pi --model ..., pi --thinking ..., pi -p ...)      │
└────────────────────────────┬─────────────────────────────┘
                             ▼
┌──────────────────────────────────────────────────────────┐
│          Nivel 2: Directorio del Proyecto (.pi/)          │
│         (.pi/settings.json, .pi/skills/, AGENTS.md)      │
└────────────────────────────┬─────────────────────────────┘
                             ▼
┌──────────────────────────────────────────────────────────┐
│         Nivel 3: Directorio Global (~/.pi/agent/)        │
│   (settings.json, antigravity-bridge/, mcp.json, npm/)   │
└──────────────────────────────────────────────────────────┘
```

---

### 2.1. Ajustes Globales de Pi (`~/.pi/agent/settings.json`)
Controla el aspecto visual y los paquetes que Pi carga al arrancar:

```json
{
  "tuiMode": "fullscreen",
  "theme": "Gentle",
  "quietStartup": "header",
  "extensions": [
    "-builtin:codemode"
  ]
}
```

* **Cambiar el modo visual (`tuiMode`):**
  * `"fullscreen"`: Modo actual, ocupa toda la terminal.
  * `"inline"`: Estilo chat tradicional; se desplaza hacia abajo en la shell preservando el historial previo de tu terminal.
* **Cambiar el tema visual (`theme`):**
  * Podés cambiarlo a `"Omarchy"` (sincronizado con el sistema en `themes/omarchy-system.json`) o a cualquiera de los temas integrados (`"dark"`, `"light"`).
* **Deshabilitar o habilitar extensiones (`extensions`):**
  * Si querés desactivar temporalmente una extensión sin desinstalarla, anteponé un guion (`-`). Por ejemplo, para apagar el arnés pesado y dejar a Pi liviano:
    ```json
    "extensions": [
      "-builtin:codemode",
      "-npm:gentle-pi"
    ]
    ```

---

### 2.2. Proveedor de Modelos y Permisos (`~/.pi/agent/antigravity-bridge/config.json`)
Gobierna la conexión con Google Antigravity (ACP) y el comportamiento de las herramientas:

```json
{
  "engine": "acp",
  "defaultModel": "flash",
  "defaultThinking": "medium",
  "mode": "accept-edits",
  "skipPermissions": true,
  "webTools": false
}
```

* **Cambiar el modelo por defecto (`defaultModel`):**
  * `"flash"`: Rápido, económico y eficiente (`Gemini 3.8 Flash`).
  * `"argon"`: Máxima capacidad de contexto y razonamiento (`Gemini 4 Argon`).
  * `"sonnet"` o `"opus"`: Si preferís la familia Claude 5.5.
* **Ajustar el nivel de razonamiento (`defaultThinking`):**
  * `"low"`: Respuestas casi instantáneas; ideal para tareas mecánicas.
  * `"medium"`: Balance óptimo (configuración actual).
  * `"high"`: Razonamiento profundo para diseño de arquitecturas o bugs difíciles.
* **Control de permisos de edición (`mode` y `skipPermissions`):**
  * Si querés que Pi te pida confirmación antes de tocar cualquier archivo, cambiá `skipPermissions: false` y `mode: "ask"`.

---

### 2.3. Servidores MCP Externos (`~/.pi/agent/mcp.json`)
Para conectar Pi con otras herramientas locales o remotas mediante el Model Context Protocol:

```json
{
  "servers": {
    "gemini-notebook": {
      "command": "node",
      "args": ["/home/giuliano/Projects/gemini-notebook-mcp/index.js"],
      "exposure": "direct"
    }
  }
}
```
* Para agregar un nuevo servidor MCP (por ejemplo, SQLite, Docker o GitHub MCP), podés añadir una entrada a este archivo o ejecutar en la terminal:
  ```bash
  pi mcp add mi-servidor "comando" "arg1" "arg2"
  ```
* Para consultar los servidores activos: `pi mcp list`.

---

### 2.4. Personalización mediante Habilidades (Skills) y Reglas de Proyecto
Pi lee habilidades desde dos ubicaciones:
1. **Globales del sistema:** `~/.agents/skills/<nombre-skill>/SKILL.md` (aquí residen las skills `omarchy`, `diagnose-crash` y `omarchy-app`).
2. **Locales del proyecto:** `.pi/skills/` o el archivo `AGENTS.md` en la raíz de cualquier repositorio.

#### Cómo crear una Skill personalizada
Solo se requiere crear un directorio con un archivo `SKILL.md` que contenga metadatos en YAML:
```markdown
---
name: obsidian-vault
description: Instrucciones y estándares para estructurar notas en el Vault personal de Obsidian.
---

# Reglas del Vault
- Todas las notas nuevas deben incluir frontmatter YAML con 'titulo', 'type', 'tags'.
- Usar enlaces bidireccionales con sintaxis [[Nota Relacionada]].
- No modificar notas existentes sin leerlas previamente en su totalidad.
```
Cualquier archivo `SKILL.md` colocado en `~/.agents/skills/` o dentro de un proyecto será indexado automáticamente por Pi y cargado solo cuando la tarea lo amerite, sin consumir tokens en reposo.

---

## 3. Cómo Alterar el Uso: Adaptando Pi a tus Necesidades

Gracias a su arquitectura minimalista, podés alterar drásticamente la forma en que usás Pi sin necesidad de reinstalar ni reconfigurar todo el sistema.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        ESPECTRO DE USO DE PI                           │
├──────────────────────────┬─────────────────────────┬───────────────────┤
│    Minimalista / CLI     │    Estudio / Vault      │    Ingeniería     │
│   (Zero-Ceremony)        │   (Investigación)       │   (ODD / TDD)     │
├──────────────────────────┼─────────────────────────┼───────────────────┤
│ • Sin extensiones        │ • Web access activo     │ • gentle-pi ON    │
│ • Tuberías Unix (|)      │ • Gemini Notebook MCP   │ • Subagentes      │
│ • Thinking low / off     │ • Lectura/escritura md  │ • Tracking todo   │
│ • Latencia mínima        │ • Memoria Engram        │ • Commits slices  │
└──────────────────────────┴─────────────────────────┴───────────────────┘
```

---

### Modo 1: El Pi Minimalista y Puro ("Zero-Ceremony")
Si vas a realizar tareas rápidas y no querés que el arnés cargue personalidades, subagentes ni ceremonias de ODD:

```bash
# Ejecutar Pi desactivando todas las extensiones instaladas
pi --no-extensions

# O desactivar solo el arnés y skills para máxima velocidad
pi --no-skills --thinking low
```
* **Ventaja:** Consumo ínfimo de tokens en el system prompt, latencia casi nula y respuestas directas sin formateos adicionales.

---

### Modo 2: Pi en Tuberías Unix y Scripts (Modo no interactivo)
Pi no requiere abrir la interfaz visual obligatoriamente. Se puede integrar en scripts de bash mediante el flag `-p` (`--print`):

```bash
# Revisar un diff antes de hacer commit
git diff | pi -p "Revisá este diff y decime si encontrás algún bug o riesgo evidente"

# Extraer información de un archivo local usando la referencia @
pi -p "@servidores.log extraé todas las líneas con errores 500 y ordenalas por frecuencia"

# Traducir o formatear texto al vuelo
cat apunte.txt | pi -p "Convertí estas notas dispersas en una lista markdown limpia" > apunte_limpio.md
```

Para automatizaciones complejas, `--mode json` emite un flujo estructurado en JSONL apto para ser procesado con `jq` u otros lenguajes.

---

### Modo 3: Modo de Investigación y Gestión de Conocimiento (Vault + Web + NotebookLM)
Podés transformar a Pi en un asistente de investigación académica o técnica:
1. **Consultar la web:** Pedile que busque documentación reciente (`"Buscá en la web la documentación de Hyprland v0.45 y explicame los cambios"`).
2. **Sincronizar con NotebookLM:** Usá las tools de `gemini-notebook` para añadir fuentes o extraer síntesis de tus cuadernos en la nube.
3. **Consolidar en Obsidian:** Pedile que genere la nota estructurada en `~/Documents/Vault/02 Referencias/...` con los metadatos estándar de tu segundo cerebro.

---

### Modo 4: Modo de Aislamiento por Proyecto (`.pi/` local)
Si estás trabajando en un proyecto específico y no querés que use las configuraciones globales, simplemente creá una carpeta `.pi/` dentro de tu proyecto:
```bash
cd ~/Projects/mi-app-secreta
mkdir -p .pi
```
Creá un `.pi/settings.json`:
```json
{
  "theme": "dark",
  "extensions": []
}
```
Pi utilizará esa configuración aislada únicamente mientras estés trabajando dentro de esa carpeta, manteniendo intacta tu configuración global.

---

## 4. Matriz Rápida: "Quiero lograr X" → "Qué hacer"

| Qué querés lograr | Acción directa |
|---|---|
| **Hacer una pregunta rápida sin ensuciar la sesión** | Usar `/btw <tu consulta>` en el chat. |
| **Cambiar de modelo en mitad de una sesión** | Presionar `Ctrl + L` o ejecutar `/model`. |
| **Acelerar la respuesta y gastar menos cuota** | Presionar `Shift + Tab` hasta fijar el thinking en `low` o `off`. |
| **Redactar un prompt largo cómodamente** | Presionar `Ctrl+G` para editar en tu editor de terminal favorito. |
| **Adjuntar un archivo o captura al vuelo** | Escribir `@` y tipear el nombre del archivo. |
| **Ver qué comandos o tools se ejecutaron** | Presionar `Ctrl + O` para desplegar el log de herramientas. |
| **Usar Pi en un script de terminal** | Usar `pi -p "prompt"` o pasar datos con pipes `cat archivo \| pi -p "..."`. |
| **Correr Pi totalmente limpio y minimalista** | Iniciar con `pi --no-extensions`. |
| **Modificar dotfiles del sistema de forma segura** | Pedirle a Pi que aplique las reglas de la skill `omarchy`. |
| **Guardar una decisión técnica para el futuro** | Pedirle al agente: *"Guardá esto en la memoria de Engram"*. |
| **Liberar contexto cuando la sesión se vuelve lenta** | Ejecutar `/compact` o ramificar con `/fork`. |

---

## Conclusiones
La potencia de Pi radica en que **el núcleo nunca es un obstáculo**: no te obliga a usar subagentes, ni te ata a un modelo comercial específico, ni te impone un flujo de desarrollo único. 

En este sistema tenés a tu disposición un entorno de alta ingeniería (`gentle-pi`, `engram`, `omarchy`), pero podés apagar capas con un simple flag de terminal (`--no-extensions`), adaptar los modelos con un atajo (`Ctrl+L`), o crear tus propias habilidades con un archivo `SKILL.md`. La herramienta se pliega a lo que tu tarea demande.

---

## Referencias Relacionadas
* [[Configuración y Ecosistema de Pi Agent]]
* [[Gemini Notebook MCP]]
* [[llm-wiki]]
* [[Second brain setup for doing]]
* [[Guia de git]]
