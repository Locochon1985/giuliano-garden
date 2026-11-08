---
fecha: <% tp.file.title %>
type: daily note
tags:
  - Diario
estado_animo:
horas_sueño:
claridad_mental:
energia:
tiempo_estudio:
---
# 📅 <% tp.file.title %>

---

## 📝 Registro

- <% tp.date.now("HH:mm") %> - <% tp.file.cursor() %>

---
## Temas sin cerrar

- pregunta

---
## Acciones para proyectos

- Tarea concreta — [[]]

---
## 🌿 Actividad en la Bóveda Hoy

```dataview
LIST type + " (" + file.folder + ")"
WHERE (file.cday = this.file.day OR file.mday = this.file.day)
WHERE file.name != this.file.name AND !contains(file.folder, "AI OS")
```
