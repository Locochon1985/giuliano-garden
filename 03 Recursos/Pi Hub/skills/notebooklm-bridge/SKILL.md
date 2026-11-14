---
name: notebooklm-bridge
description: "Trigger: notebooklm, gemini notebook, cuaderno de gemini, sincronizar fuentes con notebook, investigar y subir a notebooklm. Orchestrate web research, Google Gemini Notebook MCP source ingestion, and Obsidian Vault synthesis."
license: MIT
metadata:
  author: giuliano
  version: "1.0"
---

## Activation Contract

Use this skill when the user wants to interact with Google Gemini Notebook (NotebookLM), upload web/text sources to cloud notebooks, or bridge research between the web, NotebookLM, and the Obsidian Vault.

## Hard Rules

- Always call `list_notebooks` before creating a notebook to prevent duplicate notebooks.
- Validate source ingestion by calling `get_source` after `create_source` to confirm status (`ACTIVE`, `PROCESSING`, or `FAILED`).
- Never hardcode or expose OAuth tokens from `~/.config/gemini-notebook/credentials.json`.
- If MCP calls fail with authentication errors, instruct or run `node /home/giuliano/Projects/gemini-notebook-mcp/index.js --login`.
- When saving synthesis notes to the Vault, follow `/home/giuliano/.agents/skills/vault-curator/SKILL.md`.

## Decision Gates

| Need | Action |
| --- | --- |
| Find existing cloud notebooks | Call `list_notebooks` with `filter: "notebook.is_owner = true"` |
| Add a URL or YouTube video to a notebook | Call `create_source` with `webContent: { url }` |
| Add local Markdown or study notes to a notebook | Call `create_source` with `textContent: { content }` and `mimeType: "text/markdown"` |
| Research a topic end-to-end | Search via `web_search`/`fetch_content` -> upload via `create_source` -> write synthesis to `~/Documents/Vault/02 Referencias/Internet/` |

## Execution Steps

1. Resolve target notebook ID via `list_notebooks` (or create one via `create_notebook` if requested).
2. Gather source material (from local Vault files, user input, or `web_search` / `fetch_content`).
3. Ingest sources into the target `notebooks/{id}` via `create_source`.
4. Verify ingestion status via `get_source` or `list_sources`.
5. If requested, synthesize key findings into a Vault note in `/home/giuliano/Documents/Vault/02 Referencias/Internet/` with backlinks.

## Output Contract

Return:
- Notebook display name and `notebooks/{id}`.
- Sources added or inspected and their ingestion status (`ACTIVE` / `PROCESSING`).
- Path to any generated or updated Obsidian Vault note.

## References

- `/home/giuliano/Documents/Vault/02 Referencias/Internet/Gemini Notebook MCP.md`
- `/home/giuliano/Projects/gemini-notebook-mcp/index.js`
- `/home/giuliano/.pi/agent/mcp.json`
