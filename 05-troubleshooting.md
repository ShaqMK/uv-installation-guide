# 5. Troubleshooting

[← Command Reference](04-command-reference.md) · [README](../README.md)

Problems encountered during the real setup session, plus other common Windows issues.

## Table of Contents

- [`uv` is not recognized](#uv-is-not-recognized)
- [`mkdir` says an item already exists](#mkdir-says-an-item-already-exists)
- [`Copy-Item` cannot find the path](#copy-item-cannot-find-the-path)
- [`ls -la` fails in PowerShell](#ls--la-fails-in-powershell)
- [Garbled PATH line in installer output](#garbled-path-line-in-installer-output)
- [Cannot activate the virtual environment](#cannot-activate-the-virtual-environment)
- [VS Code does not see the environment](#vs-code-does-not-see-the-environment)
- [Duplicate files like `Bookings_2023 (1).csv`](#duplicate-files-like-bookings_2023-1csv)

---

## `uv` is not recognized

**Symptom**

```text
uv : The term 'uv' is not recognized as the name of a cmdlet, function, script file, or operable program.
```

**Cause:** The folder `C:\Users\<USER>\.local\bin` is not on PATH in the current terminal.

**Fix:** Restart the terminal, or add it for the current session:

```powershell
$env:Path = "C:\Users\<USER>\.local\bin;$env:Path"
```

---

## `mkdir` says an item already exists

**Symptom**

```text
mkdir : An item with the specified name C:\...\bookings-project\src already exists.
```

**Cause:** `uv init` already created `src`. PowerShell reports the error for that one item but still creates the others (`data\raw`, `data\processed`, `notebooks`, `outputs`).

**Fix:** No action needed. Next time, leave `src` out of the command, or use `-Force`:

```powershell
mkdir -Force data\raw, data\processed, notebooks, outputs
```

---

## `Copy-Item` cannot find the path

**Symptom**

```text
Copy-Item : Cannot find path 'C:\...\bookings-project\Bookings' because it does not exist.
```

**Cause:** The source path was relative (`..\Bookings\*.csv`) and resolved from the *current* folder, where no `Bookings` directory exists. The destination `data\raw` was also relative, so it would have pointed at `data\data\raw` from inside `data`.

**Fix:** Use absolute paths, or move to the project root first:

```powershell
cd C:\Users\<USER>\Downloads\bookings-project
Copy-Item D:\<path-to-source>\bookings\*.csv data\raw
```

---

## `ls -la` fails in PowerShell

**Symptom**

```text
Get-ChildItem : A parameter cannot be found that matches parameter name 'la'.
```

**Cause:** `ls` is an alias for `Get-ChildItem`, which does not accept Unix flags.

**Fix:**

```powershell
ls                    # list items
ls -Force             # include hidden items
```

---

## Garbled PATH line in installer output

**Symptom:** The installer's PowerShell hint looked like this:

```text
$env:Path = "C:  sers\ADMIN\.local\bin;$env:Path"
```

**Cause:** `\U` in `C:\Users` was interpreted as an escape sequence somewhere between the installer and the terminal, mangling the path.

**Fix:** Type the correct path yourself:

```powershell
$env:Path = "C:\Users\<USER>\.local\bin;$env:Path"
```

---

## Cannot activate the virtual environment

**Symptom**

```text
.venv\Scripts\Activate.ps1 cannot be loaded because running scripts is disabled on this system.
```

**Fix (recommended):** Skip activation and use `uv run`, which needs no execution-policy change.

**Alternative:** Allow local scripts for your user account:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

---

## VS Code does not see the environment

**Cause:** VS Code was opened on a subfolder (for example `data\raw`) instead of the project root.

**Fix:**

1. Close that window.
2. From the project root, run `code .`
3. Press `Ctrl+Shift+P`, choose **Python: Select Interpreter**, and pick `.venv\Scripts\python.exe`.
4. For notebooks, select the same `.venv` kernel (requires `ipykernel`).

---

## Duplicate files like `Bookings_2023 (1).csv`

**Cause:** Browsers append `(1)` when a file with the same name already exists in Downloads. The session's Downloads folder held two byte-identical copies of each Bookings CSV (same sizes).

**Fix:** Keep one copy per file. Only the clean names (`Bookings_2023.csv`, and so on) were copied into `data\raw`. Delete the `(1)` duplicates from Downloads to avoid confusion.

---

## Still stuck?

```powershell
uv --version          # confirm uv works
uv python find        # confirm a Python interpreter is found
uv sync -v            # verbose output for environment problems
```

See the [official uv documentation](https://docs.astral.sh/uv/) or the [issue tracker](https://github.com/astral-sh/uv/issues).
