# 3. Dependency Management

[← Project Setup](02-project-setup.md) · [README](../README.md) · [Next: Command Reference →](04-command-reference.md)

## Table of Contents

- [Adding runtime dependencies](#adding-runtime-dependencies)
  - [Direct vs. transitive dependencies](#direct-vs-transitive-dependencies)
- [Adding development dependencies](#adding-development-dependencies)
- [Inspecting the dependency tree](#inspecting-the-dependency-tree)
- [How pyproject.toml looks afterwards](#how-pyprojecttoml-looks-afterwards)
- [The lock file: uv.lock](#the-lock-file-uvlock)
- [Reproducing the environment elsewhere](#reproducing-the-environment-elsewhere)
- [Running code in the environment](#running-code-in-the-environment)
- [Using the environment in Jupyter](#using-the-environment-in-jupyter)
- [Changing dependencies later](#changing-dependencies-later)
- [Migrating from requirements.txt](#migrating-from-requirementstxt)
- [Next step](#next-step)

## Adding runtime dependencies

```powershell
uv add pandas numpy pyarrow
```

Session output:

```text
Using CPython 3.12.4 interpreter at: C:\Users\<USER>\AppData\Local\Programs\Python\Python312\python.exe
Creating virtual environment at: C:\Users\<USER>\Downloads\bookings-project\.venv
Resolved 7 packages in 2.49s
Prepared 7 packages in 14.44s
Installed 7 packages in 2.24s
 + bookings-project==0.1.0 (from file:///C:/Users/<USER>/Downloads/bookings-project)
 + numpy==2.5.3
 + pandas==3.0.6
 + pyarrow==25.0.1
 + python-dateutil==2.9.0.post0
 + six==1.17.0
 + tzdata==2026.5
```

On this first `uv add`, uv did several things automatically:

1. **Found a Python interpreter** (CPython 3.12.4).
2. **Created `.venv`** in the project root. No manual `python -m venv` is needed.
3. **Resolved** the dependency graph (7 packages including transitive dependencies).
4. **Installed** the packages into `.venv`.
5. **Updated `pyproject.toml`** with your direct dependencies.
6. **Created `uv.lock`** with exact versions of everything.

### Direct vs. transitive dependencies

| Type | Packages | Source |
|------|----------|--------|
| Direct (you asked for them) | pandas, numpy, pyarrow | Listed in `pyproject.toml` |
| Transitive (pulled in automatically) | python-dateutil, six, tzdata | Recorded only in `uv.lock` |

## Adding development dependencies

Tools needed only while developing (not by the code at runtime) go in the `dev` group:

```powershell
uv add --dev ipykernel
```

`ipykernel` lets Jupyter notebooks, including those in VS Code, run on the project's environment. It installed 27 packages, mostly its own dependencies (`ipython`, `jupyter-client`, `tornado`, `pyzmq` and others), taking the total to 38 resolved packages.

## Inspecting the dependency tree

```powershell
uv tree --depth 1
```

```text
bookings-project v0.1.0
├── numpy v2.5.3
├── pandas v3.0.6
├── pyarrow v25.0.1
└── ipykernel v7.4.0 (group: dev)
```

- `--depth 1` shows only direct dependencies.
- Omit it to see the full tree, including transitive packages.
- `(group: dev)` marks development-only dependencies.

## How `pyproject.toml` looks afterwards

After the commands above, the dependency sections resemble this:

```toml
[project]
name = "bookings-project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "numpy>=2.5.3",
    "pandas>=3.0.6",
    "pyarrow>=25.0.1",
]

[dependency-groups]
dev = [
    "ipykernel>=7.4.0",
]
```

> The exact contents may differ slightly (for example the `description` and the `requires-python` bound). Open your own `pyproject.toml` to confirm.

## The lock file: `uv.lock`

| Property | Detail |
|----------|--------|
| Purpose | Records the exact version of every package, direct and transitive |
| Size in session | About 129 KB for 38 packages |
| Edited by hand? | No. uv manages it. |
| Commit to Git? | **Yes**, so collaborators and CI get identical environments |

`pyproject.toml` says *what you want* (`pandas>=3.0.6`). `uv.lock` says *exactly what you got* (`pandas==3.0.6`).

## Reproducing the environment elsewhere

On a new machine or fresh clone:

```powershell
git clone <your-repo-url>
cd bookings-project
uv sync
```

`uv sync` creates `.venv` and installs exactly what `uv.lock` specifies, including the dev group by default.

| Command | Behaviour |
|---------|-----------|
| `uv sync` | Install per the lock file (updates the lock if `pyproject.toml` changed) |
| `uv sync --locked` | Fail if the lock file is out of date. Good for CI. |
| `uv sync --frozen` | Install from the lock file without checking it is current |
| `uv sync --no-dev` | Skip development dependencies |

## Running code in the environment

You do not need to activate `.venv`. Use `uv run`:

```powershell
uv run python script.py
uv run python -c "import pandas as pd; print(pd.__version__)"
```

`uv run` makes sure the environment is in sync with `pyproject.toml` and `uv.lock`, then runs your command inside it.

To activate manually instead:

```powershell
.venv\Scripts\Activate.ps1
```

> If PowerShell blocks the script, see [Troubleshooting](05-troubleshooting.md#cannot-activate-the-virtual-environment).

## Using the environment in Jupyter

With `ipykernel` installed, open a notebook in `notebooks/` from VS Code and choose the kernel backed by `.venv\Scripts\python.exe`. Alternatively, run a notebook server on demand without adding it as a project dependency:

```powershell
uv run --with jupyter jupyter lab
```

## Changing dependencies later

```powershell
uv add matplotlib                 # add a package
uv add "pandas>=3,<4"             # add with a version constraint
uv remove pyarrow                 # remove a package
uv lock --upgrade-package pandas  # upgrade one package within constraints
uv lock --upgrade                 # upgrade everything within constraints
```

## Migrating from `requirements.txt`

```powershell
uv add -r requirements.txt
```

To export back out for tools that need a `requirements.txt`:

```powershell
uv export --format requirements-txt > requirements.txt
```

## Next step

See the [Command Reference](04-command-reference.md) for a cheat sheet of everyday commands.
