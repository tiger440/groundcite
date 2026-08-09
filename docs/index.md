# groundcite

**Grounded answers engine: hybrid retrieval, span-level citations,
document-level permissions, evals built in. Self-hosted.**

> Status: early. The package currently ships the placeholder module inherited
> from the template; the retrieval pipeline lands next.

## Why

A retrieval-augmented answer is only worth what its weakest stage is worth:

$$P(\text{correct}) \approx P(\text{in corpus}) \cdot P(\text{retrieved}) \cdot P(\text{used}) \cdot P(\text{faithful})$$

Four stages at 90% give a system at 66%. groundcite instruments each stage
separately, so a wrong answer is traced to the stage that lost the information
instead of blamed on "the model hallucinated".

## Design commitments

| Commitment | What it means |
|---|---|
| Hybrid retrieval | dense + lexical over Postgres 16, fused with RRF, then reranked |
| Span-level citations | exact `(document, char_start, char_end)` preserved through the whole pipeline |
| Permissions | ACL filters inside the index scan, recall measured per permission scope |
| Evals built in | per-stage metrics, paired bootstrap CIs, frozen test set |
| Self-hosted | Postgres is the only storage; no managed vector service |
| Reproducible numbers | no published benchmark without a `make bench` target that regenerates it |

## Where to go next

- [Quickstart](quickstart.md) — running checks in under 5 minutes.
- [ML companion](ml-companion.md) — the maths behind every stage of the pipeline.

## API reference

::: groundcite.text
