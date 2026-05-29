# CLAUDE.md

This repository is a **Claude Code plugin marketplace** for distributing personal skills.
When working here, follow these conventions.

## Repository layout

```
skills/
├── .claude-plugin/
│   └── marketplace.json          # marketplace manifest (lists plugins)
└── plugins/
    └── colachg/                  # the plugin (plugin name)
        ├── .claude-plugin/
        │   └── plugin.json        # plugin manifest
        └── skills/                # skills live here — AUTO-DISCOVERED
            └── <skill-name>/
                ├── SKILL.md
                ├── references/    # optional, loaded on demand
                └── scripts/       # optional, loaded on demand
```

## How to add a new skill

1. Create a folder under `plugins/colachg/skills/` named in **kebab-case** (lowercase letters, numbers, hyphens), e.g. `plugins/colachg/skills/my-new-skill/`.
2. Add a `SKILL.md` inside it with this frontmatter:

   ```markdown
   ---
   name: my-new-skill
   description: One or two sentences stating exactly WHEN Claude should use this skill (the trigger), not just what it does. Max 1024 chars.
   ---

   # My New Skill

   Instructions for Claude go here.
   ```

3. Put any supporting files (`references/`, `scripts/`, assets) in the same skill folder. Reference them by relative path from `SKILL.md`; they load on demand.

**Hard rules:**

- `name:` in the frontmatter MUST exactly match the skill's folder name (kebab-case, max 64 chars).
- `description:` is REQUIRED and is what makes Claude trigger the skill — describe the *situation/when*, include trigger keywords, keep it specific.
- Do NOT edit `marketplace.json` or `plugin.json` to register a skill — skills in `skills/` are auto-discovered.
- One skill per folder; never put two `SKILL.md` files in the same directory.
- Keep `SKILL.md` focused; move long lookup tables, examples, or code into `references/`/`scripts/` so the main file stays lean.

## After adding or changing a skill

1. Commit using the **Gitmoji + Conventional Commits** style (this repo ships the `gitmoji-commit` skill — use it). New skill → `✨ feat: add <name> skill`.
2. Optionally bump `version` in `plugins/colachg/.claude-plugin/plugin.json`.
3. Push.

## Installing / refreshing in Claude Code

```
/plugin marketplace add <github-user>/skills      # first time (repo name)
/plugin marketplace update                          # pull latest after pushing
/plugin install colachg@skills                      # install / reinstall (plugin@marketplace)
```

## Validation before committing

- `python3 -c "import json; json.load(open('.claude-plugin/marketplace.json'))"` — manifest parses.
- `python3 -c "import json; json.load(open('plugins/colachg/.claude-plugin/plugin.json'))"` — plugin manifest parses.
- Every `skills/*/SKILL.md` has `name` matching its folder and a non-empty `description`.
