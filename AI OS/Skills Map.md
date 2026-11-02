---
title: Skills Map - Índice de Skills
type: system-map
up: "[[ME]]"
related:
  - "[[Vault Map]]"
created: 2026-10-26
tags:
  - Sistema
---

# Skills Map — Índice de Skills

> [!IMPORTANT] Instrucción para Agentes de IA
> Este archivo es el índice de la carpeta `SKILLS/`. **Antes de ejecutar una tarea compleja**, revisa si existe un skill aplicable. Si existe, adopta su procedimiento estrictamente. Si no existe, ejecuta la tarea con tus capacidades generales y considera crear un skill nuevo si la tarea es recurrente.

---

## 1. Estado Actual (Arquitectura LLM Wiki Activa)

La carpeta `AI OS/SKILLS/` implementa las operaciones fundamentales del patrón **LLM Wiki** (basado en `[[llm-wiki]]`): transformar fuentes crudas e interacciones con el agente en un **artefacto de conocimiento persistente y acumulativo**, donde el humano dirige la curación y las preguntas, y el agente ejecuta la compilación, los enlaces cruzados y el mantenimiento.

---

## 2. Qué es un Skill

Un skill es un procedimiento documentado para una tarea recurrente o especializada. Debe incluir:

- **Objetivo:** qué resuelve dentro del sistema LLM Wiki.
- **Cuándo usarlo:** disparadores o señales.
- **Pasos:** secuencia clara y accionable.
- **Reglas:** restricciones, validaciones y criterios de calidad (YAML, enrutamiento, `index.md` y `log.md`).

---

## 3. Cómo Crear un Skill

1. Crea un archivo en `AI OS/SKILLS/` con nombre descriptivo: `nombre-del-skill.md`.
2. Usa la plantilla de abajo.
3. Registra el skill en la sección **4. Skills Disponibles** de este archivo.
4. Mantén el skill atómico: una capacidad u operación por archivo.

### Plantilla de Skill

```markdown
---
title: <Nombre del Skill>
type: skill
up: "[[Skills Map]]"
related:
  - "[[Vault Map]]"
created: YYYY-MM-DD
tags:
  - Skill
  - Sistema
---

# <Nombre del Skill>

## 1. Cuándo usarlo
- Señal 1

## 2. Pasos de Ejecución
1. Paso 1
2. Paso 2

## 3. Reglas y Verificación
- Regla 1
```

---

## 4. Skills Disponibles (Operaciones Nucleares LLM Wiki)

| Skill | Archivo | Estado | Descripción |
|---|---|---|---|
| **Ingest** | `[[ingest]]` (`AI OS/SKILLS/ingest.md`) | Activo | Procesa una fuente de `02 Referencias/` o `00 Diario/`, extrae conceptos hacia `01 Notas/`, enlaza con `05 Marcos/` y actualiza `index.md` y `log.md`. |
| **Query** | `[[query]]` (`AI OS/SKILLS/query.md`) | Activo | Navega `index.md` y las páginas de la wiki para responder consultas con citas y archiva los nuevos hallazgos o ensayos de vuelta en la bóveda (*compounding*). |
| **Lint** | `[[lint]]` (`AI OS/SKILLS/lint.md`) | Activo | Audita la salud de la wiki: detecta contradicciones, notas en sequía reactivables, huérfanas, enlaces fantasma y conformidad con el Schema YAML/OKF. |
| **Index & Log** | `[[index-and-log]]` (`AI OS/SKILLS/index-and-log.md`) | Activo | Reglas de mantenimiento del catálogo orientado a contenido (`index.md` único en raíz) y la bitácora cronológica parseable (`log.md`). |

---

## 5. Skills Sugeridos (Backlog Específico de Dominio)

Basados en las áreas de enfoque de `[[ME]]`:

| Skill sugerido | Propósito |
|---|---|
| `ensamblar-mini-ensayo` | Hilvanar un conjunto de notas perennes en un ensayo publicable en `04 Proyectos/` |
| `planificacion-semanal` | Revisión semanal de `00 Diario/`, notas en sequía y prioridades del semestre |
| `curar-marco` | Estructurar un marco de `05 Marcos/` en ejes argumentativos cuando supera 7 notas |

---

## 6. Reglas de Uso

1. **Consulta primero:** Antes de una tarea compleja, revisa esta lista.
2. **Adopta el procedimiento:** Si hay skill, síguelo estrictamente.
3. **Crea si falta:** Si la tarea es recurrente y no hay skill, propón crear uno.
4. **Mantén actualizado:** Al añadir un skill, actualiza la tabla de la sección 4.
5. **Un skill por archivo:** Evita mezclar capacidades no relacionadas.

---

## 7. Mantenimiento

- Revisar este índice al final de cada semana.
- Marcar skills obsoletos o en desuso.
- Mover skills maduros a una carpeta `SKILLS/estables/` si el volumen crece.
