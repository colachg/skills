# skills

A personal Claude Code **plugin marketplace** for storing and installing my own skills.

## Repository layout

```
skills/
├── .claude-plugin/
│   └── marketplace.json          # marketplace manifest (lists plugins)
├── plugins/
│   └── colachg/                  # the plugin
│       ├── .claude-plugin/
│       │   └── plugin.json        # plugin manifest
│       └── skills/                # skills live here (auto-discovered)
│           └── <skill-name>/
│               └── SKILL.md
└── README.md
```

Skills in `plugins/colachg/skills/` are auto-discovered — no need to list them in `plugin.json`.

## Install in Claude Code

Push this repo to GitHub, then in Claude Code:

```
/plugin marketplace add <your-github-user>/skills
/plugin install colachg@skills
```

The marketplace is named `skills` (see `.claude-plugin/marketplace.json`) and
ships a plugin named `colachg`, so the install spec is `colachg@skills`
(`<plugin>@<marketplace>`).

To develop locally without pushing, point the marketplace at the local path:

```
/plugin marketplace add /Users/colachen/Experiments/GitHub/my-skills
/plugin install colachg@skills
```

After pushing changes, refresh with:

```
/plugin marketplace update
```

## Add a new skill

Create a folder under `plugins/colachg/skills/` matching the skill name, with a `SKILL.md` inside:

```markdown
---
name: my-skill
description: One or two sentences telling Claude exactly when to use this skill. Max 1024 chars.
---

# My Skill

Instructions for Claude go here. Reference supporting files (scripts/, references/)
in the same folder as needed.
```

Frontmatter rules:
- `name`: lowercase letters, numbers, and hyphens only; must match the folder name (max 64 chars).
- `description`: required; this is what triggers the skill, so be specific about *when* to use it.
- Optional: `version`, `allowed-tools`, `disable-model-invocation`.
