---
name: vault-curator
description: "Trigger: bóveda, vault, obsidian, nota de referencia, nota atómica, ingestar en vault, auditar bóveda, plantilla literatura. Curate, route, format, and audit notes in ~/Documents/Vault following the AI OS and LLM Wiki schema."
license: MIT
metadata:
  author: giuliano
  version: "1.0"
---

## Activation Contract

Use this skill whenever creating, moving, formatting, or auditing notes inside `/home/giuliano/Documents/Vault`, regardless of the current working directory.

## Hard Rules

- Treat `/home/giuliano/Documents/Vault/AI OS/Vault Map.md` as the single source of truth for folder routing, YAML frontmatter, tags, and naming. Read it before structural writes.
- Respect the 3-layer boundary:
  - `02 Referencias/`: Immutable external sources (`type: literature`, `book`, `place`, `people`).
  - `AI OS/wiki/`: 3rd-person encyclopedic synthesis maintained by the agent (`fuentes/`, `entidades/`, `conceptos/`, `análisis/`).
  - `01 Notas/`: 1st-person atomic Zettelkasten notes written in Giuliano's voice (`type: evergreen | concept`).
- Never create files in the Vault root without explicit instructions.
- Preserve Templater syntax (`<% ... %>`) when inspecting or editing templates in `03 Recursos/Plantillas/`.
- Enforce cognitive guardrails from `AI OS/ME.md`: prioritize tangible deliverables over meta-productivity loops.

## Decision Gates

| Need | Action |
| --- | --- |
| New external article, video, or technical spec | Write to `02 Referencias/Internet/` using the `literature` YAML schema |
| Full source ingestion into the LLM Wiki | Read and execute `/home/giuliano/Documents/Vault/AI OS/SKILLS/ingest.md` |
| Deep synthesis query across the Vault | Read and execute `/home/giuliano/Documents/Vault/AI OS/SKILLS/query.md` |
| Vault health audit (orphans, broken links, YAML) | Read and execute `/home/giuliano/Documents/Vault/AI OS/SKILLS/lint.md` |
| Catalog or chronological log update | Read and execute `/home/giuliano/Documents/Vault/AI OS/SKILLS/index-and-log.md` |

## Execution Steps

1. Read `/home/giuliano/Documents/Vault/AI OS/Vault Map.md` (and `AI OS/ME.md` if tone or scope is ambiguous).
2. Determine the target layer and subfolder (`00 Diario`, `01 Notas`, `02 Referencias`, `04 Proyectos`, `05 Marcos`, or `AI OS/wiki`).
3. Apply the exact YAML frontmatter schema for the note type (`titulo`, `type`, `creada`, `retocada`, `tags`, `temas`, `valor`).
4. Connect the note with existing concepts or MOCs using valid `[[wikilinks]]`.
5. Verify Markdown structure and YAML syntax by reading back the header.

## Output Contract

Return:
- Absolute path of the created or updated Vault note(s).
- Target layer and YAML `type` applied.
- Wikilinks connected (`[[...]]`).
- Whether `AI OS/wiki/index.md` or `log.md` was updated.

## References

- `/home/giuliano/Documents/Vault/AGENTS.md`
- `/home/giuliano/Documents/Vault/AI OS/ME.md`
- `/home/giuliano/Documents/Vault/AI OS/Vault Map.md`
- `/home/giuliano/Documents/Vault/AI OS/Skills Map.md`
