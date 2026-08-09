# Quickstart

Target: a working checkout with green checks in under 5 minutes.

## Prerequisites

- [uv](https://docs.astral.sh/uv/) (installs and manages Python itself)
- `git`, `make`
- Docker (optional, only for `docker build`)

## Set up

```bash
git clone https://github.com/tiger440/groundcite
cd groundcite
uv sync --all-extras
make check
```

## Daily loop

```bash
make fix     # auto-fix lint and formatting
make test    # pytest with coverage
make check   # everything CI runs, locally
```

Open a short-lived branch, commit with
[Conventional Commits](https://www.conventionalcommits.org/) and a DCO
sign-off (`git commit -s`), push, let CI go green, then squash-merge.
`release-please` opens a release PR that bumps the version and writes the
changelog from those commit messages.

## Docs

```bash
make docs-serve    # live preview on http://127.0.0.1:8000
make docs-build    # strict build, exactly what CI publishes
```

## Container

```bash
docker build -t groundcite .
docker run --rm groundcite
```
