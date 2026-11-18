---
name: new-generic-app
description: Create a new generic Python project using uv, Python 3.13, a src/ layout with src/main.py as the entry point, and pytest tests in test/. Use when the user asks to start, scaffold, bootstrap, or create a new Python project or app.
---

# New Generic Python App

Scaffold a new Python project that follows these conventions.

## Conventions

- **Package manager:** `uv`. Use it for everything: creating the project, adding dependencies
  (`uv add`, `uv add --dev`), and running commands (`uv run ...`). Don't use pip, poetry, or
  a hand-made venv.
- **Python version:** 3.13.
- **Source code:** all of it lives under `src/`.
  - The entry point is `src/main.py`.
  - Helper code goes in packages under `src/` named for what they do, e.g.
    `src/config/`, `src/services/`, `src/utils/` (each with an `__init__.py`).
  - Do **not** create a package named after the project (no `src/my_app/`).
- **Tests:** all of them live under `test/` (singular, not `tests/`).
- **Test framework:** pytest.

## Steps

1. **Ask where to create the project** before creating any files. Ask the user both:
   - **Location:** the current directory, or a new subdirectory of the current directory?
   - **Project name:** defaults to `my-app`.

   If the user picks a new directory, create it as `./<project-name>` and `cd` into it.
   Run all later steps from the project root.

2. **Initialize the project** with uv and pin Python 3.13:
   ```bash
   uv init --bare --python 3.13 --name <project-name> .
   uv python pin 3.13
   ```
   `--bare` creates only `pyproject.toml` (no sample package or `main.py`), so the `src/`
   layout below is created by hand. `uv python pin` writes `.python-version`.

3. **Add pytest** as a dev dependency:
   ```bash
   uv add --dev pytest
   ```

4. **Configure pytest** in `pyproject.toml`. `pythonpath = ["src"]` lets tests import the
   modules in `src/` directly:
   ```toml
   [tool.pytest.ini_options]
   testpaths = ["test"]
   pythonpath = ["src"]
   ```

5. **Create the starter source code.** Every new project starts with exactly this:
   - `src/lib/__init__.py`: empty file.
   - `src/lib/my_math.py`:
     ```python
     def add(a: float, b: float) -> float:
         """Return the sum of a and b."""
         return a + b
     ```
   - `src/main.py`, which calls `add` with two numbers:
     ```python
     from lib.my_math import add


     def main() -> None:
         result = add(2, 3)
         print(f"2 + 3 = {result}")


     if __name__ == "__main__":
         main()
     ```
   Later helper code goes in purpose-named packages under `src/` (or in `lib/`), each with
   an `__init__.py`, imported by package name.

6. **Write a `.gitignore`** covering the Python project **and** the local AI-harness and
   editor settings that must not be committed:
   ```gitignore
   # Python
   __pycache__/
   *.py[cod]
   *.egg-info/
   build/
   dist/
   wheels/

   # Virtual environments
   .venv/

   # Tooling caches & coverage
   .pytest_cache/
   .ruff_cache/
   .mypy_cache/
   .coverage
   htmlcov/

   # Secrets / environment
   .env
   .env.*
   !.env.example

   # Local harness settings (per-user, not shared)
   .claude/settings.local.json
   CLAUDE.local.md

   # OS / editors
   .DS_Store
   .idea/
   .vscode/
   ```
   If a `.gitignore` already exists (e.g. when using the current directory), merge these
   entries into it instead of overwriting. Do **not** ignore `uv.lock` or `.python-version`;
   commit them. Keep shared harness files such as `.claude/settings.json` tracked.

7. **Add one test** for `add` in `test/test_my_math.py`:
   ```python
   from lib.my_math import add


   def test_add_returns_sum_of_two_numbers():
       assert add(2, 3) == 5
   ```

8. **Check that it works:**
   ```bash
   uv run python src/main.py
   uv run pytest
   ```

## Resulting layout

```
<project-root>/
├── pyproject.toml
├── uv.lock
├── .python-version
├── .gitignore
├── src/
│   ├── main.py
│   └── lib/
│       ├── __init__.py
│       └── my_math.py
└── test/
    └── test_my_math.py
```

## Writing tests

- **Keep it small.** Write at most **2–3 tests per new function**. Cover the main case and one or
  two important edge cases. Only write more if the user asks for them.
- **Make tests readable.** Each test should be easy to follow at a glance:
  - Give it a descriptive name that states the expected behavior, e.g.
    `test_parse_date_returns_none_for_empty_string`.
  - Keep each test short and focused on one behavior.
  - Use plain `assert` statements with clear expected values.
  - Avoid clever abstractions, deep fixture chains, and heavy parametrization that hide what is
    being tested.

Example of a fuller set (`test/test_my_math.py`, testing `src/lib/my_math.py`):

```python
from lib.my_math import add


def test_add_returns_sum_of_two_numbers():
    assert add(2, 3) == 5


def test_add_handles_negative_numbers():
    assert add(-2, -3) == -5
```
