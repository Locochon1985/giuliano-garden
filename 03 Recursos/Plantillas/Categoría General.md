---
titulo: <% tp.file.title %>
type: category general
creada: <% tp.date.now("YYYY-MM-DD") %>
retocada:
tags:
  - Marcos/General
temas:
valor:
---
# <% tp.file.title %>
Breve descripción del dominio o tema central 

## Vista General

> [!abstract] Notas candidatas a integrar
> *Listado automático de notas etiquetadas o vinculadas a este tema que aún no han sido organizadas manualmente en las secciones superiores.*
```dataview
TABLE valor, tags, retocada as "Última revisión"
WHERE contains(temas, this.file.link) OR contains(file.outlinks, this.file.link)
WHERE file.name != this.file.name
SORT file.mtime desc
```




## 📚 Fuentes y Referencias
* [[]]