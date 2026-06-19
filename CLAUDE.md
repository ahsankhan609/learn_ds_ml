# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A learning repository for Data Science and Machine Learning with Python. Notebooks go in `notebooks/`, reusable scripts/modules go in `src/`.

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
uv run pytest                  # run all tests
uv run pytest tests/test_foo.py::test_bar  # run a single test
uv run ruff check .            # lint
uv run ruff format .           # format
```

## Python Version

Python 3.14.5 (pinned in `.python-version`). The venv is at `.venv/`.
