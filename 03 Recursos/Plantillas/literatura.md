---
titulo: "<% tp.file.title %>"
type: literature
author:
  - "<% await tp.system.prompt("Autor(es)") %>"
source_title: "<% await tp.system.prompt("URL o título de la fuente") %>"
media_type:
  - <% await tp.system.suggester(["articulo", "podcast", "video", "conferencia"], ["articulo", "podcast", "video", "conferencia"]) %>
published: <% await tp.system.prompt("Fecha de publicación (YYYY-MM-DD, opcional)") %>
creada: <% tp.date.now("YYYY-MM-DD") %>
retocada:
description: "<% await tp.system.prompt("Descripción breve") %>"
tags:
  - <% await tp.system.suggester(["Fuente/Articulo", "Fuente/Podcast", "Fuente/Video", "Fuente/Conferencia"], ["Fuente/Articulo", "Fuente/Podcast", "Fuente/Video", "Fuente/Conferencia"]) %>
status: Pendiente
temas:
valor:
---
# <% tp.file.title %>

## Síntesis


---

## Reflexiones
> "Cita textual"
> — *Página / Capítulo*

* **Reflexión personal:** Análisis propio sobre la cita anterior.


## Referencias
