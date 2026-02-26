# Amazon Review Insights - Copilot Instructions

This repository currently contains minimal structure. It is intended to support a data‑science project that ingests, processes, and visualizes Amazon product reviews.

## Big picture

- **Language/Environment**: Python is the primary language. Expect Jupyter notebooks and standard data science libraries (`pandas`, `numpy`, `scikit‑learn`, etc.).
- **Project layout (anticipate)**:
  - `notebooks/` for exploratory analysis and model experiments.
  - `src/` or `amazon_review_insights/` for reusable modules and ETL code.
  - `data/` for raw/processed datasets (ignored by Git).
  - `tests/` for unit/functional tests once modules are added.
  - `requirements.txt` or `environment.yml` to pin dependencies.

At present only `README.md` exists; contribute new folders/files as needed following this pattern.

## Developer workflows

- **Environment setup**: Typically activate a virtualenv/conda and install dependencies from `requirements.txt` or `environment.yml`. If not present, ask the user for the preferred setup.
- **Notebook use**: Run analyses interactively. When converting exploratory code into production logic, move functions into `src/` modules and add corresponding tests.
- **Running code**: There isn't a build system. Scripts are run via `python path/to/script.py` or executed within notebooks.
- **Tests**: Use `pytest` if tests appear. Run `pytest` from the repo root.

## Conventions & patterns

- Use snake_case for Python modules and functions.
- Data files typically live in a `data/` directory and are accessed via relative paths; avoid hardcoding absolute paths.
- Notebook names should start with a two‑digit prefix for ordering (`01-explore.ipynb`).

## Integration points

- There are no external services configured yet. Future expansions might include:
  - S3 or local file system for data storage.
  - Logging via the standard `logging` module.
  - A simple CLI wrapper (e.g. `python -m amazon_review_insights.cli`) for batch tasks.

## Guidance for Copilot/AIs

When assisting in this repo:

1. **Ask clarifying questions** immediately when high‑level goals are unclear. The codebase is small and evolving.
2. **Suggest structure** early: if new functionality is needed, propose creating modules under `src/` and adding tests.
3. **Be conservative** with assumptions; check with the user before adding dependencies or frameworks not already present.
4. **Use examples from the workspace** when writing code. For instance, if adding a processing function, show where it would live relative to `notebooks/`.
5. **Document new conventions** in this instructions file whenever the user adopts a new pattern.

> _This file is a living document. Update it as the project grows. If you notice missing guidance, add another section and ask the user to review._
