# 4. Command Reference

[← Dependency Management](03-dependency-management.md) · [README](../README.md) · [Next: Troubleshooting →](05-troubleshooting.md)

## Table of Contents

- [Project lifecycle](#project-lifecycle)
- [Running things](#running-things)
- [Python versions](#python-versions)
- [Virtual environments](#virtual-environments)
- [pip-compatible interface](#pip-compatible-interface)
- [Tools (replaces pipx)](#tools-replaces-pipx)
- [Maintenance](#maintenance)
- [Useful flags](#useful-flags)
- [Typical daily workflow](#typical-daily-workflow)
- [PowerShell notes](#powershell-notes)
- [Next step](#next-step)

A cheat sheet of the commands you will use most. Run `uv help` or `uv <command> --help` for the full option list.

## Project lifecycle

| Command | Description |
|---------|-------------|
| `uv init <name>` | Create a new project in a new folder |
| `uv init` | Initialise a project in the current folder |
| `uv add <pkg>` | Add a dependency (updates `pyproject.toml` and `uv.lock`, installs it) |
| `uv add --dev <pkg>` | Add a development-only dependency |
| `uv add --group <name> <pkg>` | Add to a custom dependency group |
| `uv add -r requirements.txt` | Import dependencies from a requirements file |
| `uv remove <pkg>` | Remove a dependency |
| `uv sync` | Install the environment from the lock file |
| `uv lock` | Create or refresh `uv.lock` without installing |
| `uv run <command>` | Run a command inside the project environment |
| `uv tree` | Show the dependency tree |
| `uv tree --depth 1` | Show only direct dependencies |
| `uv build` | Build the project into a distributable package |
| `uv export --format requirements-txt` | Export the lock file as `requirements.txt` |

## Running things

| Command | Description |
|---------|-------------|
| `uv run python script.py` | Run a script in the project environment |
| `uv run python` | Open a Python REPL in the environment |
| `uv run --with <pkg> <command>` | Run with an extra temporary package |
| `uv run --no-dev <command>` | Run without development dependencies |

## Python versions

| Command | Description |
|---------|-------------|
| `uv python list` | List available and installed Python versions |
| `uv python install 3.12` | Download and install a Python version |
| `uv python pin 3.12` | Write the version to `.python-version` |
| `uv python find` | Show which interpreter uv would use |
| `uv python dir` | Show where uv stores Python installs |

## Virtual environments

| Command | Description |
|---------|-------------|
| `uv venv` | Create a `.venv` in the current folder |
| `uv venv --python 3.12` | Create one with a specific Python version |

> Inside a uv project, `.venv` is created for you the first time you run `uv add`, `uv sync` or `uv run`.

## pip-compatible interface

For working outside the project workflow, or with existing pip habits:

| Command | Description |
|---------|-------------|
| `uv pip install <pkg>` | Install into the active environment |
| `uv pip install -r requirements.txt` | Install from a requirements file |
| `uv pip list` | List installed packages |
| `uv pip freeze` | Print installed packages with versions |
| `uv pip uninstall <pkg>` | Remove a package |

## Tools (replaces pipx)

| Command | Description |
|---------|-------------|
| `uvx <tool>` | Run a tool in a temporary environment (alias of `uv tool run`) |
| `uv tool install <tool>` | Install a tool globally and isolated |
| `uv tool list` | List installed tools |
| `uv tool upgrade <tool>` | Upgrade an installed tool |
| `uv tool uninstall <tool>` | Remove a tool |

Examples:

```powershell
uvx ruff check .
uv tool install ruff
```

## Maintenance

| Command | Description |
|---------|-------------|
| `uv --version` | Show the installed uv version |
| `uv self update` | Update uv itself |
| `uv cache dir` | Show the cache location |
| `uv cache clean` | Clear the cache |
| `uv cache prune` | Remove unused cache entries |

## Useful flags

| Flag | Meaning |
|------|---------|
| `--dev` | Target the development dependency group |
| `--locked` | Error if `uv.lock` is out of date |
| `--frozen` | Use `uv.lock` as-is without re-resolving |
| `--no-dev` | Exclude development dependencies |
| `--python <ver>` | Use a specific Python version |
| `--upgrade` | Allow dependencies to move to newer versions |
| `--depth <n>` | Limit tree depth (with `uv tree`) |

## Typical daily workflow

```powershell
cd bookings-project
uv sync                      # make sure the environment matches the lock file
uv add seaborn               # add something new when needed
uv run python src\analysis.py
uv tree --depth 1            # review what you depend on
git add pyproject.toml uv.lock
git commit -m "Add seaborn"
```

## PowerShell notes

Several Unix habits do not carry over to PowerShell:

| Unix | PowerShell equivalent |
|------|----------------------|
| `ls -la` | `ls` or `Get-ChildItem -Force` (shows hidden items) |
| `cp a b` | `Copy-Item a b` |
| `mkdir -p a/b` | `mkdir a\b` (creates intermediate folders) |
| `cat file` | `Get-Content file` |
| `pwd` | `pwd` or `Get-Location` |

## Next step

If something goes wrong, see [Troubleshooting](05-troubleshooting.md).
