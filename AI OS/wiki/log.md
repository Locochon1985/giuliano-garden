---
tags:
  - Sistema
---
# Vault Update Log

> Registro cronológico *append-only* siguiendo el formato parseable de `[[llm-wiki]]`: `## [YYYY-MM-DD] <ingest|query|lint|schema> | <Título>`.

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
