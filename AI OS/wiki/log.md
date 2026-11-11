---
tags:
  - Sistema
---
# Vault Update Log

> Registro cronológico *append-only* siguiendo el formato parseable de `[[llm-wiki]]`: `## [YYYY-MM-DD] <ingest|query|lint|schema> | <Título>`.

## [2026-11-11] ingest | Ingesta y formateo de referencia técnica: `Solución Audio Omarchy`
* **Formateo y enrutamiento (`02 Referencias/Internet/`):** Aplicada la plantilla canónica de literatura/internet (`type: literature`, `media_type: articulo`, `tags: Fuente/Articulo`) a [`02 Referencias/Internet/Solución Audio Omarchy.md`](02%20Referencias/Internet/Solución%20Audio%20Omarchy.md), integrando síntesis ejecutiva, cuerpo técnico, reflexiones arquitectónicas sobre PipeWire/WirePlumber/ALSA y enlaces de referencia.
* **Saneamiento de raíz:** Eliminado el archivo huérfano en la raíz de la bóveda respetando la regla canónica de `AI OS/Vault Map.md`.
* **Catálogo actualizado:** Indexada la referencia en `AI OS/wiki/index.md`.

## [2026-11-03] ingest | Importación de Google Doc y creación de proyecto `Coloquio Historia de la Educacion`
* **Fuente externa procesada (`gdocs`):** Extraído el documento [RESUMEN PERSONAL Coloquio historia de la educación](https://docs.google.com/document/d/1mLpSKQ3BZEfwfNiadm2vxO-UKsQkD5VXKwe2i328bNU/edit) y resguardado en [`02 Referencias/Profesorado/Historia de la Educación/Resumen Personal Coloquio Historia de la Educación.md`](file:///c:/Users/Admin/Documents/sincronia/Vault/02%20Referencias/Profesorado/Historia%20de%20la%20Educaci%C3%B3n/Resumen%20Personal%20Coloquio%20Historia%20de%20la%20Educaci%C3%B3n.md).
* **Proyecto académico creado:** Generado [`04 Proyectos/Academia/Coloquio Historia de la Educacion.md`](file:///c:/Users/Admin/Documents/sincronia/Vault/04%20Proyectos/Academia/Coloquio%20Historia%20de%20la%20Educacion.md) (Prof. Mónica Bacarezza) estructurando el guion de exposición oral en 6 ejes diacrónicos, banco de preguntas de examen y datos difíciles.
* **Catálogo actualizado:** Indexados ambos archivos en `AI OS/wiki/index.md`.

## [2026-11-03] schema | Incorporación de Perfil Cognitivo (INTP → INTJ) y Regla de Guardia en `AI OS/ME.md`
* **Perfil Cognitivo (`1.1`):** Documentados los puntos fuertes (`Ti` axiomático, `Ne` interdisciplinario, autocrítica) y puntos débiles/sombras (meta-productividad, dispersión, parálisis por perfeccionismo e insuficiencia).
* **Objetivos Operativos (`2`):** Especificados los 4 protocolos de conversión hacia `INTJ` (`Te-Ni`): Regla del 80% (Freeze de infraestructura), límite `WIP = 1`, arquitectura anti-akrasia (bloque monástico matutino) y derecho al "borrador espantoso".
* **Directriz para Agentes (`6`):** Añadida la *Regla de Guardia Cognitiva* que obliga al agente a frenar la procrastinación en herramientas y apalancar las fortalezas analíticas hacia entregables materiales.

## [2026-11-03] schema | Sincronización del catálogo `AI OS/wiki/index.md` y actualización de `Proyecto Ensayo`
* **Auditoría de catálogo (`AI OS/wiki/index.md`):** Verificadas las 7 secciones del índice central contra la totalidad de archivos de la bóveda; actualizada la referencia de la plantilla renombrada `Proyecto Ensayo.md` (reemplazando `Mini-ensayo.md`).
* **Reglas de enrutamiento:** Añadidas reglas en Auto Note Mover para `#Proyecto/Mini-Ensayo` y `#Proyecto/Articulo` hacia `04 Proyectos/Ensayos/`.

## [2026-11-03] schema | Sincronización integral de `AI OS/Vault Map.md` con el estado real
* **Auditoría y alineación de Schema:** Actualizado `AI OS/Vault Map.md` incorporando la topología de `02 Referencias/` (`Libros/`, `Internet/`, `Audio visual/`, `Profesorado/`, `Personas/`), `04 Proyectos/` (`Academia/`, `Ensayos/`, `Sistemas/`, `Archivo/`) y la reubicación de `Toma de notas.md` en `05 Marcos/Personales/MOCs/`.
* **Nuevos esquemas YAML y tags:** Documentados los frontmatters de `diario.md`, `Proyecto Académico.md`, `Mini-ensayo.md` (ensayos), `Proyecto Sistemas.md` y `persona.md`; incorporadas reglas automáticas de Auto Note Mover y tags `#Proyecto/*`, `#Persona`.
* **Depuración OKF y regla canónica:** Limpieza de tipos duplicados en la sección 7.5 (agregados `project` y `people`) y corrección de la ruta única de `index.md` (`AI OS/wiki/index.md`).

## [2026-11-03] schema | Reestructuración de `02 Referencias/` (Libros, Internet, Audio visual, Profesorado)
* **Nueva topología en `02 Referencias/`:** Creadas las carpetas `Libros/` (`Biblioteca.base`, `LA VIDA INTELECTUAL.md`), `Internet/` (`llm-wiki.md`, `5 ESTRATEGIAS...`), `Audio visual/` (subcarpetas `Películas/` con `La invitación.md` y `Series/`), y `Profesorado/` por cátedras (`Psicología Educativa/` con `Enseñanza para la comprensión.md`).
* **Enrutamiento automático (Auto Note Mover):** Configuradas reglas para dirigir `#Libros`, `#Fuente/Pelicula`, `#Fuente/Serie` y `#Persona` a sus carpetas respectivas.
* **Sincronización:** Actualizados `AI OS/Vault Map.md` e `AI OS/wiki/index.md`.

## [2026-11-03] schema | Creación de plantilla `Proyecto Sistemas.md` y automatización de enrutamiento
* **Nueva plantilla (`03 Recursos/Plantillas/Proyecto Sistemas.md`):** Especializada para iniciativas técnicas en `04 Proyectos/Sistemas/` (AI OS, dotfiles, Neovim/NvChad, scripts y PKM), con metadatos `alcance` y `stack`, especificación técnica/DoD, snippets de configuración, fases de prueba y radar Dataview.
* **Enrutamiento y Tipado:** Añadida regla en Auto Note Mover (`#Proyecto/Sistemas` -> `04 Proyectos/Sistemas/`); registradas `alcance` y `stack` en `.obsidian/types.json`; e indexada la plantilla en `AI OS/wiki/index.md`.

## [2026-11-03] schema | Adaptación de `Mini-ensayo.md` a Proyecto Ensayo y automatización de enrutamiento
* **Evolución de plantilla (`03 Recursos/Plantillas/Mini-ensayo.md`):** Rediseñada como entorno de trabajo para la carpeta `04 Proyectos/Ensayos/` (propiedades `status`, `medio`; bloques para definición de salida, ensamble de notas del Zettelkasten, esqueleto argumentativo y checklist de pulido).
* **Enrutamiento automático (Auto Note Mover):** Configuradas reglas para mover automáticamente notas etiquetadas con `#Proyecto/Ensayo` o `#Mini-Ensayo` hacia `04 Proyectos/Ensayos/`, y `#Proyecto/Academia` hacia `04 Proyectos/Academia/`.
* **Tipado:** Registrada la propiedad `medio` en `.obsidian/types.json`.

## [2026-11-03] schema | Creación de plantilla `Proyecto Académico.md` y actualización de proyecto de Psicología
* **Nueva plantilla (`03 Recursos/Plantillas/Proyecto Académico.md`):** Estructura especializada para cursadas, parciales y exámenes finales (propiedades `materia`, `modalidad`, `fecha_entrega`; secciones de ficha de cátedra, problema/tesis, corpus bibliográfico, fases de trabajo y radar Dataview).
* **Aplicación:** Actualizada la nota [`04 Proyectos/Academia/Final de Psicologia Educativa.md`](file:///c:/Users/Admin/Documents/sincronia/Vault/04%20Proyectos/Academia/Final%20de%20Psicologia%20Educativa.md) con el nuevo estándar y la articulación teórica de Perkins y Bruner.
* **Mantenimiento:** Sincronizados tipos en `.obsidian/types.json` e indexada la plantilla en `AI OS/wiki/index.md`.

## [2026-11-03] lint | Depuración de propiedades fantasma en `.obsidian/types.json`
* **Limpieza de metadatos:** Eliminadas 32 propiedades fantasma con 0 usos en la bóveda registradas en `.obsidian/types.json` (residuos de `excalidraw-*`, propiedades en desuso `horas_sueño`, `Energía`, `fecha de inicio`, `fecha inicio`, `calificación`, `Genero` sin tilde, `rating`, `value`, `generated`).
* **Sincronización de tipos:** Conservadas y tipadas exclusivamente las 37 propiedades reales presentes en los archivos de la bóveda.

## [2026-11-03] schema | Reestructuración de `04 Proyectos/` (Opción 1) y reubicación de `Toma de notas.md`
* **Nueva arquitectura de `04 Proyectos/`:** Eliminadas las subcarpetas temporales `Inmediato/` y `Largo Plazo/`. Creadas las subcarpetas por tipo de entregable: `Academia/` (entregas universitarias), `Ensayos/` (mini-ensayos del Zettelkasten), `Sistemas/` (AI OS, dotfiles, PKM) y `Archivo/` (proyectos cerrados).
* **Migración de archivos:** Trasladado `Final de Psicologia Educativa.md` a `04 Proyectos/Academia/`. Consolidada la reubicación de `Toma de notas.md` en su entorno natural `05 Marcos/Personales/MOCs/Toma de notas.md`.
* **Actualización de Schemas:** Modificados `AI OS/Vault Map.md` y `AI OS/wiki/index.md` para reflejar la nueva topología.

## [2026-11-03] lint | Higiene de raíz, reubicación de fuentes, saneamiento de proyectos e indexación
* **Higiene de raíz:** Reubicado `Enseñanza para la comprensión.md` desde la raíz a `02 Referencias/Enseñanza para la comprensión.md`, normalizando su frontmatter al estándar `type: literature` (autores Perkins, Gardner, Bruner; tags `Fuente/Articulo`; temas `[[Aprendizaje]]` y `[[Curriculum]]`).
* **Saneamiento en `04 Proyectos/`:** Formalizado frontmatter YAML OKF v0.2 en `04 Proyectos/Inmediato/Final de Psicologia Educativa.md`; integrada consulta Dataview de *Notas candidatas a integrar* en el MOC `04 Proyectos/Toma de notas.md`.
* **Sincronización de catálogo (`AI OS/wiki/index.md`):** Indexadas la entrada diaria `[[2026-11-03]]` (`00 Diario/`), la fuente `[[Enseñanza para la comprensión]]` (`02 Referencias/`) y el proyecto activo `[[Final de Psicologia Educativa]]` (`04 Proyectos/Inmediato/`).

## [2026-11-02] lint | Saneamiento de `index.md`, eliminación de residuo `.md` y recuperación de `Toma de notas.md`
* **Limpieza de índice (`AI OS/wiki/index.md`):** Eliminada entrada rota/duplicada de `La invitación` con ruta antigua; añadido recurso visual `Gemini_Generated_Image_dk4orcdk4orcdk4o-Photoroom.png` en `03 Recursos/Modelo/`.
* **Recuperación:** Restaurado MOC operativo `04 Proyectos/Toma de notas.md` a partir del estado de caché y eliminado `.syncthing.Toma de notas.md.tmp`.
* **Higiene de raíz:** Eliminado archivo huérfano `.md` de 0 bytes.

## [2026-11-02] schema | Actualización de `index.md` y `log.md` (entradas de diario, referencias nuevas y personas)

* **Índice actualizado (`AI OS/wiki/index.md`):** sección `0. Diario (`00 Diario/`)` con las entradas `2026-10-31`, `2026-11-01`, `2026-11-02`; referencias nuevas `[[5 ESTRATEGIAS para ser más INTELIGENTE YA mismo]]`, `[[La invitación]]` (versión limpia) y fichas de personas `[[Luisina]]`, `[[Romina]]`, `[[Santiago]]`; plantilla `persona.md` añadida al listado de `03 Recursos/Plantillas/`.
* **Motivación:** sincronizar el catálogo con archivos creados/modificados recientemente (notas de personas, nuevas referencias, entradas diarias) que no estaban indexados.

## [2026-10-28] schema | Creación de estructura `AI OS/wiki/`, unificación de reglas y alineación de `literatura.md`
* **Fuente base:** [[llm-wiki]] (`02 Referencias/llm-wiki.md`).
* **Estructura creada en `AI OS/wiki/`:** Traslado de [[index]] y [[log]] hacia `AI OS/wiki/`; creación de [[Overview]], [[convenciones]] y los directorios `fuentes/`, `entidades/`, `conceptos/` y `análisis/`.
* **Frontera Operativa (Opción A) y Fuente Única de Verdad:** Reducción de [[AGENTS]] a puntero mínimo; consolidación de reglas en [[Vault Map]]; separación estricta entre `AI OS/wiki/` (compilación enciclopédica 100% del LLM) y `01 Notas/` (reflexiones exclusivas en voz de Giuliano).
* **Alineación de Plantilla (`03 Recursos/Plantillas/literatura.md`):** Sincronización de propiedades YAML (`author`, `source_title`, `media_type`, `published`, `description`, `tags: Fuente/...`) con el formato de las notas de literatura capturadas en `02 Referencias/`.
* **Skills creados en `AI OS/SKILLS/`:** [[ingest]], [[query]], [[lint]] e [[index-and-log]].

## [2026-10-27] schema | Arquitectura 05 Marcos y limpieza de plantillas
* **Estructura:** Creación de `05 Marcos/` dividida en `Generales/` (`type: category general`, tag `Marcos/General`) y `Personales/` (`type: category personal`, tag `Marcos/Personal`). Reestructuración de `04 Proyectos/` por horizonte (`Inmediato/`, `Largo Plazo/`) y traslado de `Modelo/` a `03 Recursos/Modelo/`.
* **Plantillas y Dataview:** Creación de `Categoría General.md` y `Categoría Personal.md`; corrección de `contains(temas, this.file.link)`; vaciado de la lista precargada en `valor:`; incorporación del bloque Dataview de actividad diaria en `diario.md`.
* **Política de índice único:** Definición de regla de un único `index.md` en la raíz de la bóveda (`Vault/index.md`) y eliminación de `04 Proyectos/Modelo/index.md`.

## [2026-10-26] schema | Inicialización de AI OS y migración a OKF v0.2
* **Inicialización:** Creación de `AI OS/ME.md`, `AI OS/Vault Map.md`, `AI OS/Skills Map.md`, `AGENTS.md`, `index.md` y migración de propiedades a OKF v0.2 (`type`).


## [2026-11-05] schema | Creación de Proyectos.base con vista Kanban y sincronización de 04 Proyectos

* **Base Kanban (`04 Proyectos/Proyectos.base`):** Creación de base nativa de Obsidian Bases con 4 vistas dinámicas:
  1. `Kanban (Carpetas)` (`type: kanban`, agrupado por `file.folder` con `groupOrder` fijo para `04 Proyectos/Académico`, `04 Proyectos/Ensayos`, `04 Proyectos/Sistemas` y `04 Proyectos/Archivo`, permitiendo arrastrar tarjetas entre columnas para mover archivos físicamente entre carpetas).
  2. `Kanban (Estado)` (`type: kanban`, agrupado por `note.status`).
  3. `Tabla General` (`type: table`, agrupado por `file.folder`).
  4. `Galería por Carpeta` (`type: cards`, agrupado por `file.folder`).
* **Sincronización de Enrutamiento e Índice:** Actualización de `.obsidian/plugins/auto-note-mover/data.json` (`#Proyecto/Académico` → `04 Proyectos/Académico`), `AI OS/Vault Map.md` y `AI OS/wiki/index.md` reflejando `Coloquio Didáctica II.md`, el archivo de `Coloquio Historia de la Educacion.md` (`calificación: 10`), `2026-11-04.md` y la ubicación de `04 Proyectos/Sistemas/Modelo/`.


## [2026-11-05] schema | Reestructuración de Modelo de Realidad.md con protocolo I/O y DoD ejecutable

* **Ajuste de DoD y Operatividad Te-Ni:** Sustitución de la definición de terminado abstracta por un DoD medible de 3 condiciones (Protocolo I/O formal, 2 casos de prueba empíricos y glosario operativo).
* **Especificación de Protocolo I/O:** Formalización de la interfaz Entrada (síntomas observables) $\rightarrow$ Procesamiento (3 coordenadas: AQAL, Dilts, Assagioli) $\rightarrow$ Salida (diagnóstico y vector de agencia) con 3 reglas de derivación.
* **Articulación de Corpus Canónico:** Vinculación directa con notas troncales de la bóveda (`[[Intereses de primer y segundo orden]]`, `[[Diferencia entre un deseo y una volición]]`, `[[Carecer de libertad requiere primero la capacidad de tenerla]]`, `[[La libertad como un problema]]`, `[[ME]]`, `[[Conocimiento]]`).


## [2026-11-05] schema | Creación de plantilla lugar.md y configuración de 02 Referencias/Lugares

* **Plantilla (`03 Recursos/Plantillas/lugar.md`):** Formato Templater para fichas de lugares (`type: place`, tag `#Lugar`, campos `location`, `ciudad`, `pais`, `tipo_lugar`, `coordenadas`, `enlaces`, `description`, `temas`, `valor`).
* **Enrutamiento y Tipos:** Creación del directorio `02 Referencias/Lugares/`, regla de enrutamiento automático en `.obsidian/plugins/auto-note-mover/data.json` para `#Lugar`, registro de propiedades en `.obsidian/types.json`, y actualización del esquema en `AI OS/Vault Map.md` e `AI OS/wiki/index.md`.


## [2026-11-05] schema | Sincronización de esquema de lugar en Vault Map y tipos de Obsidian

* **Alineación de Frontmatter de Lugares:** Actualización de la especificación de `lugar.md` en `AI OS/Vault Map.md` eliminando `ciudad`, `pais` y `coordenadas` para reflejar la simplificación canónica realizada por el usuario.
* **Higiene de Tipos:** Purga de propiedades huérfanas en `.obsidian/types.json` (`ciudad`, `pais`, `coordenadas`), conservando únicamente `tipo_lugar` y `enlaces`.


## [2026-11-05] schema | Creación de Notas.base con tablero Kanban de madurez atómica

* **Base de Madurez (`01 Notas/Notas.base`):** Implementación de base nativa de Obsidian Bases con 4 vistas:
  1. `Tablero de Madurez` (`type: kanban`, agrupado por `note.tags` con orden fijo para `atómica/🌱semilla`, `atómica/🌿arbusto`, `atómica/🌲perenne`, `atómica/🥀sequía` y `atómica/🪾marchita`, permitiendo arrastrar tarjetas entre columnas para evolucionar la madurez de las notas directamente en el frontmatter).
  2. `Tablero por Valor` (`type: kanban`, agrupado por `note.valor` de 1 a 7).
  3. `Tabla General` (`type: table`, agrupada por madurez).
  4. `Galería de Notas` (`type: cards`, agrupada por madurez).
* **Sincronización:** Actualizado `AI OS/Vault Map.md` y catalogado en `AI OS/wiki/index.md` junto con `[[La constante polarización del mundo]]`.


## [2026-11-05] schema | Creación de Referencias.base para gestión de fuentes externas

* **Base de Referencias (`02 Referencias/Referencias.base`):** Configuración de Obsidian Bases con 5 vistas dinámicas:
  1. `Kanban (Carpetas)` (`type: kanban`, agrupado por `file.folder` con columnas ordenadas: `Libros`, `Internet`, `Películas`, `Series`, `Personas`, `Lugares`, `Historia de la Educación`, `Psicología Educativa`, permitiendo clasificar y mover fuentes físicamente por arrastre).
  2. `Kanban (Tipo de Fuente)` (`type: kanban`, agrupado por `note.type`: `book`, `literature`, `video`, `movie`, `github page`, `people`, `place`).
  3. `Kanban (Estado de Ingesta)` (`type: kanban`, agrupado por `note.status`: `Pendiente`, `Procesado`).
  4. `Tabla General` (`type: table`, agrupada por carpeta).
  5. `Galería Visual` (`type: cards`, con soporte para `note.Portada` y ajuste `imageFit: contain`).
* **Sincronización:** Actualizado `AI OS/Vault Map.md`, indexado en `AI OS/wiki/index.md` junto con `[[Residencia de mujeres]]`.


## [2026-11-06] ingest | Resguardo de Síntesis de Defensa en 02 Referencias/Profesorado y completado de Coloquio Didáctica II

* **Nueva referencia bibliográfica y de cátedra (`02 Referencias/Profesorado/Didáctica II/Síntesis de Defensa Didáctica II.md`):** Formalizado el documento de fundamentación curricular y metodológica de Giuliano Monzón para Filosofía (Eje II: Aristóteles), articulando las 4 secuencias de clase y el encuadre pedagógico de los 7 componentes didácticos (`type: literature`, tags `#Resumen`, `#Didáctica`).
* **Completado de proyecto académico (`04 Proyectos/Archivo/Coloquio Didáctica II.md`):** Estructurado en profundidad el guion de defensa, la articulación de coherencia epistemológica (isomorfismo contenido-método), el marco teórico crítico de cátedra (Salinas Fernández, Edelstein, Davini, Sacristán, Litwin, Anijovich, Sanjurjo, Celman) y el registro de aprobación (`calificación: 10`).
* **Sincronización de catálogo (`AI OS/wiki/index.md`):** Indexada la nueva referencia en `02 Referencias/Profesorado/` y actualizada la ruta de `Coloquio Didáctica II` en `04 Proyectos/Archivo/`.
