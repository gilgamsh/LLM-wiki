# LLM-wiki

Public template repository for maintaining a **source-grounded** knowledge wiki.

This repository standardizes a simple workflow that turns raw materials into (1) grounded extractions and (2) a curated English wiki layer suitable for long-term maintenance.

## Repository layout

- `raw/`: canonical source packages (PDFs, web exports, slides, notes) grouped by **source slug**
- `agent/`: source-grounded, agent-facing artifacts derived from one `raw/<source-slug>/`
- `wiki/`: English wiki pages (per-source pages and cross-source notes)

## Credits

This template is inspired by Andrej Karpathy’s [“LLM Wiki” concept](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

## Quick start

### 1) Drop sources into the inbox

Add newly collected files under `raw/inbox-DO-NOT-INGEST/` (PDFs, web exports, slides, notes). This template is designed for an agent-driven workflow: the agent classifies sources, promotes them into canonical `raw/<source-slug>/` packages, ingests them into `agent/<source-slug>/`, and updates `wiki/`.

### 2) Ask your agent to organize + ingest

Use your preferred coding agent (e.g. Codex / Claude Code) and instruct it to follow `AGENTS.md`. A typical request is:

- organize new inbox items into canonical `raw/<source-slug>/`
- create/update the corresponding `agent/<source-slug>/` packages
- create/update `wiki/sources/<source-slug>.md` and any relevant `wiki/notes/*.md`
- update `wiki/index.md` and append to `wiki/log.md`

## Workflow

- **Collect**: add new files under `raw/inbox-DO-NOT-INGEST/`
- **Organize**: classify sources and promote each work into `raw/<source-slug>/`
- **Ingest**: produce/update `agent/<source-slug>/` grounded artifacts
- **Write**: update `wiki/sources/` and `wiki/notes/`
- **Maintain**: keep `wiki/index.md` and `wiki/log.md` current

## Operating rules

The repository contract is defined in `AGENTS.md`. Configure any automation or agent workflows to follow it.



## Optional: Automation

This template includes optional repo-local automation under `scripts/`.

### Repo-local search (QMD)

If you have `qmd` installed, you can use a repo-local index for discovery:

```sh
scripts/qmd-wiki status
scripts/qmd-wiki query "search terms"
scripts/qmd-wiki query "search terms" -c wiki-notes
```

### Wiki lint

Generate a structural lint report (and optionally a semantic review packet):

```sh
scripts/wiki-lint structure
scripts/wiki-lint semantic
scripts/wiki-lint all
```

Reports are written to `wiki/meta/` by default. Use `--stdout` to print instead of writing.

### Log compaction

Compact older entries in `wiki/log.md` into a deterministic rollup entry:

```sh
scripts/wiki-log-compact
scripts/wiki-log-compact --write
```
