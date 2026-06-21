# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A personal learning repository for Data Science and Machine Learning with Python. Content is organized by topic under `notebooks/`. Reusable scripts and modules will go in `src/` when added.

## Workspace

Open the project via **`learn_ds_ml.code-workspace`** at the repo root (File → Open Workspace from File). This sets the workspace root to `learn_ds_ml/`, which makes notebook links like `/notebooks/05_pandas/notes.ipynb` resolve correctly.

The same Python/Jupyter settings also live in `.vscode/settings.json` for opening the folder directly.

## Project Structure

```
learn_ds_ml/
├── learn_ds_ml.code-workspace       # VS Code / Cursor workspace file
├── main.py                          # placeholder entry point
├── pyproject.toml                   # project metadata and dependencies
├── uv.lock                          # locked dependency versions
├── README.md
├── notebooks/
│   ├── 01_python/                   # (planned)
│   ├── 02_dsa_practice_python/      # (planned)
│   ├── 03_dsa_python/               # (planned)
│   ├── 04_numpy/                    # (planned)
│   ├── 05_pandas/                   # Pandas fundamentals (in progress)
│   │   ├── notes.ipynb              # main learning notebook
│   │   ├── faq.md                   # common pitfalls and fixes
│   │   ├── problems.txt             # practice problem links
│   │   └── data/
│   │       ├── marks.csv            # two-column sample (subject, marks)
│   │       └── subs_count.csv       # single-column sample (subscribe_count)
│   └── 06_feature_enginerring/      # EDA and feature engineering (in progress)
│       ├── notes.ipynb              # main learning notebook
│       ├── notes.txt                # scratch notes (optional)
│       ├── data/                    # sample datasets (when needed)
│       └── course_screenshots/      # reference images for notebooks
├── src/                             # (planned) reusable scripts and modules
└── tests/                           # (planned) pytest tests
```

### Topic status

| Topic | Path | Status |
|-------|------|--------|
| Pandas | `notebooks/05_pandas/` | Series basics, CSV I/O, indexing, vectorized ops |
| Feature engineering / EDA | `notebooks/06_feature_enginerring/` | Intro only (ML lifecycle) |

### Conventions

- New learning topics get a numbered folder under `notebooks/<NN>_<topic>/` with a `notes.ipynb`.
- Sample datasets live in `notebooks/<topic>/data/` when needed.
- Topic-specific FAQs and reference docs (e.g. `faq.md`) stay alongside the notebook.
- Notebooks use markdown explanations plus runnable code cells; type hints are used where helpful.
- Course or practice links go in `problems.txt` or markdown cells inside the notebook.
- Cross-notebook links use workspace-root paths: `/notebooks/05_pandas/notes.ipynb`.

## Dependencies

Core libraries (see `pyproject.toml`):

- **Data:** pandas, numpy, polars, scipy
- **Visualization:** matplotlib, seaborn
- **Notebooks:** ipykernel
- **Dev:** pytest, ruff

This project targets **pandas 3.x**. Notable API changes documented in `notebooks/05_pandas/faq.md`:

- `read_csv(..., squeeze=True)` is removed — use `read_csv(...).squeeze()` for single-column CSVs.
- Integer keys in `series[key]` are treated as **labels**, not positions — use `.iloc` for positional access.

## Package Manager

This project uses **uv** (not pip or poetry).

```bash
uv sync                        # install all dependencies
uv add <package>               # add a dependency
uv add --dev <package>         # add a dev dependency
uv run <command>               # run a command in the venv
```

## Common Commands

```bash
uv run python main.py          # run main script
uv run jupyter notebook        # start Jupyter
uv run pytest                  # run all tests (once tests/ exists)
uv run pytest tests/test_foo.py::test_bar  # run a single test
uv run ruff check .            # lint
uv run ruff format .           # format
```

## Python Version

Python 3.14.5 (pinned in `.python-version`). The venv is at `.venv/`.
