---
title: Lint - Auditoría de Salud y Consistencia de la Wiki
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

# Lint — Auditoría de Salud y Consistencia de la Wiki

> [!IMPORTANT] Propósito en LLM Wiki
> Las bases de conocimiento humanas se abandonan cuando el costo de mantenimiento supera su valor. Esta operación delega al agente el mantenimiento estructural, semántico y taxonómico de la bóveda para mantener el costo de curación cercano a cero.

---

## 1. Cuándo usarlo

- Cuando el usuario pide *"Analiza el estado de la bóveda"*, *"Evalúa mis cambios"*, *"Haz un lint"* o en la revisión semanal.
- Después de una sesión grande de ingesta o reorganización de carpetas.

---

## 2. Dimensiones de Auditoría (Los 6 Chequeos)

### 1. Contradicciones y Tensiones Conceptuales
- Revisa si afirmaciones en notas recientes de `01 Notas/` contradicen o superan tesis de notas anteriores.
- Si una nota quedó refutada u obsoleta, sugiere pasarla a `atómica/🪾marchita` y enlazarla a la nota que la supera.

### 2. Reactivación de Notas en Sequía (`#atómica/🥀sequía`)
- Revisa si las nuevas fuentes en `02 Referencias/` o las notas recientes ofrecen el marco o evidencia que faltaba en notas marcadas con `atómica/🥀sequía` (o con `Tesis Central` incompleta, como `[[La prueba del árbol]]`).

### 3. Huérfanas, Enlaces Fantasma y Enlaces Cruzados Faltantes
- **Páginas huérfanas:** Detecta notas en `01 Notas/` que no tienen ningún enlace entrante ni `temas` asignados.
- **Enlaces fantasma recurrentes:** Identifica `[[wikilinks]]` que apuntan a páginas aún inexistentes (ej. `[[Vencer la fricción de la acción deliberada]]`, `[[La actividad intelectual debe ver sus frutos en el mundo material]]`) y prioriza cuáles merecen convertirse en nota semilla.
- **Conexiones implícitas:** Detecta pares de notas que hablan del mismo fenómeno sin enlazarse entre sí.

### 4. Conformidad con el Schema (`AGENTS.md` y `Vault Map.md`)
- Verifica que no haya archivos sueltos o vacíos en la raíz (fuera de `AGENTS.md`, `index.md` y `log.md`).
- Verifica el frontmatter YAML en todas las notas:
  - Presencia obligatoria de `type` con valores válidos (`evergreen`, `concept`, `category general`, `category personal`, `map of content`, `literature`, `book`, `daily note`, `essay`, `system-context`, `system-map`, `skill`).
  - Uso de `temas` en plural tanto en el YAML como en consultas Dataview (`contains(temas, this.file.link)`).
  - Propiedad `valor` con un único ítem seleccionado (o vacía), nunca con los 7 valores copiados de la plantilla.
  - Redacción en primera persona en `01 Notas/`.
- Verifica que exista **un único `index.md`** ubicado en `AI OS/wiki/index.md` y un único `log.md` en `AI OS/wiki/log.md`.

### 5. Sincronización de `AI OS/wiki/index.md`
- Comprueba que todas las notas y páginas existentes en `AI OS/wiki/` y en el resto de la bóveda estén catalogadas en [`AI OS/wiki/index.md`](file:///c:/Users/Admin/Documents/sincronia/Vault/AI%20OS/wiki/index.md) y que no queden enlaces rotos a archivos movidos o eliminados.

### 6. Propuesta de Investigación y Producción
- Sugiere nuevas preguntas detonantes para `Preguntas.md`, fuentes bibliográficas para cubrir vacíos o qué conjunto de notas maduras ya está listo para ensamblarse en un `Mini-ensayo` dentro de `04 Proyectos/`.

---

## 3. Registro en `AI OS/wiki/log.md`
Al finalizar un pase de Lint (especialmente si se aplicaron correcciones), añade al inicio de [`AI OS/wiki/log.md`](file:///c:/Users/Admin/Documents/sincronia/Vault/AI%20OS/wiki/log.md):
```markdown
## [YYYY-MM-DD] lint | Auditoría de salud de la bóveda
* **Hallazgos / Correcciones:** <Resumen breve de lo auditado y corregido>
```
