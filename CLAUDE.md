# Project memory — groundcite (the trust stack)

## What this repo is

**groundcite** — a grounded answers engine: hybrid retrieval, span-level
citations, document-level permissions, evals built in. Self-hosted. It is the
first of three open source projects, "the trust stack" (with **cachette**, a
correctness-first semantic cache in Rust, and **signoff**, a human sign-off
layer for AI agents). Instantiated from `tiger440/python-template`, whose
quality bars are kept below.

### Design commitments specific to groundcite

- **Storage is Postgres 16 only** — pgvector for dense, native full-text for
  lexical. A second datastore requires an ADR that argues the numbers.
- **Hybrid by construction**, not as an option: dense embeddings are blind to
  negation, numbers, identifiers and rare entities; lexical covers exactly
  those. Fuse with RRF first (no hyperparameter), rerank after.
- **Citations are `(document_id, char_start, char_end)`**, exact through
  parsing → cleaning → chunking → index. Offset correctness is an invariant
  with unit tests: a span off by three characters looks like a lie.
- **ACL filters run inside the index scan**, never as an application-level
  post-filter, and recall is measured *per permission scope* — narrow-permission
  users are exactly those a silent recall drop hurts most.
- **Every stage of the funnel is measured separately**
  (`in corpus → retrieved → used → faithful`); a wrong answer is attributed to
  a stage, never to "the model hallucinated".
- **No comparison without a paired bootstrap CI** and the count of discordant
  queries. Dev set for iteration, frozen test set looked at 3–4 times total.
- Latency target to hold: **p95 < 800 ms** end to end.

`docs/ml-companion.md` derives the maths behind each of these; keep it amended
when an eval loop contradicts it.

## Non-negotiables

- Every public number must be reproducible: no benchmark claim without a
  `make bench` target that regenerates it.
- Boring tech by default: Postgres 16 (incl. pgvector + full-text search) is
  the only storage. Anything else requires an ADR first.
- Narrow and finished beats broad and half-done. Out-of-milestone scope →
  open an issue + ADR first, never code it directly.
- Quickstart must stay under 5 minutes on a clean machine.
- Code, docs, commits: English. Always.

## Engineering rules

- Python 3.12+. `uv` only — never call pip or poetry directly.
- Lint/format: ruff (line length 100). Types: pyright strict — new code ships typed.
- Conventional Commits are REQUIRED (release-please derives versions and
  changelogs from them): `feat | fix | docs | chore | refactor | perf | test | ci`.
  Breaking change: `feat!:` with a `BREAKING CHANGE:` footer.
- Tests: pytest. A feature without tests is not done. Keep the unit suite under 60 s.
- Errors: raise typed exceptions; never bare `except:`; no silent fallbacks.
- Observability: OpenTelemetry spans on every I/O boundary, from day one.
- Public API change → docstrings + mkdocs page updated in the same PR.

## Workflow

- Trunk-based: short-lived branch → PR → squash-merge. CI must be green — never merge red.
- Definition of done: code + tests + docs + conventional commit + benchmarks not regressed.
- Structural decisions get an ADR (use `/adr`). One decision per ADR.
- Before any release: run `/ship` and follow the checklist.
- Rhythm: ship something visible every week; release at most every two weeks.

## Commands

- `uv sync --all-extras` — install everything
- `make check` — ruff + format check + pyright + pytest (run before every PR)
- `make fix` — auto-fix lint + format
- `make docs-serve` — mkdocs live preview
- `make bench` — reproducible benchmark harness (placeholder in the template)
