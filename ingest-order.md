---
type: overview
---

# Ingest Order

## Summary

This note records the current ingest queue for canonical `raw/<source-slug>/` folders that do not yet have a complete `agent/<source-slug>/` package and `wiki/sources/<source-slug>.md` page. Empty `agent/<source-slug>/` placeholder folders count as not ingested and should be removed. The queue excludes `raw/inbox-DO-NOT-INGEST/` and hidden or internal folders.

## Queue Format

`scripts/ingest-codex-langgraph` consumes only Markdown bullets whose entire item is a backticked source slug, for example ``- `raw-source-slug-en` ``.

Do not use plain bullets, task-list checkboxes, links, inline comments, or extra text on queue item lines. Headings and `(none pending)` markers are ignored. The first matching bullet is the next ingest target; after a successful ingest, remove exactly that first backticked slug and preserve the remaining matching bullets in order.

## Ingest Queue

(none pending)
