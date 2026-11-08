---
title: Query - Consulta Sintética y Retroalimentación a la Wiki
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

# Query — Consulta Sintética y Retroalimentación a la Wiki

> [!IMPORTANT] Propósito en LLM Wiki
> Responder preguntas navegando primero la wiki compilada en lugar de redescubrir fragmentos desde cero. **Las respuestas valiosas no deben perderse en el historial del chat**: se archivan de vuelta en la bóveda como nuevas páginas o ensayos, haciendo que cada exploración capitalice conocimiento.

---

## 1. Cuándo usarlo

- Cuando el usuario hace una pregunta conceptual, comparativa o estratégica sobre los temas de su bóveda.
- Cuando pide cruzar dos disciplinas (ej. neurociencia y filosofía de la voluntad, o teoría curricular y materias de Ciencias de la Educación).
- Cuando pide preparar un esquema, tabla comparativa o borrador de ensayo a partir de sus notas.

---

## 2. Pasos de Ejecución

### Paso 1: Navegación Guiada por el Índice
1. Lee primero [`AI OS/wiki/index.md`](file:///c:/Users/Admin/Documents/sincronia/Vault/AI%20OS/wiki/index.md) y [`AI OS/wiki/Overview.md`](file:///c:/Users/Admin/Documents/sincronia/Vault/AI%20OS/wiki/Overview.md) para localizar todas las páginas pertinentes en `AI OS/wiki/` (`fuentes/`, `entidades/`, `conceptos/`, `análisis/`), `01 Notas/`, `05 Marcos/`, `04 Proyectos/` y `02 Referencias/`.
2. Lee los archivos identificados y sigue sus `[[wikilinks]]` salientes para capturar el contexto argumentativo completo.

### Paso 2: Síntesis con Citas y Pensamiento Crítico
1. Construye la respuesta conectando explícitamente las páginas de la bóveda mediante `[[wikilinks]]` y enlaces clicables.
2. Aplica pensamiento crítico (según `[[ME]]`): señala si hay huecos en la argumentación actual de la bóveda, supuestos no demostrados o tensiones interesantes entre dos notas.

### Paso 3: Retroalimentación Acumulativa (*Compounding Step*)
1. Evalúa si la respuesta generada produjo conocimiento nuevo digno de persistir:
   - **Una comparación, tabla o síntesis analítica** $\rightarrow$ Archívala en `AI OS/wiki/análisis/` (`type: wiki-analysis`).
   - **Una conexión o tesis propia en primera persona** $\rightarrow$ Propón o crea una nota atómica (`type: evergreen`) en `01 Notas/`.
   - **Una síntesis de varias notas maduras para publicar** $\rightarrow$ Propón o crea un mini-ensayo (`type: essay`, plantilla `Mini-ensayo.md`) en `04 Proyectos/Inmediato/` o `04 Proyectos/Largo Plazo/`.
2. Si se archiva una nueva página producto de la consulta, actualiza [`AI OS/wiki/index.md`](file:///c:/Users/Admin/Documents/sincronia/Vault/AI%20OS/wiki/index.md) y registra la operación en [`AI OS/wiki/log.md`](file:///c:/Users/Admin/Documents/sincronia/Vault/AI%20OS/wiki/log.md):
   ```markdown
   ## [YYYY-MM-DD] query | <Tema o pregunta consultada>
   * **Páginas consultadas:** [[Nota A]], [[Nota B]]
   * **Artefacto generado en la wiki:** [[Nueva Nota o Ensayo]] (si aplica)
   ```

---

## 3. Reglas
- Prioriza siempre las definiciones y argumentos que ya están compilados en `01 Notas/` antes de recurrir a conocimiento externo genérico; cuando agregues conocimiento externo, distínguelo claramente para que pueda convertirse en nueva evidencia.
- Mantén cero relleno (*no fluff*): directo a la síntesis y las conexiones.
