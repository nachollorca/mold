# mold

A modern [![Copier](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/copier-org/copier/master/img/badge/badge-grayscale-inverted-border-orange.json)](https://github.com/copier-org/copier) template for Python packages.

![mold](media/mold.png)

## Why this template

**One source of truth for all code quality checks, identical locally and remotely, plus fully automated semantic versioning and publishing.**

**Code Quality:** every check (lint, type, complexity, spelling, …) is defined once and runs both in your pre-commit hook *and* in CI — so what passes on your machine is exactly what passes on the remote. Developer commands live in a single `justfile`, invoked the same way locally and from CI, with no duplicated logic to drift apart.

**Automatic Release:** [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) are enforced on every commit, which means the version number, changelog, git tag, GitHub release, and PyPI publish can all be derived deterministically from history. The whole **feat-branch → PR → merge → release loop is hands-off**: you write conventional commits, the rest happens by itself — no manual tagging, changelogs, or release steps.

The particulars are intentionally kept in the files themselves; browse [`template/`](template/) or follow the links in the tooling list below if you want to see exactly how each piece is wired.

## Tooling

- [`uv`](https://docs.astral.sh/uv/) — dependency management, builds, and publishing
- [`uv_build`](https://docs.astral.sh/uv/concepts/build-backend/) — build backend
- [`just`](https://github.com/casey/just) — task runner (see [`justfile`](justfile))
- [`prek`](https://github.com/j178/prek) — fast pre-commit hook runner (see [`prek.toml`](prek.toml))
- [`ruff`](https://docs.astral.sh/ruff/) — linting and formatting
- [`ty`](https://github.com/astral-sh/ty) — static type checking
- [`complexipy`](https://github.com/rohaquinlop/complexipy) — cyclomatic complexity checks
- [`pytest`](https://docs.pytest.org/) + [`pytest-cov`](https://pytest-cov.readthedocs.io/) — tests with coverage gate
- [`codespell`](https://github.com/codespell-project/codespell) — spell checking
- [`gitleaks`](https://github.com/gitleaks/gitleaks) — secret scanning
- [`pre-commit-hooks`](https://github.com/pre-commit/pre-commit-hooks) — generic file hygiene (trailing whitespace, EOF, merge conflicts, large files, TOML/YAML/JSON validity, …)
- [`commitizen`](https://commitizen-tools.github.io/commitizen/) — Conventional Commits enforcement
- [`python-semantic-release`](https://python-semantic-release.readthedocs.io/) — automatic version bumping, changelog, and GitHub release
- GitHub Actions — [`verify.yaml`](.github/workflows/verify.yaml) (per-branch checks) and [`release`](.github/workflows/release.yaml) (auto publish to PyPI + GitHub on merge to `main`)

## Prerequisites

Before using this template, you need to install the following tools on your system (they are not Python dependencies and won't be installed in a virtual environment):

- [uv](https://docs.astral.sh/uv/getting-started/installation/) — required to run Copier and manage the project.
- [just](https://github.com/casey/just#installation) — required to run the tasks defined in the `justfile`.
- [prek](https://github.com/j178/prek#installation) — required to run the pre-commit hooks.

## Usage

```bash
# from github
uvx copier copy gh:nachollorca/mold my-new-project
# or from a local checkout:
uvx copier copy ./mold my-new-project
```
