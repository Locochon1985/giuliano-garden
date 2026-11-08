---
titulo: <% tp.file.title %>
type: map of content
creada: <% tp.date.now("YYYY-MM-DD") %>
retocada:
tags:
  - MOCs
temas:
valor:
---

# <% tp.file.title %>

> **Propósito del MOC**: Breve descripción del dominio o tema central que abarca este mapa. ¿Qué preguntas busca responder esta red de conocimiento?

---
## Ejes

### 1. Conceptos Fundamentales 
* Nota atómica inicial o definición clave
* [[]]

### 2. Principios y Argumentos 
* [[]]

### 3. Aplicaciones y Casos Prácticos
* [[]]



## Vista General

> [!abstract] Notas candidatas a integrar
> *Listado automático de notas etiquetadas o vinculadas a este tema que aún no han sido organizadas manualmente en las secciones superiores.*
```dataview
TABLE valor, tags, retocada as "Última revisión"
WHERE contains(temas, this.file.link) OR contains(file.outlinks, this.file.link)
WHERE file.name != this.file.name
SORT file.mtime desc
```




## Fuentes y Referencias
* [[]]