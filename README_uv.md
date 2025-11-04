
## What is uv (in one line)?

A very fast, Rust‑based package manager that can act as a drop‑in replacement for `pip`/`pip-tools`/`virtualenv`, and also offers a higher‑level “project” workflow with a lockfile (`uv.lock`). ([astral.sh][1])

---

## Install uv

Pick one that fits your machine:

* macOS/Linux (standalone installer):

  ```bash
  curl -LsSf https://astral.sh/uv/install.sh | sh
  ```
* Windows (PowerShell):

  ```powershell
  powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
  ```
* Homebrew:

  ```bash
  brew install uv
  ```

(Other options: pipx/pip, WinGet, Scoop, Docker images, GitHub releases.) ([Astral Docs][2])

---

## Use uv as a **drop‑in pip** replacement

The quickest migration is “change `pip` to `uv pip`” and (optionally) “change `python -m venv` to `uv venv`”.

### Common command translations

| Task                | pip / friends                     | uv equivalent                        |
| ------------------- | --------------------------------- | ------------------------------------ |
| Create venv         | `python -m venv .venv`            | `uv venv`                            |
| Install packages    | `pip install pkg`                 | `uv pip install pkg`                 |
| Install from file   | `pip install -r requirements.txt` | `uv pip install -r requirements.txt` |
| Uninstall           | `pip uninstall pkg`               | `uv pip uninstall pkg`               |
| List                | `pip list`                        | `uv pip list`                        |
| Freeze              | `pip freeze > requirements.txt`   | `uv pip freeze > requirements.txt`   |
| Show package        | `pip show pkg`                    | `uv pip show pkg`                    |
| Tree                | `pipdeptree`                      | `uv pip tree`                        |
| Compile (pip‑tools) | `pip-compile req.in -o req.txt`   | `uv pip compile req.in -o req.txt`   |
| Sync (pip‑tools)    | `pip-sync req.txt`                | `uv pip sync req.txt`                |

Notes:

* `uv` prefers virtual environments by default: it auto‑uses `.venv` if present (even if not activated) and only installs to system Python if you pass `--system`. ([Astral Docs][3])
* `uv pip compile` can produce **cross‑platform** `requirements.txt` via `--universal`, so you don’t need a different lock per OS. ([Astral Docs][4])
* All of the package management subcommands above are documented under the **pip interface**. ([Astral Docs][5])

---

## Level up: uv’s **project** workflow (pyproject + uv.lock)

If you want reproducible, modern dependency management without juggling `requirements.in`/`requirements.txt`:

```bash
uv init myproj
cd myproj
uv add fastapi "pydantic>2"
uv run -- python -c "import fastapi, pydantic; print('ok')"
```

What this does:

* `uv init` scaffolds a `pyproject.toml`.
* `uv add` writes dependencies to `pyproject.toml`, updates `uv.lock`, and syncs `.venv`.
* `uv run` ensures the lockfile and environment are up to date, then runs your command.
  The lockfile (`uv.lock`) is cross‑platform and should be committed. ([Astral Docs][6])

Useful commands:

* Update lock only: `uv lock` (use `--upgrade` / `--upgrade-package` to bump).
* Sync env explicitly: `uv sync` (supports `--locked`/`--frozen` for CI).
* Export to requirements: `uv export --format requirements-txt -o requirements.txt`. ([Astral Docs][7])

---

## One‑liners for tools (like `pipx`/`npx`)

* Run a tool in a **temporary** env: `uvx ruff`, `uvx black --version`
* Install a **persistent** CLI: `uv tool install ruff` (then run `ruff` from your PATH)
  (`uvx` is an alias for `uv tool run`.) ([Astral Docs][8])

---

## Python versions without a separate pyenv

Install and pin interpreters directly:

```bash
uv python install 3.12
uv python pin 3.12
uv venv --python 3.12
```

uv can auto‑download Python on demand and can install PyPy as well. ([Astral Docs][9])

---

## CI patterns

### A. You keep requirements files (classic)

```bash
uv venv
uv pip install -r requirements.txt
# or enforce exact match to the lock
uv pip sync requirements.txt
```

(Replace `pip-compile` with `uv pip compile` in your update step.) ([Astral Docs][10])

### B. You use uv projects (recommended)

```bash
uv sync --frozen   # fail if uv.lock is out of date
pytest -q
```

`--frozen`/`--locked` prevents re-locking during CI, ensuring reproducibility. ([Astral Docs][7])

---

## Differences vs pip you should know (to avoid surprises)

* **Config files & env vars:** uv does **not** read `pip.conf`/`PIP_INDEX_URL`. Use `UV_INDEX_URL`, `uv.toml`, or `[tool.uv.pip]` in `pyproject.toml` instead. ([Astral Docs][11])
* **Default target:** uv installs to virtual environments by default and searches for `.venv`; use `--system` to opt into installing to system Python. ([Astral Docs][3])
* **Multiple indexes:** uv prefers the **first** index that has the package (safer against dependency confusion). You can change this with `--index-strategy`. ([Astral Docs][11])
* **Build isolation:** uv uses PEP 517 build isolation by default; add `--no-build-isolation` if needed. ([Astral Docs][11])

---

## Quick “cheat sheet” you can paste into a README

```bash
# Install
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh
# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# Project setup (uv workflow)
uv init
uv add numpy pandas
uv sync
uv run -- python -c "import numpy, pandas; print('ready')"

# Classic workflow (requirements files)
uv venv
uv pip install -r requirements.txt

# Locking
uv pip compile requirements.in -o requirements.txt
# Cross-platform lock
uv pip compile --universal requirements.in -o requirements.txt

# Tools (like pipx)
uvx ruff
uv tool install black
```

([Astral Docs][2])

---

[1]: https://astral.sh/blog/uv?utm_source=chatgpt.com "uv: Python packaging in Rust - Astral"
[2]: https://docs.astral.sh/uv/getting-started/installation/ "Installation | uv"
[3]: https://docs.astral.sh/uv/pip/environments/ "Using environments | uv"
[4]: https://docs.astral.sh/uv/guides/migration/pip-to-project/ "From pip to a uv project | uv"
[5]: https://docs.astral.sh/uv/pip/packages/ "Managing packages | uv"
[6]: https://docs.astral.sh/uv/guides/projects/ "Working on projects | uv"
[7]: https://docs.astral.sh/uv/reference/cli/ "Commands | uv"
[8]: https://docs.astral.sh/uv/guides/tools/ "Using tools | uv"
[9]: https://docs.astral.sh/uv/guides/install-python/ "Installing and managing Python | uv"
[10]: https://docs.astral.sh/uv/pip/compile/ "Locking environments | uv"
[11]: https://docs.astral.sh/uv/pip/compatibility/ "Compatibility with pip | uv"
