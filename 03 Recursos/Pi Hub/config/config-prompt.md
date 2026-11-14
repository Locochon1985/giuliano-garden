---
description: Manage Pi Hub agents, skills, MCP servers, and orchestrator rules in ~/Documents/Vault/03 Recursos/Pi Hub
---

You are managing the centralized **Pi Hub** configuration inside the Obsidian Vault.

## Single Source of Truth (`/home/giuliano/Documents/Vault/03 Recursos/Pi Hub`)

All Pi configurations, subagent definitions, skills, MCP servers, and orchestrator rules live in `/home/giuliano/Documents/Vault/03 Recursos/Pi Hub/`:

- **Core & Runtime Configs (`config/`):**
  - `config/settings.json` (symlinked from `~/.pi/agent/settings.json`)
  - `config/subagents.json` (symlinked from `~/.pi/agent/subagents.json`)
  - `config/mcp.json` (symlinked from `~/.pi/agent/mcp.json`)
  - `config/bridge.json` (symlinked from `~/.pi/agent/antigravity-bridge/config.json`)
  - `config/config-prompt.md` (symlinked from `~/.pi/agent/prompts/config.md`)
- **Subagents (`agents/`):**
  - `agents/gentle-ai-explore.md`
  - `agents/gentle-ai-worker.md`
  - `agents/gentle-ai-verify.md`
  - `agents/vault-agent.md`
- **User Skills (`skills/`):**
  - `skills/<skill-name>/SKILL.md` (symlinked from `~/.agents/skills/<skill-name>`)
- **Orchestrator Contracts (`orchestrator/`):**
  - `orchestrator/orchestrator.md`
  - `orchestrator/orchestrator-delegation.md`

## Mandatory Workflow

1. Inspect current state first (check `Pi Hub/` and `~/.pi/agent/`).
2. Edit files inside `/home/giuliano/Documents/Vault/03 Recursos/Pi Hub/` (and keep `~/.pi/agent/AGENTS.md` / `~/.pi/agent/extensions/` aligned when changing global rules or local extensions).
3. For bridge performance presets (`fast` / `deep`), use `/home/giuliano/.local/bin/pi-ctl mode fast | deep` if available, or edit `Pi Hub/config/bridge.json`.
4. Copy updated agents (`agents/*.md`) to `~/.pi/agent/agents/` if they are not symlinked.

$ARGUMENTS
