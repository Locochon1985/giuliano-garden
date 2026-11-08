---
title: Overview - Descripción General de la LLM Wiki
type: system-map
related:
  - "[[index]]"
  - "[[convenciones]]"
  - "[[log]]"
  - "[[llm-wiki]]"
  - "[[Vault Map]]"
created: 2026-10-28
tags:
  - Sistema
---

# Overview — Descripción General de la LLM Wiki

> [!IMPORTANT] Propósito de `AI OS/wiki/`
> Este directorio aloja la **base de conocimientos generada y mantenida por el agente LLM**, siguiendo el patrón de `[[llm-wiki]]`. Mientras que las carpetas `01 Notas/`, `04 Proyectos/` y `05 Marcos/` contienen la producción manuscrita y curada en primera persona por Giuliano, `AI OS/wiki/` actúa como el **compilador persistente del agente** sobre todas las fuentes, entidades, conceptos y análisis de la bóveda.

---

## 1. Estructura de `AI OS/wiki/`

```text
AI OS/wiki/
├── index.md            # Índice general orientado a contenido
├── log.md              # Registro cronológico de operaciones (ingest, query, lint, schema)
├── Overview.md         # Esta página: visión general y estado de síntesis de la wiki
├── convenciones.md     # Reglas de formato, nomenclatura y preferencias de mantenimiento
├── fuentes/            # Resúmenes estructurados de materiales procesados desde 02 Referencias/
├── entidades/          # Páginas de personas (autores, pensadores), organizaciones y herramientas
├── conceptos/          # Páginas de teorías, métodos, modelos y patrones compilados por el LLM
└── análisis/           # Comparaciones interdisciplinarias, síntesis de consultas y estados de cuestión
```

---

## 2. Síntesis Temática Actual de la Bóveda

La base de conocimientos actual gira en torno a tres ejes interconectados:

1. **Filosofía de la Acción, Volición y Libertad:**
   - Tensión entre **impulsos de primer orden** y **preferencias/voliciones de segundo orden** (Harry G. Frankfurt, Ludwig von Mises) en `[[Intereses de primer y segundo orden]]` y `[[Diferencia entre un deseo y una volición]]`.
   - El problema de la *akrasia* y la división interna de la persona reflexiva en `[[La libertad como un problema]]` y `[[Carecer de libertad requiere primero la capacidad de tenerla]]`.
2. **Autoaprendizaje Deliberado y Diseño Curricular:**
   - Construcción del `[[Curriculum Personal]]` frente a la educación formal, apoyado en la vocación de estudio de A.D. Sertillanges (`[[LA VIDA INTELECTUAL]]`).
   - Selección entre contenidos **constantes** y **variables** (`[[El desafío de seleccionar contenido]]`) y estrategias de recorte epistémico (`[[Delimitar la perspectiva del curriculum]]`).
3. **Sistemas de Conocimiento y Cognición (`PKM` + `LLM Wiki`):**
   - Metodología de `[[Nota perenne]]` (Matuschak, Ahrens, Luhmann), su ciclo de salida al mundo material (`[[La prueba del árbol]]`) y las bases neurocognitivas de las redes conceptuales (`[[Comer una manzana segun la neurociencia]]`).
   - Automatización del mantenimiento mediante agentes (`[[llm-wiki]]`).

---

## 3. Cómo Interactúan `AI OS/wiki/` y el Resto de la Bóveda

| Operación | Entrada (*Input*) | Acción en `AI OS/wiki/` | Acción en el resto de la bóveda |
|---|---|---|---|
| **`[[ingest]]`** | Fuente cruda en `02 Referencias/` o `00 Diario/` | Crea/actualiza resumen en `fuentes/`, páginas de autores en `entidades/` y teorías en `conceptos/`. Actualiza `index.md` y `log.md`. | Propone o genera notas atómicas en primera persona en `01 Notas/` y vincula a `05 Marcos/`. |
| **`[[query]]`** | Pregunta del usuario | Consulta `index.md`, cruza `conceptos/`, `entidades/` y `01 Notas/`, y archiva comparaciones valiosas en `análisis/`. | Alimenta mini-ensayos en `04 Proyectos/` o nuevas semillas en `01 Notas/`. |
| **`[[lint]]`** | Estado global de la bóveda | Audita contradicciones, huérfanas, notas en `🥀sequía` y registra el pase en `log.md`. | Sugiere correcciones de YAML, enlaces cruzados y reactivaciones en `01 Notas/`. |
