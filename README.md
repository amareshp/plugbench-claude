# plugbench

A personal [Claude Code](https://claude.com/claude-code) plugin marketplace. The plugins here
capture my standard software development and testing practices so Claude follows them
consistently across projects.

## Using this marketplace

Add the marketplace in Claude Code:

```
/plugin marketplace add amareshp/plugbench-claude
```

For a local checkout, use the path instead:

```
/plugin marketplace add /path/to/plugbench-claude
```

Install a plugin:

```
/plugin install python@plugbench
```

Pull the latest changes later with:

```
/plugin marketplace update plugbench
```

Skills trigger automatically when your request matches their description. You can also invoke
one directly as `/<plugin>:<skill>`, e.g. `/python:new-generic-app`.

## Plugins

### `python`

Python development workflows.

| Skill | Invoke | What it does |
|-------|--------|--------------|
| `new-generic-app` | `/python:new-generic-app` | Creates a new Python project with my standard setup (see below). |

**`new-generic-app` conventions:**
- `uv` for project and dependency management
- Python 3.13
- Asks whether to create the project in the current directory or a new one, and for the project name (default `my-app`)
- Source code in `src/`: entry point `src/main.py`, helper code in purpose-named packages (e.g. `src/utils/`); no package named after the project
- Starter code: `src/lib/my_math.py` with an `add` function, called from `src/main.py`, plus one test in `test/test_my_math.py`
- Tests in `test/`
- pytest for testing
- `.gitignore` covering Python artifacts, secrets, and local harness settings (e.g. `.claude/settings.local.json`)
- Lean tests: at most 2–3 readable tests per new function unless you ask for more

## Repository layout

```
.claude-plugin/marketplace.json   # Marketplace manifest
plugins/<plugin>/                 # One directory per plugin
  .claude-plugin/plugin.json
  skills/<skill>/SKILL.md
```

See [AGENTS.md](AGENTS.md) for contributor and agent conventions.
