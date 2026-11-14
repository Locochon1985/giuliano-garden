---
name: pi-hygiene
description: "Trigger: auditar pi, limpiar pi, mantenimiento de pi, optimizar pi, revisar salud de pi, podar agentes o skills. Audit, diagnose, and streamline the local Pi Coding Agent configuration, subagents, skills, and Engram memory."
license: MIT
metadata:
  author: giuliano
  version: "2.0"
---

## Activation Contract

Use this skill when asked to diagnose Pi errors, reduce system prompt overhead, audit installed extensions/skills/subagents, synchronize Pi Hub changes, or verify the health of Engram and the Antigravity Bridge.

## Single Source of Truth & Management CLI

- **Pi Hub Directory:** `/home/giuliano/Documents/Vault/03 Recursos/Pi Hub/` is the canonical single source of truth for configurations (`config/`), subagents (`agents/`), and user skills (`skills/`).
- **Global Rules & Local Extensions:** Lightweight workflow rules live in `~/.pi/agent/AGENTS.md`; standalone TUI extensions (`todo.ts`, `ask-user.ts`, `codegraph.ts`, `learnharness/`, `quiz.ts`, `md-log.ts`, `visual-tools/`, `custom-header.ts`, `prompt-snippets/`, `tts.ts`) live in `~/.pi/agent/extensions/`.
- **Bridge Mode:** Switch `Pi Hub/config/bridge.json` between `fast` (`flash` / `medium`) and `deep` (`argon` / `high`).

## Hard Rules

- Preserve Pi's minimalist philosophy: favor removing or disabling unused layers over adding complexity.
- Always edit configurations, agents, and skills inside `/home/giuliano/Documents/Vault/03 Recursos/Pi Hub/` (and `~/.pi/agent/AGENTS.md` / `~/.pi/agent/extensions/` for global rules/extensions).
- Never delete active user configurations or skills without explicit confirmation.
- Keep the 4 operational subagents (`gentle-ai-explore`, `gentle-ai-worker`, `gentle-ai-verify`, and `vault-agent`) in `Pi Hub/agents/` aligned with `~/.pi/agent/agents/`.
- Keep `~/.pi/agent/subagents.json` (`Pi Hub/config/subagents.json`) configured with `default_mode: "background"` and empty `model_profiles: {}` (inheriting the active session model).

## Decision Gates

| Symptom or Goal | Action |
| --- | --- |
| Full health check or drift detection | Inspect `Pi Hub/config/`, `~/.pi/agent/extensions/`, `~/.agents/skills/`, and `mem_doctor` |
| Startup noise or unwanted banners | Check `quietStartup: "header"` in `Pi Hub/config/settings.json` |
| High token usage or slow responses | Audit active packages in `settings.json`, MCP servers in `mcp.json`, or switch `bridge.json` to `fast` |
| Deep architectural reasoning needed | Switch `bridge.json` to `deep` (`argon` + `high` thinking) |
| Engram memory errors | Run `mem_doctor` and verify `engram` binary on `127.0.0.1:7437` |

## Execution Steps

1. Inspect `Pi Hub/config/{settings.json,bridge.json,mcp.json,subagents.json}`, `~/.pi/agent/AGENTS.md`, and `~/.pi/agent/extensions/`.
2. Run `mem_doctor` if memory diagnostics are requested.
3. Apply any requested changes cleanly and sync `Pi Hub/agents/` -> `~/.pi/agent/agents/` if agents changed.
4. Report findings ordered by impact.

## Output Contract

Return:
- Output summary of `pi-ctl status` (Core, Antigravity Bridge, Engram, MCP servers, Subagents, and Skills).
- Files modified in `Pi Hub`, skills enabled/disabled, or sync actions performed.
- Estimated context/token savings or resolved warnings.

## References

- `/home/giuliano/Documents/Vault/03 Recursos/Pi Hub/`
- `/home/giuliano/.local/bin/pi-ctl`
- `/home/giuliano/Documents/Vault/02 Referencias/Internet/Configuración y Ecosistema de Pi Agent.md`
- `/home/giuliano/Documents/Vault/02 Referencias/Internet/Guía de Uso y Adaptabilidad de Pi Agent.md`
