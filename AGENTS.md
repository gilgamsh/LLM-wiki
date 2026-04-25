# LLM-wiki Agent Contract

This repo is a maintained knowledge vault. Treat it as a working knowledge base, not a loose file dump.

`AGENTS.md` is the canonical contract; `CLAUDE.md` is a symlink. Edit `AGENTS.md` directly — do not replace the symlink.

## Hard Rules

- Keep the three content layers: `raw/`, `agent/`, `wiki/`.
- Use `raw/<source-slug>/` as the canonical source ID everywhere else.
- Do not rename, move, rewrite, delete, or reorganize existing `raw/<source-slug>/` content unless explicitly asked.
- Use `raw/inbox-DO-NOT-INGEST/` only as a loose collection inbox for newly gathered materials that have not been organized into canonical source folders yet.
- When organizing collected materials, promote each distinct work into its own canonical `raw/<source-slug>/` folder; do not leave canonical sources loose in the inbox.
- Group same-work materials in the same canonical folder when they belong to one source package, such as a paper, appendix, slides, or supplemental notes for the same work.
- For `qmd-wiki.query`, do not use hyphenated words or `-term` syntax in `vec` or `hyde` queries. Use `lex` for exact matches and exclusions.

## Source Slugs

New source slugs must:

- be lowercase kebab-case
- end with a language suffix: `-en`, `-zh`, ...
- follow the recommended pattern: `<class>-<topic>-<lang>`

Suggested class prefixes (optional, but helps future search and maintenance):

- `paper-`: academic paper
- `report-`: longer report / survey / roadmap
- `post-`: blog post / engineering article
- `repo-`: codebase as a source package
- `talk-`: slides / talk / lecture
- `spec-`: specification or standard
- `dataset-`: benchmark or dataset documentation

## Agent Package Shape

Each `agent/<source-slug>/` package should follow:

- `source.md`: source-grounded transcription with explicit per-file boundaries like `## Source: paper.pdf`
- `metadata.json`: package metadata and source-file inventory
- `refs.json`: extracted references/footnotes when available
- `images/`: optional images referenced by `source.md`

Templates live under `agent/_templates/`.

## Wiki Layout

- `wiki/` is English-only.
- `wiki/sources/<source-slug>.md` is the English per-source page for that slug.
- `wiki/notes/` holds cross-source notes.
- In `wiki/notes/`, include frontmatter `type: entity|concept|comparison|overview|synthesis`.
- `wiki/meta/` is for generated maintenance reports; it is not wiki content.
- Keep `wiki/index.md` as the wiki content catalog.
- `slides/` holds source-grounded presentation markdown (a fourth content surface, searched by QMD as `wiki-slides`). `view/` holds rendered views; do not edit unless asked.

## Edit Scope

- Keep edits focused on the current task and directly relevant wiki fallout.
- For source-specific work, start from `agent/<source-slug>/`, `wiki/sources/<source-slug>.md`, relevant `wiki/notes/*.md`, and `wiki/index.md`.
- During ingest or analysis, update related notes, links, and syntheses when the source adds, corrects, contradicts, or significantly sharpens existing wiki content.
- Write reusable comparisons, syntheses, and cross-source insights into `wiki/` when they are source-backed and likely to be reused.
- Do not refactor folder structure, rename slugs, or reorganize unrelated notes unless explicitly asked.

## Workflows

### Quality Control

1. Use [resource-quality-policy.md](resource-quality-policy.md) as the reference for source-quality scoring and triage.
2. Record `canonical`, `support-only`, `monitor`, and `skip` judgments in review queues or other operational review files.
3. Keep quality scores and mini-reviews out of `agent/metadata.json` and public wiki source pages.
4. Treat transfer-track sources as candidates only when they provide mechanisms transferable to the focus area.

### Collect and Organize

1. Drop newly gathered files into `raw/inbox-DO-NOT-INGEST/`.
2. Group by work identity rather than file type; promote each distinct work into `raw/<source-slug>/`, and keep same-work papers, appendices, slides, and supplements together.
3. Start ingest only after the canonical raw folder exists.

### Ingest

1. Read the source from `raw/<source-slug>/`.
2. Create or update `agent/<source-slug>/source.md` in the inherited source language with inline citation traceability.
3. Fill `## Authors and Affiliation` in `agent/<source-slug>/source.md` from `raw/<source-slug>/`. Source-grounded only; no cross-source synthesis.
4. Create or update `agent/<source-slug>/metadata.json` and `refs.json`.
5. Create or update `wiki/sources/<source-slug>.md` in English.
6. Update materially affected `wiki/notes/*.md` and `wiki/index.md`.

### Query

1. Read `wiki/index.md` first.
2. Use the MCP `qmd-wiki.query` tool (available in both Codex and Claude Code) to discover relevant wiki pages; never use `scripts/qmd-wiki query "<question>"` for discovery unless MCP is unavailable.
3. Use MCP `qmd-wiki.get` for one exact page and MCP `qmd-wiki.multi_get` for multiple exact pages after discovery, or when exact paths are known.
4. Use `scripts/qmd-wiki get <path>` or `scripts/qmd-wiki multi-get <pattern>` only as CLI fallback, or when validating the CLI wrapper/index behavior.
5. For MCP `qmd-wiki.get`, use returned paths or collection-relative paths such as `sources/<slug>.md` and `notes/<name>.md`.
6. For MCP `qmd-wiki.multi_get`, comma-separated lists must be collection-relative without leading `wiki/`, such as `sources/<slug>.md,notes/<name>.md`; `wiki/sources/...` and `wiki/notes/...` may be reported missing.
7. Raise `maxBytes` or fetch large pages separately when needed, and read `agent/` when wiki pages are insufficient or source-language grounding matters.
8. If you write reusable analysis back into `wiki/notes/`, also update `wiki/index.md`.
9. Do not create generic question-answer pages unless the user explicitly asks.

### Long-Running Command Monitoring

- When asked to run, monitor, and possibly stop a long-running command, start it with `tty=true` so `Ctrl-C` can be sent through the active session.
- Prefer session polling and `Ctrl-C` over `pgrep`, `ps`, or `pkill`; macOS process-list commands may be blocked by the agent sandbox.
- If a non-TTY command must be stopped and session input is unavailable, request escalation before using process-list or kill commands.

## Repo-Local Search

- `scripts/qmd-wiki` is the only supported CLI `qmd` entrypoint for this repo. It keeps config and SQLite state under `.qmd/`.
- Codex loads the repo-local MCP server from `.codex/config.toml` when started in this repo or a subdirectory.
- The default searchable scope is `wiki/index.md`, `wiki/sources/**/*.md`, `wiki/notes/**/*.md`, and `slides/**/*.md`; `wiki/meta/` is excluded.
- Use `scripts/qmd-wiki` for CLI fallback and maintenance commands such as `update` and `embed`.
- After editing `wiki/`, run `scripts/qmd-wiki update`, then run `scripts/qmd-wiki embed` when status shows missing embeddings.

## Maintenance

- Run maintenance checks when ingesting a source, editing a synthesis or overview, or when asked for cleanup.
- Check task-relevant issues: stale or missing index entries, broken links, duplicate-note risk, missing relevant note coverage, and contradicted syntheses or overviews.
- Use `scripts/wiki-lint structure`, `scripts/wiki-lint semantic`, or `scripts/wiki-lint all` for lint reports. Reports are written to `wiki/meta/wiki-lint-YYYY-MM-DD.md` by default.
- Do not add `wiki/meta/` lint reports to `wiki/index.md`.
- Treat semantic lint as agent-reviewed: use QMD discovery and targeted page reads before recording hidden conflicts, stale synthesis, unsupported synthesis, duplicate-note risk, or missing bridge notes as real issues. The standard is no hidden conflicts; meaningful conflicts should be resolved, scoped, or explicitly marked unresolved with source grounding.

## Writing Rules

- In `agent/`, stay close to the source and preserve source-local terminology when useful.
- In `wiki/`, write concise English pages with useful links.
- Do not invent unsupported claims.
- Prefer updating an existing relevant page over creating a duplicate.
