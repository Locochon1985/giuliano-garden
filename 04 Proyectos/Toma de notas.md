---
titulo: Toma de notas
type: map of content
creada: 2026-10-07
retocada: 2026-10-08
tags:
  - MOCs
temas:
  - "[[Curriculum]]"
valor:
  - 6 - Excelente
---
# Toma de notas

> [!note] **Propósito del MOC**
> Métodos, filosofías y estructuras entorno a la escritura de notas para gestionar mi auto aprendizaje.

---
## Ejes

### 1. Conceptos Fundamentales (Temas)
* Nota atómica inicial o definición clave
* [[]]

### 2. Principios y Argumentos (Notas Evergreen)
* [[Nota perenne]]

### 3. Aplicaciones y Casos Prácticos
* [[]]

## Vista General

> [!warning] 🌾 Notas en Sequía / Latentes
> *Conceptos estancados o en pausa a la espera de nuevas fuentes, resolución de contradicciones o maduración reflexiva.*
```dataview
TABLE 
  temas as "Tema",
  valor as "Valor",
  retocada as "Último retoque",
  (date(today) - file.mtime) as "Días en sequía"
FROM "01 Notas"
WHERE contains(string(file.tags), "sequía")
SORT file.mtime asc
```

> [!tip] 🔄 Notas Reactivadas Recientes
```dataview
TABLE 
  reactivada as "Fecha Reactivación",
  motivo as "Motivo",
  valor as "Valor"
FROM "01 Notas"
WHERE reactivada != null
SORT reactivada desc
```

## Fuentes y Referencias
* 
