---
name: skill-creator
description: "Trigger: /skill-creation, skill creation, skill creator, create skill, new skill. Create concise, LLM-first skills with valid frontmatter for Pi and local agents."
metadata:
  version: "1.0"
---

## Activation Contract

Use this skill when creating or updating a reusable AI skill.

Create a skill when:
- a workflow or convention is reused across sessions;
- generic agent behavior needs specific constraints or decision gates;
- templates, schemas, or examples make future execution more reliable.

Do not create a skill for a one-off task, generic documentation, or rules that belong in code or tests.

## Hard Rules

- A skill is an LLM runtime contract, not human-facing tutorial prose.
- Keep `SKILL.md` concise (target 180–450 tokens, max ~1000).
- Use imperative instructions and concrete decision gates.
- Frontmatter `description` must be a single YAML-safe line starting with `Trigger: ...` followed by what the skill does.
- Put templates, schemas, or example payloads in `assets/`.
- Put longer rationale, edge cases, or deep docs in `references/`.
- Place user-wide skills in `~/.agents/skills/<skill-name>/SKILL.md` (or `~/.pi/agent/skills/<skill-name>/SKILL.md`) and project-specific skills in `.pi/skills/<skill-name>/SKILL.md`.

## Decision Gates

| Need | Action |
| --- | --- |
| Small reusable workflow | Create `<skill-name>/SKILL.md` only |
| Templates, schemas, fixtures | Add `<skill-name>/assets/` |
| Longer edge cases or reference docs | Add `<skill-name>/references/` |
| Existing skill already covers it | Update the existing skill instead |

## Execution Steps

1. Inspect `~/.agents/skills/`, `~/.pi/agent/skills/`, and any project `.pi/skills/` to confirm the skill does not duplicate an existing one.
2. Choose a kebab-case `name` matching the primary trigger.
3. Create the directory structure:

```text
<skill-name>/
├── SKILL.md
├── assets/       # optional
└── references/   # optional
```

4. Write frontmatter in this shape:

```yaml
---
name: <skill-name>
description: "Trigger: <phrases users or agents will say>. <What this skill does>."
metadata:
  version: "1.0"
---
```

5. Structure the body with clear, scannable sections: `Activation Contract`, `Hard Rules`, `Decision Gates`, `Execution Steps`, `Output Contract`, and `References` (only if supporting files exist).
6. Verify YAML frontmatter is valid and `SKILL.md` stays within the token budget.

## Output Contract

Return:
- Files created or modified (exact paths).
- Whether a new skill was created or an existing one updated.
- Any `assets/` or `references/` files added.
