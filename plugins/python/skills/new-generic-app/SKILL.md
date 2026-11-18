---
name: new-generic-app
description: Create a new generic Python project using uv, Python 3.13, a src/ layout, and pytest tests in test/. Use when the user asks to start, scaffold, bootstrap, or create a new Python project or app.
---

# New Generic Python App

Scaffold a new Python project that follows these conventions.

## Conventions

- **Package manager:** `uv`. Use it for everything: creating the project, adding dependencies
  (`uv add`, `uv add --dev`), and running commands (`uv run ...`). Don't use pip, poetry, or
  a hand-made venv.
- **Python version:** 3.13.
- **Source code:** all of it lives under `src/`.
- **Tests:** all of them live under `test/` (singular, not `tests/`).
- **Test framework:** pytest.

## Steps

1. Create the project with a src layout and pinned Python:
   ```bash
   uv init --package --python 3.13 <project-name>
   cd <project-name>
   ```
   This creates `pyproject.toml`, `.python-version` (3.13), and `src/<package_name>/__init__.py`.
2. Add pytest as a dev dependency:
   ```bash
   uv add --dev pytest
   ```
3. Create the `test/` directory and point pytest at it in `pyproject.toml`:
   ```toml
   [tool.pytest.ini_options]
   testpaths = ["test"]
   ```
4. Write a `.gitignore` (replace the minimal one `uv init` generates) with entries for a
   Python project **and** for local AI-harness/editor settings that must not be committed:
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
   Do **not** ignore `uv.lock` or `.python-version` — commit them. Keep shared harness files
   such as `.claude/settings.json` tracked.
5. Add a first test in `test/test_<module>.py` for the starter code.
6. Check that it works:
   ```bash
   uv run pytest
   ```

## Resulting layout

```
<project-name>/
├── pyproject.toml
├── uv.lock
├── .python-version
├── .gitignore
├── README.md
├── src/
│   └── <package_name>/
│       └── __init__.py
└── test/
    └── test_<module>.py
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

Example:

```python
from my_app.math_utils import add


def test_add_returns_sum_of_two_numbers():
    assert add(2, 3) == 5


def test_add_handles_negative_numbers():
    assert add(-2, -3) == -5
```
