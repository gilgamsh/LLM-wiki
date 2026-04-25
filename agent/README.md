# `agent/` Layer

`agent/` stores source-grounded, agent-friendly derivatives of raw source packages.

## Purpose

- make sources easier for LLM tools and agents to read than PDFs alone
- preserve the original source language
- provide stable per-source metadata and reference structure

## Standard Layout

Each source package should use this layout:

- `agent/<source-slug>/source.md`
- `agent/<source-slug>/metadata.json`
- `agent/<source-slug>/refs.json`
- `agent/<source-slug>/images/` (optional)

`source.md` should preserve a source-grounded transcription of the package content while keeping clear boundaries such as `## Source: paper.pdf`. Keep inline citation or reference markers when the source exposes them. If the source uses hyperlink-style citations or ambiguous anchor text, normalize them into inline markers like `[ref:1]` or `[ref:1,2]` that resolve through the sibling `refs.json`. Do not preserve page-by-page scaffolding unless it is needed for understanding.

`metadata.json` should describe the package, point back to `raw/<source-slug>/`, and inventory the source files.

`refs.json` should store bibliography-style references and footnotes. Keep the file present even when the arrays are empty, and keep its `ref_id` values stable so `source.md` markers remain traceable over time.

Use the templates in `agent/_templates/` when processing a new source.
