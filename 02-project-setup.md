# 2. Project Setup

[← Installation](01-installation.md) · [README](../README.md) · [Next: Dependency Management →](03-dependency-management.md)

## Table of Contents

- [Step 1: Choose a location and initialise the project](#step-1-choose-a-location-and-initialise-the-project)
- [Step 2: Create the data-project folders](#step-2-create-the-data-project-folders)
- [Step 3: Copy the raw data](#step-3-copy-the-raw-data)
- [Step 4: Add dependencies](#step-4-add-dependencies)
- [Step 5: Open the project in VS Code](#step-5-open-the-project-in-vs-code)
- [Final structure](#final-structure)
- [Recommended: add a .gitignore before pushing to GitHub](#recommended-add-a-gitignore-before-pushing-to-github)
- [Next step](#next-step)

This section walks through creating the `bookings-project`, organising its folders, and loading the raw data.

## Step 1: Choose a location and initialise the project

```powershell
cd $HOME\Downloads
uv init bookings-project
```

```text
Initialized project `bookings-project` at `C:\Users\<USER>\Downloads\bookings-project`
```

`uv init <name>` creates a new folder with a starter project:

| File / Folder | Purpose |
|---------------|---------|
| `pyproject.toml` | Project metadata and dependency list |
| `.python-version` | Python version uv should use for this project |
| `README.md` | Empty project readme to fill in |
| `src/` | Source folder (created by uv in this session) |

> Tip: to initialise inside an existing folder, run `uv init` with no name from within that folder.

## Step 2: Create the data-project folders

Move into the project and create the structure for a data workflow:

```powershell
cd bookings-project
mkdir data\raw, data\processed, notebooks, src, outputs
```

| Folder | Purpose |
|--------|---------|
| `data\raw` | Original input files. Treat as read-only. |
| `data\processed` | Cleaned and transformed datasets |
| `notebooks` | Jupyter notebooks for exploration and analysis |
| `src` | Reusable Python code (functions, modules) |
| `outputs` | Charts, reports and exported results |

> **Heads-up:** In the session, `mkdir` reported that `src` already existed because `uv init` had already created it. This is harmless. The other folders were still created. To avoid the error, omit `src` from the command.

## Step 3: Copy the raw data

The three yearly CSV files were copied into `data\raw`:

```powershell
Copy-Item D:\<path-to-source>\bookings\*.csv C:\Users\<USER>\Downloads\bookings-project\data\raw
```

Verify the copy:

```powershell
cd data\raw
ls
```

```text
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         10/5/2026  10:39 AM         324107 Bookings_2023.csv
-a----         10/5/2026  10:39 AM         393937 Bookings_2024.csv
-a----         10/5/2026  10:40 AM         462895 Bookings_2025.csv
```

> **Path tip:** Relative paths in `Copy-Item` resolve from your *current* directory. When the source path was wrong (`..\Bookings\*.csv`), PowerShell reported `PathNotFound`. Use absolute paths or confirm your location with `pwd` first.

## Step 4: Add dependencies

Dependencies are covered in detail in [Dependency Management](03-dependency-management.md). In short:

```powershell
uv add pandas numpy pyarrow
uv add --dev ipykernel
```

You can run these from any subfolder of the project. uv searches parent directories for `pyproject.toml`.

## Step 5: Open the project in VS Code

```powershell
cd C:\Users\<USER>\Downloads\bookings-project
code .
```

Open the project **root** (the folder containing `pyproject.toml`). Opening a subfolder such as `data\raw` makes VS Code treat that subfolder as the workspace and can hide the project's `.venv` from the Python extension.

In VS Code, select the interpreter at `.venv\Scripts\python.exe` so notebooks use the project environment.

## Final structure

```text
bookings-project/
├── .venv/
├── data/
│   ├── raw/
│   │   ├── Bookings_2023.csv
│   │   ├── Bookings_2024.csv
│   │   └── Bookings_2025.csv
│   └── processed/
├── notebooks/
├── outputs/
├── src/
├── .python-version
├── pyproject.toml
├── README.md
└── uv.lock
```

## Recommended: add a `.gitignore` before pushing to GitHub

Do not commit the virtual environment or bulky data. A suitable starting point:

```gitignore
# Virtual environment
.venv/

# Python caches
__pycache__/
*.pyc
.ipynb_checkpoints/

# Data (keep folder structure, ignore contents)
data/raw/*
data/processed/*
!data/raw/.gitkeep
!data/processed/.gitkeep

# Generated outputs
outputs/*
!outputs/.gitkeep
```

Create the placeholder files so Git keeps the empty folders:

```powershell
New-Item data\raw\.gitkeep, data\processed\.gitkeep, outputs\.gitkeep -ItemType File
```

**Commit** `pyproject.toml`, `uv.lock` and `.python-version`. They let anyone recreate your exact environment.

## Next step

Continue to [Dependency Management](03-dependency-management.md).
