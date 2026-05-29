---
name: gitmoji-commit
description: 'Plan and draft git commits in the Gitmoji + Conventional Commits style — a leading emoji (✨, 🐛, ♻️ …) plus a conventional type, like "✨ feat: add avatar upload". Splits a working tree into a clean sequence of reviewable, logically-grouped commits, isolates lockfiles into their own final commit, and hands back signed, copy-paste git commands. Use whenever the user wants help committing changes, writing/improving a commit message, asks "how should I commit this", wants to break work into reviewable commits, or mentions gitmoji/git-emoji/emoji commits. The user runs git themselves — this skill produces messages and commands, it does not commit.'
---

# Gitmoji Commit

Help the user turn a pile of working changes into a clean sequence of commits, each in the **Gitmoji + Conventional Commits** style: a leading emoji that signals the *kind* of change, then a conventional type and an imperative subject.

```
✨ feat: add avatar upload to the profile page
```

The user runs `git` themselves. Your job is to **plan the commits and produce the messages + commands** they can copy — not to run `git add` or `git commit`.

## The workflow

1. **Survey everything that changed.** Don't assume the right things are staged. Inspect the whole working tree (all read-only and safe):
   - `git status` — staged, unstaged, and untracked files at a glance
   - `git diff --staged` and `git diff` — the actual content of staged and unstaged edits
   - For untracked files, look at the new files themselves
   Read the substance, not just filenames: a change under `tests/` that backs a new feature is part of that feature's story; a `.md` file might be user docs or just a code comment.

2. **Group changes into reviewable commits.** This is the heart of it. Aim for **atomic commits** — each commit is one coherent concern that a reviewer can understand on its own. The point is reviewability: someone reading the history should be able to follow your reasoning commit by commit.
   - Keep *related* edits together (a feature + its tests + its doc note can be one commit if they tell one story).
   - Split *unrelated* edits apart (a bug fix and an unrelated config tweak are two commits, even if both are staged right now).
   - Separate **mechanical noise from logic**: pure formatting, renames, or generated files shouldn't be buried in a substantive diff — give them their own commit so reviewers can skim past them.
   - Default to **file-level grouping** so the staging recipe is clean copy-paste (`git add path/a path/b`). Only when a *single file* genuinely mixes two concerns, suggest interactive `git add -p` — and tell the user it's interactive (they pick hunks).
   - **Dependencies:** use judgment. When a feature directly uses a dependency it adds (the manifest edit and the code tell one story and build together), bundle the manifest — e.g. `package.json`, `pyproject.toml`, `Cargo.toml` — with the feature commit. When the dependency change is independent (a routine upgrade, an unrelated bump), give it its own commit. Either way, the generated **lockfile** is always separate and last (next section).

3. **Order the sequence sensibly.** Order so each commit stands on its own and, ideally, the tree builds at every step. A light heuristic: foundational/prep changes first, the feature or fix that uses them next, and mechanical/generated changes (formatting, lockfiles) last so they don't bury the real diff. **Lockfiles are always the final, separate commit — see below.**

4. **For each commit, produce three things:** the gitmoji message (subject, plus a *why*-focused body when non-trivial), the `git add` command(s) that stage exactly that group, and a signed `git commit` command.

5. **Hand over the recipe.** Present the commits as an ordered, runnable sequence. Don't run any of it — staging and committing are the user's calls.

## Lockfiles go last, in their own commit

In JavaScript/TypeScript/bun projects, dependency lockfiles get their **own commit, ordered last**, never mixed with source changes:

- `package-lock.json`, `npm-shrinkwrap.json`, `yarn.lock`, `pnpm-lock.yaml`, `bun.lock`, `bun.lockb`

**Why:** lockfiles are large, generated, and conflict constantly on merge. When they conflict, the clean resolution is usually to regenerate (`npm install`, `pnpm install`, `bun install`) rather than hand-merge. Keeping them isolated in the last commit means that regeneration touches exactly one commit and never entangles your actual source changes. The same principle applies to other ecosystems' lockfiles (`Cargo.lock`, `poetry.lock`, `composer.lock`) — isolate them too if they show up.

Use 📦️ `build` (or ➕/➖/⬆️ when it's specifically adding/removing/upgrading a dep) for the lockfile commit, e.g. `📦️ build: update package-lock.json`.

## Sign each commit

Add `-S` to every suggested `git commit` command so commits are cryptographically signed:

```bash
git commit -S -m "✨ feat: ..."
```

Before presenting the recipe, check whether signing is already configured:
- `git config --get commit.gpgsign` (if `true`, signing happens automatically — `-S` is harmless and explicit, keep it)
- `git config --get user.signingkey`

If neither is set, still include `-S`, but add a one-line note that it will fail until the user configures a signing key (GPG or SSH), pointing them to `git config user.signingkey` and `gpg.format`. If signing is already on, no note is needed.

## Message format rules

- **Subject:** `<emoji> <type>: <description>` — e.g. `🐛 fix: handle null user in payment processor`.
  - Imperative mood ("add", "fix", "remove" — not "added"/"fixes").
  - Lowercase the first word of the description; no trailing period.
  - Aim for ~50 characters; hard-stop around 72.
  - One space after the emoji. Some emoji (🚑️ 🔒️ 📦️) carry a variation selector — copy them as-is from the list.
- **Scope (optional):** add a parenthesized scope when it clarifies *where* the change lives: `✨ feat(auth): add SSO login`. Use it when the repo has clear modules; skip it when it'd be noise.
- **Body (optional, encouraged for non-trivial changes):** blank line after the subject, then explain the **why** and context a reviewer needs — not a restatement of the diff. Wrap at ~72 characters.
- **Breaking changes:** 💥 with a `!` after the type and a `BREAKING CHANGE:` footer, e.g. `💥 feat!: drop support for Node 16`.
- **Issue references:** add a footer like `Refs: #123` or `Closes: #123` when the user mentions a ticket.

## Emoji → type mapping (common cases)

Pick by intent — the single emoji that fits the dominant change; the type follows from it. The full official list of all 73 gitmojis is in `references/gitmojis.md`; consult it for cases not below (accessibility, i18n, feature flags, database, secrets, healthchecks, etc.).

| Intent | Emoji | Type |
|---|---|---|
| Introduce a new feature | ✨ | feat |
| Fix a bug | 🐛 | fix |
| Critical hotfix | 🚑️ | fix |
| Simple fix for a non-critical issue | 🩹 | fix |
| Fix a security/privacy issue | 🔒️ | fix |
| Add or update documentation | 📝 | docs |
| Add or update comments in source | 💡 | docs |
| Refactor code (no behavior change) | ♻️ | refactor |
| Improve code structure / format | 🎨 | refactor |
| Remove code or files | 🔥 | refactor |
| Remove dead code | ⚰️ | refactor |
| Move or rename resources | 🚚 | refactor |
| Improve performance | ⚡️ | perf |
| Add, update, or pass tests | ✅ | test |
| Add a failing test (TDD) | 🧪 | test |
| Add or update UI / style files | 💄 | style |
| Fix compiler / linter warnings | 🚨 | style |
| Add or update config files | 🔧 | chore |
| Work in progress | 🚧 | chore |
| Add or update dev scripts | 🔨 | chore |
| Add or update logs | 🔊 | chore |
| Add a dependency | ➕ | build |
| Remove a dependency | ➖ | build |
| Upgrade dependencies | ⬆️ | build |
| Downgrade dependencies | ⬇️ | build |
| Pin dependencies | 📌 | build |
| Add or update compiled files / packages / lockfiles | 📦️ | build |
| Add or update CI build system | 👷 | ci |
| Fix CI build | 💚 | ci |
| Deploy stuff | 🚀 | chore |
| Release / version tag | 🔖 | chore |
| Revert changes | ⏪️ | revert |
| Merge branches | 🔀 | — (merge) |
| Begin a project | 🎉 | chore |
| Fix typos | ✏️ | fix |
| Introduce breaking changes | 💥 | feat!/fix! |
| Add or update types | 🏷️ | feat |

When more than one row fits, prefer the more specific one (🚑️ over 🐛 for a true hotfix; ⚰️ over 🔥 when the point is removing dead code).

## Worked example

Suppose `git status` shows: a new debounce on a search input (`search.js`) plus its test (`search.test.js`), the `lodash.debounce` dependency it uses added to `package.json`, an unrelated typo fix in `README.md`, and a regenerated `package-lock.json`.

The feature directly uses the new dependency, so the manifest rides with it. That's two concerns + a lockfile → three commits, lockfile last:

```bash
# 1 — the feature, its test, and the dependency it needs (one buildable story)
git add src/search.js src/search.test.js package.json
git commit -S -m "✨ feat(search): debounce query input" -m "Searching fired a request on every keystroke, hammering the API on fast typers. Debounce by 300ms so we only query once typing settles. Adds lodash.debounce and a test covering the timing."

# 2 — unrelated docs typo, on its own
git add README.md
git commit -S -m "✏️ fix: correct typo in README setup steps"

# 3 — lockfile last, isolated so a merge conflict can just be regenerated
git add package-lock.json
git commit -S -m "📦️ build: update package-lock.json"
```

For a single-commit change with no body, one `-m` is enough:

```bash
git add report.py
git commit -S -m "♻️ refactor(report): simplify build with a line-total helper"
```

If the user prefers to commit everything as one (declining the split), pick the emoji for the most significant change and say what you set aside.
