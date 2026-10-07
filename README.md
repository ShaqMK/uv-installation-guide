# uv Guide: Python Project Management on Windows

A practical, end-to-end guide to installing [uv](https://docs.astral.sh/uv/), the fast Python package and project manager from Astral, and using it to set up a reproducible data-analysis project.

The guide is based on a real setup session (uv `0.12.23`, Windows, PowerShell, CPython 3.12) in which a `bookings-project` was created to analyse yearly bookings CSV files with pandas.

---

## Table of Contents

- [Documentation](#documentation)
- [Repository Structure](#repository-structure)
- [The Example Project](#the-example-project)
- [Quick Start](#quick-start)
- [Versions Used](#versions-used)
- [Further Reading](#further-reading)

---

## Documentation

| # | Document | What it covers |
|---|----------|----------------|
| 1 | [Installation and Prerequisites](docs/01-installation.md) | System requirements, installing uv, PATH setup, verification, updating |
| 2 | [Project Setup](docs/02-project-setup.md) | `uv init`, folder layout, copying data, opening in VS Code |
| 3 | [Dependency Management](docs/03-dependency-management.md) | `uv add`, dev dependencies, `pyproject.toml`, `uv.lock`, virtual environments |
| 4 | [Command Reference](docs/04-command-reference.md) | Cheat sheet of the most common uv commands |
| 5 | [Troubleshooting](docs/05-troubleshooting.md) | Real errors from the session and how to fix them |

Suggested reading order: 1 → 2 → 3, then keep 4 and 5 handy as references.

---

## Repository Structure

```text
uv-guide/
├── README.md
└── docs/
    ├── 01-installation.md
    ├── 02-project-setup.md
    ├── 03-dependency-management.md
    ├── 04-command-reference.md
    └── 05-troubleshooting.md
```

## The Example Project

The guide builds the following project, which is the end state of the session:

```text
bookings-project/
├── .venv/                 # Virtual environment (created by uv, not committed)
├── data/
│   ├── raw/               # Original, untouched CSV files
│   │   ├── Bookings_2023.csv
│   │   ├── Bookings_2024.csv
│   │   └── Bookings_2025.csv
│   └── processed/         # Cleaned / transformed data
├── notebooks/             # Jupyter notebooks for exploration
├── outputs/               # Charts, reports, exported results
├── src/                   # Reusable Python modules
├── .python-version        # Pinned Python version
├── pyproject.toml         # Project metadata and dependencies
├── uv.lock                # Exact, reproducible dependency lock file
└── README.md
```

## Quick Start

```powershell
# 1. Install uv
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# 2. Verify
uv --version

# 3. Create a project
uv init bookings-project
cd bookings-project

# 4. Add dependencies
uv add pandas numpy pyarrow
uv add --dev ipykernel

# 5. Run code inside the project environment
uv run python -c "import pandas; print(pandas.__version__)"
```

## Versions Used

| Component | Version |
|-----------|---------|
| uv | 0.12.23 |
| CPython | 3.12.4 |
| pandas | 3.0.6 |
| numpy | 2.5.3 |
| pyarrow | 25.0.1 |
| ipykernel (dev) | 7.4.0 |

> Versions reflect the session date (October 2026). Running `uv add` today may resolve newer releases; `uv.lock` is what guarantees reproducibility.

## Further Reading

- [Official uv documentation](https://docs.astral.sh/uv/)
- [uv GitHub repository](https://github.com/astral-sh/uv)
