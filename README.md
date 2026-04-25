# mold

A modern [Copier](https://copier.readthedocs.io/) template for Python packages.

![mold](media/mold.png)

## Why this template

**One source of truth for code quality, identical locally and on CI, plus fully automated semantic versioning and publishing.**

Every check (lint, format, type, complexity, spelling, secret scanning, tests) is defined once in the [`justfile`](template/justfile) / [`prek.toml`](template/prek.toml) and runs both in your pre-commit hook *and* in the GitHub Actions [`verify.yaml`](template/.github/workflows/verify.yml) workflow. Conventional commits are enforced, and every merge to `main` triggers an automatic version bump, GitHub release, and PyPI publish — no manual tagging, changelogs, or release steps.

## How it works

### Code Quality Checks
The [`justfile`](template/justfile) is the canonical place where developer commands live: `just format` (ruff lint + format), `just check-types` (ty), `just check-complexity` (complexipy), and `just test` (pytest with coverage).

On every commit, [`prek`](https://github.com/j178/prek) (a faster `pre-commit` drop-in) runs such just recipes (except for the tests) + a combination of generic file hygiene hooks, `codespell` for typos, `gitleaks` for leaked secrets and `commitizen` for [conventional commit convention](https://www.conventionalcommits.org/) enforcement.

The CI [`verify.yml`](template/.github/workflows/verify.yml) workflow simply executes `prek run --all-files` followed by `just test` — so what passes (or fails) on your machine is exactly what passes (or fails) on the remote, and [`prek.toml`](template/prek.toml) is the single source of truth for all checks.

### Automatic Versioning and Releases
Because every commit on `main` is guaranteed to follow the Conventional Commits spec (thanks to the `commitizen` hook), [`python-semantic-release`](https://python-semantic-release.readthedocs.io/) can deterministically derive the next version number from the commit history.

The [`release.yml`](template/.github/workflows/release.yml) workflow runs on every merge to `main`:
1. Computes the new version
2. Updates `pyproject.toml` and the changelog
3. Tags the commit
4. Builds the wheel with `uv build`
5. Publishes it to PyPI with `uv publish`
6. Creates the matching GitHub release.
 
The whole **feat-branch → PR → merge → release loop is hands-off**: you write conventional commits, the rest happens by itself.

## Tooling

- [`uv`](https://docs.astral.sh/uv/) — dependency management, builds, and publishing
- [`uv_build`](https://docs.astral.sh/uv/concepts/build-backend/) — build backend
- [`just`](https://github.com/casey/just) — task runner (`justfile`)
- [`prek`](https://github.com/j178/prek) — fast pre-commit hook runner
- [`ruff`](https://docs.astral.sh/ruff/) — linting and formatting
- [`ty`](https://github.com/astral-sh/ty) — static type checking
- [`complexipy`](https://github.com/rohaquinlop/complexipy) — cyclomatic complexity checks
- [`pytest`](https://docs.pytest.org/) + [`pytest-cov`](https://pytest-cov.readthedocs.io/) — tests with coverage gate
- [`codespell`](https://github.com/codespell-project/codespell) — spell checking
- [`gitleaks`](https://github.com/gitleaks/gitleaks) — secret scanning
- [`pre-commit-hooks`](https://github.com/pre-commit/pre-commit-hooks) — generic file hygiene (trailing whitespace, EOF, merge conflicts, large files, TOML/YAML/JSON validity, …)
- [`commitizen`](https://commitizen-tools.github.io/commitizen/) — Conventional Commits enforcement
- [`python-semantic-release`](https://python-semantic-release.readthedocs.io/) — automatic version bumping, changelog, and GitHub release
- GitHub Actions — `verify` (per-branch checks) and `release` (auto publish to PyPI + GitHub on merge to `main`)

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
