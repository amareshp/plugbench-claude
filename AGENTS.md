# AGENTS.md

Guidance for AI coding agents (Claude Code and others) working in this repository.

## What this repo is

`plugbench` is a personal **Claude Code plugin marketplace**. Each plugin packages the owner's
standard software development and testing practices as skills (and later possibly commands,
agents, or hooks) that can be installed into Claude Code.

## Layout

```
.claude-plugin/marketplace.json      # Marketplace manifest: lists every plugin
plugins/<plugin>/
  .claude-plugin/plugin.json         # Plugin manifest (name, description, version)
  skills/<skill>/SKILL.md            # One directory per skill
AGENTS.md                            # This file (CLAUDE.md imports it)
README.md                            # User-facing docs for the marketplace and its plugins
```

## Current plugins

| Plugin   | Skills            | Purpose                                         |
|----------|-------------------|-------------------------------------------------|
| `python` | `new-generic-app` | Scaffold a new Python project: asks for location/name (default `my-app`), uv, Python 3.13, `src/main.py` + purpose-named helper packages under `src/`, starter `src/lib/my_math.py` `add()` with one test, pytest tests in `test/`, 2–3 readable tests per function, `.gitignore` incl. local harness settings. |

## Conventions

- Plugin and skill names are lowercase kebab-case. A skill's directory name must match the
  `name` in its `SKILL.md` frontmatter.
- Every `SKILL.md` has YAML frontmatter with `name` and `description`. The description says
  **what** the skill does and **when** to use it — Claude uses it to decide when to trigger.
- Keep `SKILL.md` focused; move long reference material into sibling files in the skill
  directory and link to them.
- Versions use semver. Bump the plugin's `version` in **both** `plugins/<plugin>/.claude-plugin/plugin.json`
  and its entry in `.claude-plugin/marketplace.json` when its behavior changes.

## Adding a plugin

1. Create `plugins/<name>/.claude-plugin/plugin.json`.
2. Add skills under `plugins/<name>/skills/<skill>/SKILL.md`.
3. Register the plugin in `.claude-plugin/marketplace.json` with `"source": "./plugins/<name>"`.
4. Update the plugin table above and the plugin section in `README.md`.

## Keeping docs in sync (required)

Whenever you make a significant change — adding/removing a plugin or skill, or changing how a
plugin or skill behaves — update in the same change:

- **AGENTS.md**: the "Current plugins" table and any affected conventions.
- **README.md**: the plugin catalog and usage notes for the affected plugin/skill.

## Validating

Run `claude plugin validate .` from the repo root to check the marketplace and plugin manifests
before committing.
