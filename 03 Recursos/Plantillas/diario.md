---
fecha: <% tp.file.title %>
type: daily note
tags:
  - Diario
temas:
valor:
---
# **📅 <% tp.file.title %>**


---
## 📝Registro

- <% tp.date.now("HH:mm") %> - <% tp.file.cursor() %>



---
## 🌿 Actividad en la Bóveda Hoy

```dataview
LIST type + " (" + file.folder + ")"

WHERE (file.cday = this.file.day OR file.mday = this.file.day)

WHERE file.name != this.file.name AND !contains(file.folder, "AI OS")

```