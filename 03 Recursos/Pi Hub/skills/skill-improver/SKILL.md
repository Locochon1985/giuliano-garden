---
name: skill-improver
description: "Trigger: improve skills, audit skills, refactor skills, skill quality. Audit and upgrade existing LLM-first skills."
metadata:
  version: "1.0"
---

## Activation Contract

Use this skill when auditing, refactoring, normalizing, or improving existing `SKILL.md` files. Use `skill-creator` when creating a brand-new skill.

## Hard Rules

- Treat the existing `SKILL.md` as the source of truth; preserve author intent, critical constraints, activation semantics, and output requirements.
- Default to audit-only. Modify files only when the user explicitly asks to apply improvements.
- Never delete meaningful rules silently; move long explanations, examples, templates, or schemas into local `references/` or `assets/`.
- Do not invent triggers, policies, or domain rules. Flag ambiguous or conflicting rules for human review.

## Decision Gates

| Situation | Action |
| --- | --- |
| Missing or invalid frontmatter | Fix `name` and single-line quoted `description` with `Trigger:` |
| Skill reads like a tutorial | Convert to imperative runtime instructions; move background to `references/` |
| Body exceeds ~1000 tokens | Preserve core rules; offload examples and edge cases to supporting files |
| Branching logic buried in prose | Convert to a compact decision table |
| Rules conflict or intent is unclear | Report the issue; do not rewrite automatically |

## Execution Steps

1. Scan target skill directories (`~/.agents/skills/*/SKILL.md`, `~/.pi/agent/skills/*/SKILL.md`, or `.pi/skills/*/SKILL.md`) for the requested skills.
2. For each skill, audit:
   - Frontmatter validity (`name`, single-line `description` with triggers).
   - Conciseness and token budget.
   - Actionability (imperative steps, clear decision gates, explicit output contract).
   - Supporting file hygiene (`assets/` and `references/` actually linked and used).
3. Return an audit report grouped by skill with severity (`high`, `medium`, `low`) and concrete proposed changes.
4. If the user asked to apply changes, edit only clear improvements, preserve all core intent, and list exact modified paths.

## Output Contract

Return:
- Skills audited and exact file paths.
- Issues found, grouped by severity.
- Files modified (if apply mode was requested).
- Ambiguities requiring human decision.
