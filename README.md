# groundcite

[![CI](https://github.com/tiger440/groundcite/actions/workflows/ci.yml/badge.svg)](https://github.com/tiger440/groundcite/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/tiger440/groundcite?display_name=tag&sort=semver)](https://github.com/tiger440/groundcite/releases)
[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE)

**groundcite — grounded answers engine: hybrid retrieval, span-level citations,
document-level permissions, evals built in. Self-hosted.**

> Status: early. The package currently ships the placeholder module inherited
> from the template; the retrieval pipeline lands next.

## What it will do

- **Hybrid retrieval** — dense vectors and lexical search over Postgres 16
  (pgvector + full-text), fused with reciprocal rank fusion, then reranked.
- **Span-level citations** — every claim points at exact `(document, start, end)`
  character offsets, preserved end to end through parsing and chunking.
- **Document-level permissions** — ACL filters applied *inside* the index scan,
  never as a post-filter, with recall measured per permission scope.
- **Evals built in** — a harness that measures each stage of the funnel
  separately, with paired bootstrap confidence intervals on every comparison.
- **Self-hosted** — Postgres is the only storage. No managed vector service.

## Quickstart

```bash
git clone https://github.com/tiger440/groundcite
cd groundcite
uv sync --all-extras
make check
```

Under 5 minutes on a clean machine, or it is a bug.

## Commands

| Command | What it does |
|---|---|
| `uv sync --all-extras` | install everything |
| `make check` | ruff + format check + pyright + pytest |
| `make fix` | auto-fix lint and formatting |
| `make test` | pytest with coverage |
| `make docs-serve` | mkdocs live preview |
| `make bench` | reproducible benchmark harness |

## Docs

Published at <https://tiger440.github.io/groundcite/>. The
[ML companion](docs/ml-companion.md) derives the maths behind each stage of the
pipeline — retrieval geometry, BM25, fusion, reranking, grounded generation and
the statistics of evaluating all of it.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md): DCO sign-off, Conventional Commits,
`make check` green before every PR. Structural decisions get an ADR in
[`adr/`](adr/).

## License

Apache-2.0 — see [LICENSE](LICENSE).
