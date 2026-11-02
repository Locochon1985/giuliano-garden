---
title: Index and Log - Catálogo de Contenido y Bitácora Cronológica
type: skill
up: "[[Skills Map]]"
related:
  - "[[Vault Map]]"
  - "[[llm-wiki]]"
  - "[[ingest]]"
  - "[[query]]"
  - "[[lint]]"
created: 2026-10-28
tags:
  - Skill
  - Sistema
---

# Index and Log — Catálogo de Contenido y Bitácora Cronológica

> [!IMPORTANT] Propósito en LLM Wiki
> Según el patrón `[[llm-wiki]]`, dos archivos especiales dentro de `AI OS/wiki/` permiten al agente y al usuario navegar y auditar la base de conocimiento sin depender de infraestructura externa de embeddings (RAG): [`AI OS/wiki/index.md`](file:///c:/Users/Admin/Documents/sincronia/Vault/AI%20OS/wiki/index.md) (orientado a contenido) y [`AI OS/wiki/log.md`](file:///c:/Users/Admin/Documents/sincronia/Vault/AI%20OS/wiki/log.md) (orientado al tiempo).

---

## 1. Reglas de [`AI OS/wiki/index.md`](file:///c:/Users/Admin/Documents/sincronia/Vault/AI%20OS/wiki/index.md) (Orientado a Contenido)

1. **Ubicación única:** Existe **un solo `index.md` en toda la bóveda**, ubicado exclusivamente en `AI OS/wiki/index.md` con `okf_version: "0.2"` en su frontmatter YAML. Nunca crear archivos `index.md` adicionales en otras carpetas.
2. **Estructura orientada a contenido:** Cataloga todas las páginas de `AI OS/wiki/` (`Overview.md`, `convenciones.md`, `fuentes/`, `entidades/`, `conceptos/`, `análisis/`) y las páginas de la bóveda (`01 Notas/`, `02 Referencias/`, `05 Marcos/`, `04 Proyectos/`) con:
   - Enlace al archivo (`[[Nombre]]`).
   - Tipo (`type`) o estado de madurez.
   - **Resumen de una línea** que sintetice su tesis o propósito.
3. **Uso por el agente:** Antes de responder cualquier consulta (`[[query]]`) o procesar una fuente (`[[ingest]]`), el agente lee `AI OS/wiki/index.md` para ubicar las páginas relevantes y luego entra en ellas.
4. **Mantenimiento:** Se actualiza automáticamente en cada operación `ingest`, `query` (cuando genera páginas nuevas) o cambio estructural.

---

## 2. Reglas de [`AI OS/wiki/log.md`](file:///c:/Users/Admin/Documents/sincronia/Vault/AI%20OS/wiki/log.md) (Orientado a Cronología)

1. **Naturaleza *append-only* (cronológico inverso o directo consistente):** Registra qué ocurrió en la bóveda y cuándo (ingestas, consultas que generaron notas, auditorías `lint` y cambios de schema).
2. **Formato de encabezado parseable:** Cada entrada debe comenzar con un encabezado `H2` estricto:
   ```markdown
   ## [YYYY-MM-DD] <operación> | <Título o alcance>
   ```
   Donde `<operación>` es uno de:
   - `ingest` — Procesamiento de una fuente cruda e integración en la wiki.
   - `query` — Consulta sintética que produjo o actualizó páginas en la wiki.
   - `lint` — Pase de auditoría de salud, limpieza o corrección de inconsistencias.
   - `schema` — Evolución de la arquitectura en `AI OS/`, `AGENTS.md` o plantillas.
3. **Ventaja técnica:** Este formato permite filtrar el historial instantáneamente con herramientas de línea de comandos (`Select-String "^## \[" log.md`).
