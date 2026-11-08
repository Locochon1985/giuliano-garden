---
title: Ingest - Ingesta y Compilación de Fuentes
type: skill
up: "[[Skills Map]]"
related:
  - "[[Vault Map]]"
  - "[[llm-wiki]]"
  - "[[index-and-log]]"
created: 2026-10-28
tags:
  - Skill
  - Sistema
---

# Ingest — Ingesta y Compilación de Fuentes

> [!IMPORTANT] Propósito en LLM Wiki
> En lugar de usar las fuentes solo para consulta puntual (RAG), esta operación lee una fuente cruda nueva y **compila su conocimiento dentro de la wiki persistente**, creando o actualizando notas atómicas, conectando marcos y dejando trazabilidad en el índice y el log.

---

## 1. Cuándo usarlo

- Cuando se agrega un nuevo libro, artículo (vía Obsidian Web Clipper), PDF o video en `02 Referencias/`.
- Cuando el usuario vuelca apuntes de clase (Ciencias de la Educación) o notas fugaces en `00 Diario/` y pide procesarlos.
- Cuando el usuario pide explícitamente: *"Ingesta esta fuente"*, *"Procesa el archivo X"* o *"Extrae notas de..."*.

---

## 2. Regla sobre las Fuentes Crudas (Capa Raw)

- Los textos originales capturados en `02 Referencias/` (artículos recortados, PDFs, citas textuales) son la **fuente de verdad inmutable**: no borres ni distorsiones el contenido original del autor.
- Puedes completar los metadatos YAML faltantes de la fuente (`type: literature` o `type: book`, `autor`, `temas`, `status: Procesado`, `valor`) y añadir tu síntesis en las secciones correspondientes si usa la plantilla `literatura.md` o `libro.md`.

---

## 3. Pasos de Ejecución

### Paso 1: Lectura y Diálogo de Enfoque
1. Lee la fuente completa en `02 Referencias/` (y revisa `index.md` para conocer qué conceptos relacionados ya existen en la bóveda).
2. **Modo guiado (por defecto):** Presenta al usuario las tesis centrales, conceptos candidatos a extraer y cómo conectan o chocan con sus notas actuales en `01 Notas/` antes de crear archivos masivos.
3. **Modo directo/lote:** Si el usuario pide crear las notas directamente, procede al Paso 2 sin pausa intermedia.

### Paso 2: Integración en la Capa Wiki (Frontera `AI OS/wiki/` vs. `01 Notas/`)
1. **Compilar automáticamente en `AI OS/wiki/` (Territorio 100% del Agente LLM):**
   - Crea o actualiza la página de desglose objetivo de la fuente en `AI OS/wiki/fuentes/` (`type: wiki-source`).
   - Crea o actualiza las páginas de autores, organizaciones o herramientas relevantes en `AI OS/wiki/entidades/` (`type: wiki-entity`).
   - Crea o actualiza los conceptos teóricos o académicos en `AI OS/wiki/conceptos/` (`type: wiki-concept`).
2. **Curación hacia `01 Notas/` (Territorio exclusivo de la voz de Giuliano):**
   - No vuelques apuntes enciclopédicos crudos en `01 Notas/`.
   - Si la fuente aporta evidencia nueva, un contraejemplo o destraba una nota de `01 Notas/` que estaba en `#atómica/🥀sequía`, actualiza esa nota (`retocada:` / `reactivada:` + `motivo:`).
   - Solo crea nuevas notas en `01 Notas/` (`type: evergreen` o `concept`) para tesis y reflexiones curadas en **primera persona** con la voz y ejemplos de Giuliano, enlazando hacia el respaldo teórico de `AI OS/wiki/` y los marcos de `05 Marcos/`.

### Paso 3: Actualización de `AI OS/wiki/index.md` y `AI OS/wiki/log.md`
1. Añade la fuente y las páginas/notas creadas o modificadas en [`AI OS/wiki/index.md`](file:///c:/Users/Admin/Documents/sincronia/Vault/AI%20OS/wiki/index.md) con su resumen de una línea (siguiendo el skill `[[index-and-log]]`).
2. Agrega una entrada al inicio de [`AI OS/wiki/log.md`](file:///c:/Users/Admin/Documents/sincronia/Vault/AI%20OS/wiki/log.md) con el prefijo parseable:
   ```markdown
   ## [YYYY-MM-DD] ingest | <Título de la fuente>
   * **Fuente procesada:** [[<Título de la fuente>]]
   * **Notas creadas:** [[Nota A]], [[Nota B]]
   * **Notas actualizadas / reactivadas:** [[Nota C]]
   ```

---

## 4. Lista de Verificación de Calidad
- [ ] El frontmatter YAML de cada nota nueva incluye `type`, `titulo`, `creada`, `retocada`, `tags`, `temas` (plural) y `valor` (un solo ítem o vacío).
- [ ] Ninguna nota se creó en la raíz de la bóveda.
- [ ] Existen enlaces bidireccionales (`[[wikilinks]]`) entre la fuente y las notas atómicas.
- [ ] Se actualizaron `index.md` y `log.md`.
