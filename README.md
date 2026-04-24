# python-package-template

A [Copier](https://copier.readthedocs.io/) template for Python packages.

It scaffolds:

- `src/`-layout Python package with `uv_build` backend
- `uv` for dependency management
- `just` task runner (`justfile`)
- `prek` for pre-commit hooks
- GitHub Actions: `verify` (lint + type + complexity + tests) and semantic `release`
- Issue and PR templates
- Sensible `.gitignore`, empty `.env`, minimal `README.md`

Both the `prek` / pre-commit hooks and the `verify` CI workflow delegate to the same `just` recipes (`just format` triggers `ruff`, `just check-types` triggers `ty`, `just check-complexity` triggers `complexipy`, `just test` triggers `pytest-cov`), so the exact checks that run locally on commit are the ones that run on CI.

## Usage

```bash
# from github
uvx copier copy gh:nachollorca/python-package-template my-new-project
# or from a local checkout:
uvx copier copy ./python-package-template my-new-project
```
