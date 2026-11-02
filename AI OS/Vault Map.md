---
title: Vault Map - Mapa de la Bóveda
type: system-map
up: "[[ME]]"
related:
  - "[[Skills Map]]"
created: 2026-10-26
tags:
  - Sistema
---

# Vault Map — Mapa de la Bóveda

> [!IMPORTANT] Instrucción para Agentes de IA
> Este archivo define **dónde está cada cosa** en la bóveda. Antes de crear, mover o buscar archivos, consulta este mapa. **No crees archivos en la raíz sin una razón explícita y sin actualizar este documento.**

---

## 1. Estructura General de Directorios

```text
Vault/
├── AGENTS.md                        # Reglas globales para agentes
├── 00 Diario/                       # Notas diarias
├── 01 Notas/                        # Notas atómicas y conceptos propios (Zettelkasten)
├── 02 Referencias/                  # Fuentes externas (libros, artículos, Biblioteca.base)
├── 03 Recursos/                     # Infraestructura y recursos de soporte
│   ├── Anexos/                      # Archivos adjuntos
│   ├── Modelo/                      # Modelos visuales (Excalidraw, imágenes)
│   └── Plantillas/                  # Plantillas Templater
├── 04 Proyectos/                    # Proyectos activos, MOCs de proyecto y ensayos
│   ├── Toma de notas.md             # MOC operativo de toma de notas
│   ├── Inmediato/                   # Proyectos y entregables de corto plazo
│   └── Largo Plazo/                 # Proyectos de mediano y largo plazo
├── 05 Marcos/                       # Mapas conceptuales y categorías de dominio
│   ├── Generales/                   # Disciplinas y dominios universales (Marcos/General)
│   └── Personales/                  # Marcos propios (Marcos/Personal)
├── AI OS/                           # Sistema operativo de la bóveda (Schema, Operaciones y LLM Wiki)
│   ├── ME.md                        # Contexto personal, roles y directrices
│   ├── Vault Map.md                 # Este archivo (Schema estructural)
│   ├── Skills Map.md                # Índice de operaciones y skills
│   ├── SKILLS/                      # Protocolos ejecutables (ingest, query, lint, index-and-log)
│   │   ├── ingest.md
│   │   ├── query.md
│   │   ├── lint.md
│   │   └── index-and-log.md
│   └── wiki/                        # Base de conocimientos generada y mantenida por LLM
│       ├── index.md                 # Índice general de contenido
│       ├── log.md                   # Registro cronológico de operación
│       ├── Overview.md              # Página de descripción general
│       ├── convenciones.md          # Convenciones de uso y preferencias
│       ├── fuentes/                 # Páginas de resumen de materiales
│       ├── entidades/               # Páginas de entidades (personas, organizaciones, herramientas)
│       ├── conceptos/               # Páginas de conceptos (teorías, métodos, patrones)
│       └── análisis/                # Páginas de análisis (comparaciones, síntesis)
├── .obsidian/                       # Configuración de Obsidian (no editar manualmente)
├── .stfolder/                       # Metadatos de sincronización (Syncthing)
└── .trash/                          # Papelera de Obsidian
```

---

## 1.5 Arquitectura de 3 Capas y Frontera `AI OS/wiki/` vs. `01 Notas/`

Siguiendo el diseño de `[[llm-wiki]]`, la bóveda separa tres capas con una **frontera estricta entre el compilador automático del LLM y la producción personal de Giuliano**:

1. **Capa 1: Raw Sources (Fuentes Crudas / Inmutables)**
   - **Ubicación:** `02 Referencias/`, `03 Recursos/Anexos/` y las capturas en bruto de `00 Diario/`.
   - **Regla:** Son la fuente primaria de verdad (libros, recortes de Obsidian Web Clipper, PDFs, apuntes de clase). El agente lee de ellas y completa sus metadatos/síntesis, pero nunca altera ni destruye el texto original del autor.
2. **Capa 2: The Wiki (Separación entre Pre-Compilación IA y Canon Personal)**
   - **`AI OS/wiki/` (Territorio 100% Automático del Compilador LLM):**
     - Aquí el agente vuelca y mantiene apuntes enciclopédicos objetivos, resúmenes exhaustivos de materiales (`fuentes/`), fichas de autores/organizaciones/herramientas (`entidades/`), definiciones teóricas o de cátedra (`conceptos/`) y comparaciones/cruces (`análisis/`).
     - Se redacta en tercera persona objetiva y densamente interconectada, liberando a Giuliano del trabajo mecánico de resumir textos largos o manuales universitarios.
   - **`01 Notas/`, `05 Marcos/` y `04 Proyectos/` (Territorio de Voz Propia / Canon Personal):**
     - `01 Notas/` está reservado **exclusivamente para reflexiones escritas o curadas en la propia voz de Giuliano** (primera persona, postura crítica, ejemplos propios, escala de `valor` y estados `🌱semilla` / `🌿arbusto` / `🌲perenne` / `🥀sequía` / `🪾marchita`), las cuales se nutren de lo compilado en `AI OS/wiki/`.
3. **Capa 3: The Schema & Operations (Fuente Única de Reglas)**
   - **Ubicación:** Este archivo (`AI OS/Vault Map.md`) es la **única fuente de verdad** para la estructura, enrutamiento y formato YAML de toda la bóveda, complementado por `AI OS/ME.md` (contexto del usuario) y `AI OS/Skills Map.md` (`AI OS/SKILLS/`).

---

## 2. Propósito de Cada Carpeta

### `00 Diario/`
- **Contenido:** Notas diarias y registros temporales.
- **Enrutamiento:** Entradas de diario y revisiones diarias.

### `01 Notas/`
- **Contenido:** Notas atómicas, evergreen y conceptos permanentes.
- **Convención:** Un concepto por archivo. Títulos descriptivos en español, redactadas en primera persona.
- **Frontmatter esperado:** `type` (`evergreen` | `concept`), `titulo`, `creada`, `retocada`, `reactivada`, `motivo`, `tags`, `temas`, `valor`.
- **Enrutamiento:** Toda idea propia, reflexión o concepto consolidado va aquí.

### `02 Referencias/`
- **Contenido:** Fuentes externas: libros, artículos, literatura y `Biblioteca.base`.
- **Ejemplos actuales:** `LA VIDA INTELECTUAL.md`, `La invitación una historia teatral elevada por su elenco y por una muy cuidada dirección.md`, `Biblioteca.base`.
- **Enrutamiento:** Toda fuente externa o material de estudio va aquí.

### `03 Recursos/`
- **Contenido:** Infraestructura de soporte del sistema de notas y modelos visuales.
- **Subcarpetas:**
  - `Anexos/` → archivos auxiliares y adjuntos.
  - `Modelo/` → esquemas y modelos visuales (`.excalidraw`, imágenes).
  - `Plantillas/` → Plantillas Templater (`atómica.md`, `Categoría General.md`, `Categoría Personal.md`, `diario.md`, `libro.md`, `literatura.md`, `Mini-ensayo.md`, `moc.md`, `Preguntas.md`).
- **Enrutamiento:** Plantillas, modelos gráficos y recursos reutilizables.

### `04 Proyectos/`
- **Contenido:** Proyectos activos, entregables y ensayos divididos por horizonte temporal.
- **Subcarpetas:**
  - `Inmediato/` → proyectos en curso y entregables próximos.
  - `Largo Plazo/` → iniciativas de desarrollo extendido.
- **Enrutamiento:** Proyectos accionables y producción con entregable.

### `05 Marcos/`
- **Contenido:** Índices dinámicos, categorías de dominio y mapas de contenido (MOCs).
- **Subcarpetas:**
  - `Generales/` → disciplinas universales (`type: category general`, tag `Marcos/General`).
  - `Personales/` y `Personales/MOCs/` → marcos conceptuales propios (`type: category personal`, tag `Marcos/Personal`) y mapas de contenido (`type: map of content`, tag `MOCs`).
- **Enrutamiento:** Nuevas categorías generales, marcos personales y MOCs permanentes.

### `AI OS/`
- **Contenido:** Sistema operativo de la bóveda (`ME.md`, `Vault Map.md`, `Skills Map.md`, `SKILLS/`) y subsistema de pre-compilación automática del agente en `AI OS/wiki/` (`index.md`, `log.md`, `Overview.md`, `convenciones.md`, `fuentes/`, `entidades/`, `conceptos/`, `análisis/`).
- **Enrutamiento:** Archivos de sistema y páginas enciclopédicas compiladas por el LLM en tercera persona (`AI OS/wiki/`). Las notas propias en primera persona van siempre a `01 Notas/`.

---

## 3. Convenciones de Nombres y Formato (Fuente Única de Reglas)

### Nombres de Archivo y Voz
- Usar español, con espacios y acentos cuando sea necesario. Un concepto por archivo.
- **Voz en `01 Notas/`:** Primera persona, reflexión propia de Giuliano, con ejemplos personales y citas en `## Fuentes`.
- **Voz en `AI OS/wiki/`:** Tercera persona enciclopédica, objetiva y densamente interconectada, mantenida automáticamente por el agente.
- Partir siempre de la plantilla correspondiente en `03 Recursos/Plantillas/` y preservar la sintaxis de Templater (`<% ... %>`) cuando se editen plantillas.

### Frontmatter YAML por Tipo de Nota
1. **Notas Atómicas (`01 Notas/` — plantilla `atómica.md`):**
   ```yaml
   ---
   titulo: <Título de la nota>
   type: evergreen | concept
   creada: YYYY-MM-DD
   retocada: YYYY-MM-DD
   reactivada: YYYY-MM-DD
   motivo: <Motivo de reactivación (opcional)>
   tags:
     - atómica/🌱semilla
   temas:
     - "[[Marco en 05 Marcos]]"
   valor:
     - 6 - Excelente
   ---
   ```
2. **Notas de Literatura / Web Clipper (`02 Referencias/` — plantilla `literatura.md`):**
   ```yaml
   ---
   titulo: "<Título>"
   type: literature
   author:
     - "<Autor>"
   source_title: "<URL o fuente>"
   media_type:
     - articulo | podcast | video | conferencia
   published: YYYY-MM-DD
   creada: YYYY-MM-DD
   retocada:
   description: "<Descripción breve>"
   tags:
     - Fuente/Articulo
   status: Pendiente | Procesado
   temas:
   valor:
   ---
   ```
3. **Fichas de Libros (`02 Referencias/` — plantilla `libro.md` / QuickAdd Book Search):**
   - Requieren `type: book` y `tags: Libros` para aparecer en `02 Referencias/Biblioteca.base` (que lee la propiedad `Portada`).
4. **Páginas del Compilador LLM (`AI OS/wiki/`):**
   - Ver detalles de las 4 subcarpetas en `[[convenciones]]` (`type: wiki-source | wiki-entity | wiki-concept | wiki-analysis`).

### Tags
- **Madurez atómica (con emoji):** `atómica/🌱semilla`, `atómica/🌿arbusto`, `atómica/🌲perenne`, `atómica/🪾marchita`, `atómica/🥀sequía`.
- **Marcos y Mapas:** `Marcos/General`, `Marcos/Personal`, `MOCs`.
- **Fuentes y Producción:** `Fuente/Articulo`, `Fuente/Podcast`, `Fuente/Video`, `Fuente/Conferencia`, `Libros`, `Mini-Ensayo`, `Diario`.
- **Subsistema Wiki (`AI OS/wiki/`):** `Wiki/Fuente`, `Wiki/Entidad`, `Wiki/Concepto`, `Wiki/Analisis`.
- **Sistema:** `Sistema`, `Skill`.

### Propiedad `temas`
- Siempre en plural (`temas`, nunca `tema`) tanto en el frontmatter como en consultas Dataview (`WHERE contains(temas, this.file.link)`).
- En notas atómicas: lista de wikilinks a marcos o MOCs relevantes.
- En marcos (`05 Marcos/`): wikilinks a los marcos padre (jerarquía).

### Propiedad `valor`
- Lista de **un único elemento** en la escala de 7 puntos (o vacía hasta ser evaluada; nunca dejar la escala entera copiada):
  `7 - Trascendente` > `6 - Excelente` > `5 - Bueno` > `4 - Aceptable` > `3 - Deficiente` > `2 - Atroz` > `1 - Nocivo`.

---

## 4. Jerarquía de Marcos (`05 Marcos/`)

```text
05 Marcos/Generales/
├── Conocimiento
│   ├── Filosofía
│   │   ├── Filosofía de la mente
│   │   └── Ética
│   ├── Ciencia
│   │   └── Neurociencia
│   └── Aprendizaje
│       └── Curriculum
└── Raíces transversales:
    ├── Compatibilismo
    ├── Economía
    ├── Sociología
    ├── Ciencia política
    └── Teoría de la elección racional
        ├── Filosofía
        ├── Economía
        ├── Sociología
        └── Ciencia política
```

---

## 5. Reglas de Enrutamiento

1. **Nunca crear archivos en la raíz** (en la raíz solo vive `AGENTS.md`).
2. **Índice y Log únicos en `AI OS/wiki/`:** Existe **un solo `index.md`** (`AI OS/wiki/index.md`) y **un solo `log.md`** (`AI OS/wiki/log.md`) en toda la bóveda. No crear índices adicionales en otras carpetas.
3. **Reflexiones y tesis en primera persona** → `01 Notas/`.
4. **Compilación enciclopédica automática del LLM** → `AI OS/wiki/` (`fuentes/`, `entidades/`, `conceptos/`, `análisis/`).
5. **Fuentes crudas externas (libros, artículos)** → `02 Referencias/`.
6. **Modelos visuales (`.excalidraw`, esquemas)** → `03 Recursos/Modelo/`.
7. **Plantillas nuevas** → `03 Recursos/Plantillas/`.
8. **Proyectos activos y MOCs de proyecto** → `04 Proyectos/` (`Inmediato/`, `Largo Plazo/`, `Toma de notas.md`).
9. **Categorías universales / disciplinas** → `05 Marcos/Generales/`.
10. **Marcos propios** → `05 Marcos/Personales/`.
11. **Archivos de sistema y skills** → `AI OS/`.

---

## 6. Herramientas y Plugins Relevantes

- **Templater:** Plantillas en `03 Recursos/Plantillas/`.
- **Dataview:** Consultas automáticas en `05 Marcos/`, `04 Proyectos/` y `00 Diario/`.
- **Excalidraw:** Modelos visuales en `03 Recursos/Modelo/`.
- **Syncthing:** Sincronización (carpeta `.stfolder/`).
- **Graph view:** Colores por tags de `Marcos/` y por path `01 Notas`.

---

## 7. Metodología OKF (Open Knowledge Format) v0.2

La bóveda se considera un **bundle OKF conformante a nivel mínimo** y puede exportarse como bundle OKF para agentes de IA.

### 7.1 Principios
- Un bundle es un directorio de archivos Markdown con frontmatter YAML.
- Un concepto = un archivo. La ruta del archivo es el ID del concepto.
- Solo `type` es obligatorio; todo lo demás es opcional o extensión.
- El formato es la interfaz: portable, diffable, legible por humanos y agentes.

### 7.2 Campos OKF
- **Obligatorio:** `type` (no vacío).
- **Recomendados:** `title`, `description`, `resource`, `tags`.
- **Provenance / trust / lifecycle:** `sources`, `generated`, `verified`, `status`, `stale_after`.
- **Extensiones permitidas:** `titulo`, `creada`, `retocada`, `reactivada`, `motivo`, `temas`, `valor`, etc.

### 7.3 Convención de enlaces
- En la bóveda se usan wikilinks `[[...]]` para Obsidian.
- En un bundle OKF exportado, los enlaces deben ser Markdown estándar bundle-relative:
  `[texto](/ruta/al/archivo.md)`.

### 7.4 Archivos reservados
- `AI OS/wiki/index.md`: único catálogo general de contenido de la bóveda (`okf_version: "0.2"` en el frontmatter).
- `AI OS/wiki/log.md`: único historial cronológico de operaciones (`## [YYYY-MM-DD] <op> | <Título>`).
- No usar estos nombres para notas de conceptos.

### 7.5 Tipos (`type`) usados en esta bóveda
- `evergreen` / `concept` (notas en primera persona en `01 Notas/`)
- `category general` / `category personal` (marcos en `05 Marcos/`)
- `map of content` / `essay` (MOCs y mini-ensayos en `04 Proyectos/`)
- `literature` / `book` (fuentes y libros en `02 Referencias/`)
- `daily note` (entradas en `00 Diario/`)
- `wiki-source` / `wiki-entity` / `wiki-concept` / `wiki-analysis` (páginas del compilador en `AI OS/wiki/`)
- `system-context` / `system-map` / `skill` (archivos en `AI OS/`)
- `literature` (notas de fuentes, artículos, videos en `02 Referencias/`)
- `book` (fichas de libros en `02 Referencias/`)
- `daily note` (entradas en `00 Diario/`)
- `essay` (mini-ensayos en `04 Proyectos/`)
- `system-context` / `system-map` / `skill` (archivos en `AI OS/`)

### 7.6 Exclusiones para Bundles OKF
No incluir en un bundle OKF:
- `.obsidian/`
- `.stfolder/`
- `.trash/`
- `03 Recursos/Plantillas/`
- Archivos no Markdown (imágenes, PDFs, `.excalidraw`, `.base`)

---

## 8. Mantenimiento

- Actualizar este mapa cuando se añadan carpetas o cambie la estructura.
- Revisar categorías huérfanas periódicamente.
- Mantener consistencia entre `temas` y enlaces reales en el cuerpo de las notas.
- Mantener el único `index.md` (en la raíz) y `log.md` actualizados sin generar `index.md` en subcarpetas.