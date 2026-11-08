---
title: Convenciones - Reglas Internas del Subsistema AI OS/wiki
type: system-map
related:
  - "[[Vault Map]]"
  - "[[ME]]"
  - "[[Skills Map]]"
  - "[[Overview]]"
created: 2026-10-28
tags:
  - Sistema
---

# Convenciones — Reglas Internas de `AI OS/wiki/`

> [!IMPORTANT] Fuente Única de Verdad (`[[Vault Map]]`)
> Todas las reglas generales de la bóveda (estructura global de carpetas, enrutamiento, tipos OKF y convenciones de `01 Notas/`) viven en **`[[Vault Map]]`**. Este archivo solo especifica el comportamiento interno de las 4 subcarpetas automáticas del compilador LLM dentro de `AI OS/wiki/`.

---

## 1. Frontera Operativa: `AI OS/wiki/` vs. `01 Notas/`

* **`AI OS/wiki/` es territorio 100% automático del agente LLM:** Aquí el agente compila resúmenes objetivos de textos (`fuentes/`), fichas de autores/herramientas (`entidades/`), desgloses teóricos o académicos (`conceptos/`) y comparativas (`análisis/`) en tercera persona enciclopédica.
* **`01 Notas/` es territorio exclusivo de la voz de Giuliano:** Ningún resumen automático o apunte enciclopédico va a `01 Notas/`. Solo pasan a `01 Notas/` las tesis y reflexiones curadas en primera persona con juicio propio.

---

## 2. Esquema YAML de las 4 Subcarpetas de `AI OS/wiki/`

```yaml
---
titulo: <Nombre de la página>
type: wiki-source | wiki-entity | wiki-concept | wiki-analysis
creada: YYYY-MM-DD
retocada: YYYY-MM-DD
sources:
  - "[[Archivo en 02 Referencias]]"
temas:
  - "[[Marco en 05 Marcos]]"
tags:
  - Wiki/Fuente | Wiki/Entidad | Wiki/Concepto | Wiki/Analisis
---
```

| Subcarpeta | `type` | `tags` | Qué compila el agente aquí |
|---|---|---|---|
| `fuentes/` | `wiki-source` | `Wiki/Fuente` | Desglose exhaustivo de capítulos, argumentos y citas de materiales de `02 Referencias/`. |
| `entidades/` | `wiki-entity` | `Wiki/Entidad` | Fichas de autores (ej. Frankfurt, Mises, Sertillanges), escuelas o herramientas y su postura en la bóveda. |
| `conceptos/` | `wiki-concept` | `Wiki/Concepto` | Definiciones teóricas, modelos y conceptos de estudio (incluyendo cátedras de Ciencias de la Educación). |
| `análisis/` | `wiki-analysis` | `Wiki/Analisis` | Cruces entre autores, tablas comparativas y síntesis producidas durante consultas (`[[query]]`). |
