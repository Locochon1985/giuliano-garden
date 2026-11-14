---
name: concept-visualizer
description: "Trigger: explicar en HTML, diseño en HTML para explicar, visualizar nota, infografía interactiva, esquematizar concepto visualmente. Transform complex Markdown notes or mental models into self-contained interactive HTML visual explainers."
license: MIT
metadata:
  author: giuliano
  version: "1.0"
---

## Activation Contract

Use this skill when the user asks to visually explain, diagram, or transform a Vault note, study topic, or technical architecture into an interactive HTML page or visual artboard.

## Hard Rules

- Read the source note or concept in full before designing; preserve axiomatic rigor and exact terminology.
- Build 100% self-contained HTML files (inline CSS and vanilla JS) with zero fragile external JS dependencies.
- Default to dark-mode palettes aligned with the system aesthetic (Kanagawa Wave/Dragon `#1f1f28`, `#dcd7ba`, `#7e9cd8`, `#dca561` or Omarchy theme tokens).
- Replace walls of text with visual primitives: interactive flowcharts, comparison matrices, collapsible mental-model cards, step-by-step tabs, and progress trackers.
- Keep all user-facing copy in natural Spanish unless the source requires English technical terms.

## Decision Gates

| Need | Action |
| --- | --- |
| Multi-direction exploration on the Gentle canvas | Use `gentle_design` (`design/<slug>/01-*.html`) and open on canvas |
| Standalone interactive explainer file | Write a single self-contained `.html` file at the requested path |
| Academic or philosophical study note | Include axiom cards, dialectical comparisons, and active-recall toggles |
| System or software architecture note | Include interactive data-flow diagrams, layer toggles, and command copy blocks |

## Execution Steps

1. Read the target note or specification and extract its core thesis, taxonomy, flows, and contrasts.
2. Select the delivery mode (`gentle_design` artboards under `design/<slug>/` vs standalone `.html`).
3. Structure the layout with clear visual hierarchy: Hero Synthesis -> Interactive Architecture/Flow -> Deep-Dive Cards -> Actionable Takeaways.
4. Implement responsive CSS Grid/Flexbox and lightweight vanilla JS interactivity (tabs, filters, expandable nodes).
5. Open or refresh the canvas via `gentle_design` (`action: "open"`) when using `design/<slug>/`.

## Output Contract

Return:
- Path(s) to the generated HTML file(s).
- Visual primitives and interactive controls included.
- Canvas URL if opened via `gentle_design`.

## References

- `/home/giuliano/design/second-brain-for-doing/`
- `/home/giuliano/.pi/agent/themes/omarchy-system.json`
