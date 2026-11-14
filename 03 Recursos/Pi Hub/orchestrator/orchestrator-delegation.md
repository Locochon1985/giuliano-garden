# Orchestrator — Delegation Card (lazy-loaded)

Bind this to the parent Pi session only, on delegation or routing triggers. Read `orchestrator-delegation-reference.md` only when a situation names it.

## Handoff

Write clear, self-contained task instructions for the subagent, including exact file paths, requirements verbatim, and any relevant skill paths under `## Skills to load before work`.

- Preserve exact user quotes, UI copy, error messages, filenames, commands, and domain terms in their original language.
- For Vault notes (`~/Documents/Vault`) and Spanish HTML visual explainers (`concept-visualizer`), instruct the subagent to write user-facing content in natural Spanish.

#### Active Delegation Triggers

Proactively delegate to keep the parent session's context window clean and responsive:

1. **Ask:** an open product or design decision gets one focused user question; stop and wait.
2. **Explore (`gentle-ai-explore`):** delegate web research (`perplexity_search`, `web_search`, `fetch_content`), academic/documentation lookups, or local codebase/Vault mapping requiring more than 2-3 file reads.
3. **Vault & NotebookLM (`vault-agent`):** delegate creating, formatting, routing, or ingesting notes in `/home/giuliano/Documents/Vault` (`02 Referencias/`, `AI OS/wiki/`, `01 Notas/`, `index.md`, `log.md`) and syncing sources with Google Gemini NotebookLM (`mcp__gemini_notebook__*`).
4. **Implement (`gentle-ai-worker`):** delegate multi-file or substantive single-file code/script implementation, Omarchy desktop customization, interactive HTML visual explainers (`/home/giuliano/.agents/skills/concept-visualizer/SKILL.md`), or any 2+ independent units that can run in parallel. Only quick 1-2 file edits that are already understood stay inline.
5. **Verify (`gentle-ai-verify`):** delegate test-suite execution, build/typecheck verification, high-risk change verification, crash diagnostics (`diagnose-crash`), Pi ecosystem health checks (`pi-hygiene`), or read-only Vault audits (`lint.md`).
6. **Track:** for multi-session or large features, maintain `odd/tasks/<feature>.md`, its Engram mirror, and `todo` (`orchestrator-memory.md`).

## Launch

- Pass resolved skill paths under `## Skills to load before work` when applicable (`orchestrator-skills.md`).
- When launching `gentle-ai-worker` in a repository, optionally include `## Allowed edit surfaces` with relative paths/globs to prevent parallel writers from overlapping.
- Background (policy on, `subagent_run` present): omit `mode` (or pass `mode: "background"`) with a 3-6 word `label`; it returns a task id at once. Launch independent subagents in parallel in the same turn, then end the turn and resume automatically on `gentle-agents.result` completion messages; never sleep or poll `subagent_status`.
- Do not pass `model` to subagents unless the user explicitly asks; subagents inherit the active session model by default.

#### Key Learnings closing block

Subagents (`gentle-ai-explore`, `gentle-ai-worker`, `gentle-ai-verify`, `vault-agent`) may close their final response with a `## Key Learnings` block of 1–3 numbered standalone factual sentences when they discover durable, reusable technical insights.
