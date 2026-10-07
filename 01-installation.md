# 1. Installation and Prerequisites

[← Back to README](../README.md) · [Next: Project Setup →](02-project-setup.md)

## Table of Contents

- [What is uv?](#what-is-uv)
- [Prerequisites](#prerequisites)
- [Install uv](#install-uv)
  - [What gets installed](#what-gets-installed)
  - [Alternative installation methods](#alternative-installation-methods)
- [Add uv to your PATH](#add-uv-to-your-path)
- [Verify the installation](#verify-the-installation)
- [Updating and uninstalling](#updating-and-uninstalling)
- [Next step](#next-step)

## What is uv?

uv is an extremely fast Python package and project manager written in Rust. A single tool replaces several you may already use:

| Replaces | With uv |
|----------|---------|
| `pip` | `uv pip`, `uv add` |
| `venv` / `virtualenv` | `uv venv` (automatic in projects) |
| `pip-tools` | `uv lock`, `uv sync` |
| `pipx` | `uv tool`, `uvx` |
| `pyenv` | `uv python` |

## Prerequisites

| Requirement | Needed? | Notes |
|-------------|---------|-------|
| Windows 10 or later (x86_64) | Yes | The session used the `x86_64-pc-windows-msvc` build |
| PowerShell | Yes | Used to run the installer script |
| Internet access | Yes | To download uv and packages |
| Python pre-installed | No | uv can find an existing interpreter or download one itself |
| Git | Optional | Recommended for version control and publishing to GitHub |
| VS Code | Optional | Used in the session via the `code .` command |

> **Note:** No administrator rights are required. uv installs into your user profile.

## Install uv

Run the official installer in PowerShell:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

What this does:

- `irm` (`Invoke-RestMethod`) downloads the installer script from `astral.sh`.
- `iex` (`Invoke-Expression`) executes it.
- `-ExecutionPolicy ByPass` lets the script run for this one command only, without changing your system-wide policy.

Expected output:

```text
downloading uv 0.12.23 (x86_64-pc-windows-msvc)
installing to C:\Users\<USER>\.local\bin
  uv.exe
  uvx.exe
  uvw.exe
everything's installed!
```

### What gets installed

| Binary | Purpose |
|--------|---------|
| `uv.exe` | The main uv command |
| `uvx.exe` | Shortcut for `uv tool run` (run tools without installing them) |
| `uvw.exe` | Windowless variant, for launching GUI scripts without a console window |

They are placed in `C:\Users\<USER>\.local\bin`.

### Alternative installation methods

```powershell
# WinGet
winget install --id=astral-sh.uv -e

# Scoop
scoop install main/uv

# pip (if Python is already installed)
pip install uv
```

## Add uv to your PATH

The installer prints instructions for making `uv` available in the current terminal. Either **restart your shell** (simplest) or update PATH for the current session only:

```powershell
# PowerShell
$env:Path = "C:\Users\<USER>\.local\bin;$env:Path"
```

```bat
:: Command Prompt (cmd)
set Path=C:\Users\<USER>\.local\bin;%Path%
```

> Replace `<USER>` with your Windows username. The installer normally updates your user PATH permanently, so new terminals pick it up automatically.

## Verify the installation

```powershell
uv --version
```

```text
uv 0.12.23 (46b84fd0b 2026-10-03 x86_64-pc-windows-msvc)
```

If you see a version number, uv is ready. If you get "command not found", see [Troubleshooting](05-troubleshooting.md#uv-is-not-recognized).

## Updating and uninstalling

```powershell
# Update uv to the latest version
uv self update

# Remove cached data (optional)
uv cache clean
```

To uninstall completely, delete the binaries from `C:\Users\<USER>\.local\bin` and remove uv's data directories. Run `uv cache dir` and `uv python dir` to find them.

## Next step

With uv installed, continue to [Project Setup](02-project-setup.md).
