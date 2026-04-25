# LLM-wiki Agent Contract

This repo is a maintained knowledge vault. Treat it as a working knowledge base, not a loose file dump.

## Hard Rules

- Keep the three content layers: `raw/`, `agent/`, `wiki/`.
- Use `raw/<source-slug>/` as the canonical source ID everywhere else.
- Do not rename, move, rewrite, delete, or reorganize existing `raw/<source-slug>/` content unless explicitly asked.
- Use `raw/inbox-DO-NOT-INGEST/` only as a loose collection inbox for newly gathered materials that have not been organized into canonical source folders yet.
- When organizing collected materials, promote each distinct work into its own canonical `raw/<source-slug>/` folder; do not leave canonical sources loose in the inbox.
- Group same-work materials in the same canonical folder when they belong to one source package, such as a paper, appendix, slides, or supplemental notes for the same work.

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
- Keep `wiki/log.md` as the active chronological operational log.

## Default Workflows

### Collect and Organize

1. Drop newly gathered files into `raw/inbox-DO-NOT-INGEST/`.
2. Group files by work identity rather than by file type alone.
3. Promote each distinct work into `raw/<source-slug>/` using the slug rules above.
4. Keep same-work materials together inside that canonical raw folder.
5. Start ingest only after the canonical raw folder is in place.

### Ingest

1. Read the source from `raw/<source-slug>/`.
2. Create or update `agent/<source-slug>/source.md` with a citation-traceable transcription.
3. Create or update `agent/<source-slug>/metadata.json` and `agent/<source-slug>/refs.json`.
4. Create or update `wiki/sources/<source-slug>.md` in English.
5. Update related `wiki/notes/*.md` when the source materially changes their content or synthesis.
6. Update `wiki/index.md`.
7. Add one chronological entry to `wiki/log.md` using `## [YYYY-MM-DD] <operation> | <subject>`.

### Query

1. Read `wiki/index.md` first.
2. Read `wiki/sources/` for per-source pages.
3. Read `wiki/notes/` for reusable cross-source notes.
4. Read `agent/` only when source-language grounding matters.

## Writing Rules

- In `agent/`, stay close to the source and preserve source-local terminology when useful.
- In `wiki/`, write concise English pages with useful links.
- Do not invent unsupported claims.
- Prefer updating an existing relevant page over creating a duplicate.
