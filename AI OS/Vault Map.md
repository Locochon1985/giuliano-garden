---
title: Vault Map - Mapa de la Bóveda
type: system-map
related:
  - "[[Skills Map]]"
  - "[[ME]]"
  - "[[Vault Map]]"
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
├── 01 Notas/                        # Notas atómicas y conceptos propios (Zettelkasten) + Notas.base
├── 02 Referencias/                  # Fuentes externas inmutables clasificadas por medio + Referencias.base
│   ├── Audio visual/                # Cine, teatro, series y documentales (Películas/, Series/)
│   ├── Internet/                    # Artículos web, videos de YouTube, especificaciones
│   ├── Libros/                      # Fichas de libros leídos y Biblioteca.base
│   ├── Lugares/                     # Fichas de lugares, sedes y espacios (type: place)
│   ├── Personas/                    # Fichas de personas y relaciones
│   └── Profesorado/                 # Bibliografía de cátedra por materia (Psicología Educativa/, Didáctica II/)
├── 03 Recursos/                     # Infraestructura y recursos de soporte
│   ├── Anexos/                      # Archivos adjuntos
│   └── Plantillas/                  # Plantillas Templater
├── 04 Proyectos/                    # Proyectos activos organizados por tipo de entregable + Proyectos.base
│   ├── Académico/                   # Entregables universitarios, cursadas y exámenes finales
│   ├── Ensayos/                     # Mini-ensayos y producción escrita del Zettelkasten
│   ├── Sistemas/                    # AI OS, bóveda, modelos (Modelo/) y dotfiles
│   └── Archivo/                     # Proyectos concluidos para trazabilidad histórica
├── 05 Marcos/                       # Mapas conceptuales, categorías de dominio y MOCs
│   ├── Generales/                   # Disciplinas y dominios universales (Marcos/General)
│   └── Personales/                  # Marcos propios (Marcos/Personal) y MOCs (Toma de notas.md)
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
- **Contenido:** Fuentes externas inmutables clasificadas por medio y origen.
- **Subcarpetas:**
  - `Audio visual/` (`Películas/`, `Series/`) → Obras cinematográficas, teatro, series y documentales (`La invitación.md`).
  - `Internet/` → Artículos web, videos, recortes y especificaciones (`llm-wiki.md`, `5 ESTRATEGIAS...`).
  - `Libros/` → Fichas bibliográficas con portada y metadatos (`LA VIDA INTELECTUAL.md`, `Biblioteca.base`).
  - `Lugares/` → Fichas de lugares geográficos, instituciones, cafeterías y residencias relevantes (`type: place`, tag `Lugar`).
  - `Personas/` → Fichas de personas significativas (`Luisina.md`, `Romina.md`, `Santiago.md`).
  - `Profesorado/` → Bibliografía obligatoria y apuntes de cátedra organizados por materia (`Psicología Educativa/`, `Historia de la Educación/`, `Didáctica II/`).
- **Enrutamiento:** Toda fuente externa o material de estudio va en su subcarpeta correspondiente.

### `03 Recursos/`
- **Contenido:** Infraestructura de soporte del sistema de notas y modelos visuales.
- **Subcarpetas:**
  - `Anexos/` → archivos auxiliares y adjuntos.
  - `Modelo/` → esquemas y modelos visuales (`.excalidraw`, imágenes).
  - `Plantillas/` → Plantillas Templater (`atómica.md`, `Categoría General.md`, `Categoría Personal.md`, `diario.md`, `libro.md`, `literatura.md`, `lugar.md`, `moc.md`, `persona.md`, `Preguntas.md`, `Proyecto Académico.md`, `Proyecto Ensayo.md`, `Proyecto Sistemas.md`).
- **Enrutamiento:** Plantillas, modelos gráficos y recursos reutilizables.

### `04 Proyectos/`
- **Contenido:** Proyectos activos, iniciativas y producción con entregable final, clasificados por dominio.
- **Subcarpetas:**
  - `Académico/` → entregables universitarios, cursadas, exámenes parciales y finales.
  - `Ensayos/` → mini-ensayos, artículos y publicaciones destiladas del Zettelkasten.
  - `Sistemas/` → infraestructura técnica, AI OS, scripts, Neovim/NvChad y PKM.
  - `Archivo/` → proyectos concluidos para memoria y trazabilidad histórica.
- **Enrutamiento:** Proyectos accionables y producción con entregable.

### `05 Marcos/`
- **Contenido:** Índices dinámicos, categorías de dominio y mapas de contenido (MOCs).
- **Subcarpetas:**
  - `Generales/` → disciplinas universales (`type: category general`, tag `Marcos/General`).
  - `Personales/` y `Personales/MOCs/` → marcos conceptuales propios (`type: category personal`, tag `Marcos/Personal`) y mapas de contenido permanentes (`type: map of content`, tag `MOCs`, ej. `Toma de notas.md`).
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
3. **Fichas de Libros (`02 Referencias/Libros/` — plantilla `libro.md` / QuickAdd Book Search):**
   - Requieren `type: book` y `tags: Libros` para aparecer en `02 Referencias/Libros/Biblioteca.base` (que lee la propiedad `Portada`).
4. **Fichas de Personas (`02 Referencias/Personas/` — plantilla `persona.md`):**
   ```yaml
   ---
   nombre: <Nombre>
   aliases:
     - <Alias>
   cumpleaños: YYYY-MM-DD
   relación: Pareja | Madre | Amigo | Docente
   correos:
     - <Email>
   fuente:
   type: people
   tags:
     - Persona
   ---
   ```
5. **Fichas de Lugares (`02 Referencias/Lugares/` — plantilla `lugar.md`):**
   ```yaml
   ---
   nombre: <Nombre del lugar>
   type: place
   tags:
     - Lugar
   aliases:
   location: <Dirección o URL>
   tipo_lugar:
     - Estudio | Institución | Residencia | Café | Naturaleza | Viaje
   enlaces:
   creada: YYYY-MM-DD
   retocada: YYYY-MM-DD
   temas:
   description: <Descripción breve>
   valor:
   ---
   ```
6. **Notas Diarias (`00 Diario/` — plantilla `diario.md`):**
   ```yaml
   ---
   fecha: YYYY-MM-DD
   type: daily note
   tags:
     - Diario
   estado_animo: 1-10
   horas_sueño: N
   ---
   ```
6. **Proyectos Activos (`04 Proyectos/`):**
   - **Académico (`04 Proyectos/Académico/` — `Proyecto Académico.md`):**
     ```yaml
     ---
     titulo: <Título>
     type: project
     materia: <Asignatura>
     modalidad: Examen Final | Parcial | Monografía | Coloquio Final
     fecha_entrega: YYYY-MM-DD
     status: Planificación | En curso | Revisión | Procesado
     creada: YYYY-MM-DD
     retocada: YYYY-MM-DD
     tags:
       - Proyecto/Académico
     temas:
       - "[[Marco en 05 Marcos]]"
     calificación:
     ---
     ```
   - **Ensayos (`04 Proyectos/Ensayos/` — `Proyecto Ensayo.md`):**
     ```yaml
     ---
     titulo: <Título>
     type: essay
     status: Borrador | En redacción | Revisión | Publicado
     medio: Substack | Blog | Redes | Divulgación
     creada: YYYY-MM-DD
     retocada: YYYY-MM-DD
     tags:
       - Mini-Ensayo
       - Proyecto/Ensayo
     temas:
     valor:
     ---
     ```
   - **Sistemas (`04 Proyectos/Sistemas/` — `Proyecto Sistemas.md`):**
     ```yaml
     ---
     titulo: <Título>
     type: project
     status: En curso | En pruebas | Estable
     alcance: Bóveda | Entorno SO | IA
     stack:
       - Neovim
       - Obsidian
       - PowerShell
     creada: YYYY-MM-DD
     retocada: YYYY-MM-DD
     tags:
       - Proyecto/Sistemas
     temas:
     ---
     ```
7. **Páginas del Compilador LLM (`AI OS/wiki/`):**
   - Ver detalles de las 4 subcarpetas en `[[convenciones]]` (`type: wiki-source | wiki-entity | wiki-concept | wiki-analysis`).

### Tags
- **Madurez atómica (con emoji):** `atómica/🌱semilla`, `atómica/🌿arbusto`, `atómica/🌲perenne`, `atómica/🪾marchita`, `atómica/🥀sequía`.
- **Marcos y Mapas:** `Marcos/General`, `Marcos/Personal`, `MOCs`.
- **Proyectos:** `Proyecto/Académico`, `Proyecto/Ensayo`, `Proyecto/Mini-Ensayo`, `Proyecto/Articulo`, `Proyecto/Sistemas`, `Mini-Ensayo`.
- **Fuentes y Referencias:** `Articulo`, `Video`, `Podcast`, `Pelicula`, `Serie`, `Conferencia`, `Libros`, `Persona`.
- **Registros y Sistema:** `Diario`, `Sistema`, `Skill`.
- **Subsistema Wiki (`AI OS/wiki/`):** `Wiki/Fuente`, `Wiki/Entidad`, `Wiki/Concepto`, `Wiki/Analisis`.

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
6. **Modelos visuales (`.excalidraw`, esquemas)** → `04 Proyectos/Sistemas/Modelo/`.
7. **Plantillas nuevas** → `03 Recursos/Plantillas/`.
8. **Proyectos activos y entregables** → `04 Proyectos/` (`Académico/`, `Ensayos/`, `Sistemas/`, `Archivo/`, visualizados en `04 Proyectos/Proyectos.base`). MOCs permanentes van a `05 Marcos/Personales/MOCs/`.
9. **Categorías universales / disciplinas** → `05 Marcos/Generales/`.
10. **Marcos propios** → `05 Marcos/Personales/`.
11. **Archivos de sistema y skills** → `AI OS/`.

---

## 6. Herramientas y Plugins Relevantes

- **Templater:** Automatización de creación de notas mediante plantillas dinámicas en `03 Recursos/Plantillas/`.
- **Dataview:** Consultas automáticas en `05 Marcos/`, `04 Proyectos/` y `00 Diario/`.
- **Auto Note Mover:** Enrutamiento automático por tags hacia `01 Notas/`, `02 Referencias/` (`Libros/`, `Audio visual/`, `Personas/`), `04 Proyectos/` (`Académico/`, `Ensayos/`, `Sistemas/`) y `05 Marcos/Generales/`.
- **Obsidian Bases:** Vistas de bases de datos tipo Kanban/tabla/tarjetas (`01 Notas/Notas.base`, `02 Referencias/Referencias.base`, `02 Referencias/Libros/Biblioteca.base`, `04 Proyectos/Proyectos.base`).
- **Excalidraw:** Modelos visuales conceptuales en `04 Proyectos/Sistemas/Modelo/`.
- **Syncthing:** Sincronización continua peer-to-peer (directorio `.stfolder/`).
- **Graph view:** Colores por tags de `Marcos/` y por ruta `01 Notas/`.

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
- `evergreen` / `concept` (notas atómicas y permanentes en `01 Notas/`)
- `category general` / `category personal` (marcos en `05 Marcos/`)
- `map of content` (MOCs permanentes en `05 Marcos/Personales/MOCs/`)
- `project` (proyectos en `04 Proyectos/Academia/` y `04 Proyectos/Sistemas/`)
- `essay` (mini-ensayos en `04 Proyectos/Ensayos/`)
- `literature` / `book` (fuentes, literatura y libros en `02 Referencias/`)
- `people` (fichas de personas en `02 Referencias/Personas/`)
- `place` (fichas de lugares en `02 Referencias/Lugares/`)
- `daily note` (entradas en `00 Diario/`)
- `wiki-source` / `wiki-entity` / `wiki-concept` / `wiki-analysis` (páginas del compilador en `AI OS/wiki/`)
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
- Mantener el único `index.md` (`AI OS/wiki/index.md`) y `log.md` (`AI OS/wiki/log.md`) actualizados sin generar archivos de índice en subcarpetas.