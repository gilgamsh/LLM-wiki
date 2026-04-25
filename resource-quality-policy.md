# Resource Quality Policy

This policy defines how candidate papers and resources are selected, scored, and triaged for this wiki. Apply it before promoting inbox materials into a canonical `raw/<source-slug>/` folder and before committing to canonical ingest.

## Where Quality Records Live

- Record quality scores, decisions, and mini-reviews in review queues or other operational review files.
- Do not store quality scores in `agent/<source-slug>/metadata.json`, `refs.json`, or public `wiki/sources/<source-slug>.md` pages.
- Author and affiliation facts for each source live in `agent/<source-slug>/source.md` under `## Authors and Affiliation`, source-grounded and limited to that package. Author/lab provenance scores and cross-source judgments remain in review queues.
- Prefer primary sources over mirrors, metadata pages, and summaries.

## Accepted Source Tracks

### Core Topic

Use this track for sources whose primary contribution is directly in the wiki's focus area. In-scope topics include:

- Methods, systems, mechanisms, and evaluations central to the focus area
- Benchmarks, datasets, and evaluation harnesses for the focus area
- Tools, infrastructure, and workflows that materially shape work in the focus area

### Transferable Methods

Use this track for sources outside the immediate focus area that provide mechanisms likely to transfer in. In-scope topics include:

- Coding agents, software engineering agents, and repository-scale repair systems
- Web agents and long-horizon tool-use agents
- Memory, context engineering, retrieval, planning, reflection, and evaluation harnesses
- Multi-agent coordination, workflow search, sandboxing, tool protocols, and agent observability

Do not canonicalize a transfer-track source unless the review can clearly complete this sentence: "This helps the focus area with ___."

### Support Sources

Use this track for durable references that support interpretation, implementation, or evaluation but are not central research sources by themselves. Examples include:

- Standards, vendor documentation, tool manuals, and benchmark documentation
- Artifacts, datasets, repositories, appendices, and supplementary materials
- Surveys, background technical papers, and authoritative reports

## Hard Exclusion Rules

Skip a candidate when any hard exclusion applies:

- A primary source exists and the candidate is only a duplicate mirror, upload, metadata page, or scraped abstract page.
- The source is a low-authority tutorial, forum thread, AI summary page, generic news article, or promotional page with no durable technical content.
- The source makes vague claims without mechanisms, evaluation, or artifacts that can inform research.
- A transfer-track source cannot answer: "This helps the focus area with ___."
- The candidate is already covered by a stronger source in the wiki and adds no new mechanism, evidence, artifact, or contradiction.

## Decisions

Use these decision labels in review queues:

- `canonical`: promote or keep as a canonical `raw/<source-slug>/` package and eligible for full `agent/` plus `wiki/` ingest.
- `support-only`: retain only as support for another source, note, artifact, or citation trail. Do not give it a standalone wiki source page unless it becomes a durable reference needed across the wiki.
- `monitor`: do not promote yet. Record why it may become useful and what evidence would change the decision.
- `skip`: do not download, promote, or ingest. If already present in an inbox, leave removal or cleanup to an explicit collection-maintenance task.

If an existing queue uses `include`, treat it as equivalent to `canonical`.

## Scoring Rubric

Score each candidate out of 16 points.

| Criterion | Points | Guide |
| --- | ---: | --- |
| Domain or transfer relevance | 0-3 | `0`: no clear focus-area or transfer value. `1`: broad background only. `2`: concrete transferable mechanism or adjacent relevance. `3`: direct focus-area source or high-value transfer mechanism. |
| Source reliability | 0-2 | `0`: anonymous, promotional, unverifiable, or low-authority. `1`: identifiable preprint, workshop paper, artifact, or secondary report. `2`: peer-reviewed paper, official documentation, widely used artifact, or authoritative primary source. |
| Author/lab provenance | 0-2 | `0`: no added author signal, unsupported reputation claim, unknown relationship, or irrelevant author history. `1`: identifiable authors, lab, or artifact maintainers have source-backed relevant prior work in the focus area or adjacent fields. `2`: field-leading or especially trusted authors/labs have source-backed direct relevance to the candidate's area, such as an established professor group, advisor/student lineage, major benchmark or artifact maintainers, or recognized academic/industry team. |
| Technical mechanism and explanatory insight | 0-4 | `0`: vague claims only. `1`: high-level method description. `2`: clear algorithms, system architecture, prompts, workflows, tasks, tool interfaces, or dataflow. `3`: explains why the technique should work, including design rationale, feedback signals, constraints, decomposition, retrieval, reward, tool-use, or learning dynamics. `4`: gives deep causal insight into why the technique works or transfers, backed by ablations, failure-mode analysis, examples, theory, or component-level evidence showing which parts matter. |
| Empirical grounding | 0-2 | `0`: no meaningful evidence. `1`: limited examples, small experiments, or qualitative evidence. `2`: credible benchmarks, ablations, real tasks, user studies, or tool-validated results. |
| Reproducibility or artifact clarity | 0-1 | `0`: no usable implementation, data, protocol, or artifact detail. `1`: clear code, data, prompts, benchmark protocol, artifact, or enough implementation detail to reproduce the core idea. |
| Novelty vs existing wiki | 0-1 | `0`: duplicative of stronger wiki sources. `1`: adds a new mechanism, dataset, result, contradiction, or useful framing. |
| Wiki integration value | 0-1 | `0`: isolated, hard to connect, or unlikely to affect notes. `1`: clearly updates source pages, notes, comparisons, syntheses, or ingest priorities. |

Author/lab provenance is a confidence and prioritization signal, not a substitute for technical content. Do not use reputation alone to rescue a candidate that fails hard exclusions, has no focus-area relevance, or lacks mechanism/evidence. When awarding nonzero author points, record the evidence in the review queue rather than in public wiki source pages.

Suggested author provenance registry fields:

```md
## Author or Lab Name

- Current or known affiliation:
- Role: professor | student | industry researcher | artifact maintainer | benchmark maintainer | unknown
- Relevant areas:
- Related vault sources:
- Relationships:
  - Advisor/student, coauthor, lab, company, benchmark, or artifact relationship, with evidence
- Provenance note:
```

## Decision Thresholds

| Score | Default decision |
| ---: | --- |
| 13-16 | `canonical`, high priority |
| 10-12 | `canonical`, normal priority |
| 8-9 | `monitor` or `support-only` |
| 5-7 | `support-only` |
| 0-4 | `skip` |

Reviewer judgment can override a threshold, but the review queue should state the reason. Hard exclusions override the numeric score.

## Preprint and Unpublished Mini-Review

Unpublished manuscripts, arXiv preprints, technical reports, and unreviewed PDFs require a short reviewer-style mini-review before canonical ingest. This is required even when the source scores above the canonical threshold.

The mini-review must record:

- Score
- Author/lab provenance evidence when the author score is nonzero
- Mechanism insight, especially why the technique works or transfers
- Strengths
- Weaknesses
- Confidence
- Main risk during ingest
- Related wiki notes likely affected

Use the mini-review to identify overclaimed results, weak baselines, missing artifacts, fragile evaluation, unclear relation to the focus area, or claims that should be presented as tentative.

## Review Queue Template

Use this template in review queues before downloading, promoting, or ingesting resources:

```md
Decision: canonical | support-only | monitor | skip
Track: core-topic | transferable-method | support
Quality score: N/16
Reason:
Author/lab provenance evidence:
Mechanism insight:
Mini-review:
- Strengths:
- Weaknesses:
- Confidence: high | medium | low
- Ingest risk:
Related wiki notes:
Primary source preferred over:
```

Keep detailed quality judgments out of canonical wiki source pages unless they become content-relevant critiques.

## Calibration Examples

- A direct focus-area paper with clear experiments and clear mechanism insight should usually score `13-16` and become `canonical`, high priority.
- A transfer-track source should become `canonical` only when its mechanism has clear transfer value, such as repository-scale repair, tool-feedback loops, evaluation harness design, context management, or multi-agent coordination.
- A weak arXiv preprint with unclear evaluation, thin artifacts, or overbroad claims should receive a mini-review and usually become `monitor` until stronger evidence appears.
- A ResearchGate upload, Semantic Scholar metadata page, scraped abstract page, or duplicate PDF mirror should become `skip` when a primary source exists, or at most `support-only` when it is needed to locate the primary source.
