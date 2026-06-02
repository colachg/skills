---
name: gitmoji-commit
description: 'Plan and create git commits in the Gitmoji + Conventional Commits style — a leading emoji (✨, 🐛, ♻️ …) plus a conventional type, like "✨ feat: add avatar upload". Splits a working tree into a clean sequence of reviewable, logically-grouped commits, isolates lockfiles into their own final commit, and runs the signed git commands. Use whenever the user wants help committing changes, writing/improving a commit message, asks "how should I commit this", wants to break work into reviewable commits, or mentions gitmoji/git-emoji/emoji commits. Shows the plan, then runs git add and git commit itself.'
---

# Gitmoji Commit

Turn a pile of working changes into a clean sequence of commits, each styled as an emoji + conventional type + imperative subject:

```
✨ feat: add avatar upload to the profile page
```

**You plan, show the plan, then run it.** Lay out the commit sequence so the user can see what's coming, then run the `git add` and `git commit` commands yourself.

## Workflow

1. **Survey everything.** Don't trust what's staged. Run `git status`, `git diff --staged`, `git diff`, and open untracked files. Read the substance, not filenames — a `tests/` change may belong to a feature; a `.md` file may be docs or just a comment.

2. **Group into atomic commits.** Each commit = one coherent concern a reviewer can follow on its own.
   - Keep related edits together (feature + its tests + its doc note = one story).
   - Split unrelated edits apart, even if staged together now.
   - Separate mechanical noise (formatting, renames, generated files) from logic — give it its own commit.
   - Default to **file-level grouping** for clean copy-paste (`git add path/a path/b`). Only suggest `git add -p` when one file mixes two concerns — and warn it's interactive.
   - **Dependencies:** bundle the manifest (`package.json`, `pyproject.toml`, `Cargo.toml`) with the feature when the feature uses the dep it adds. Give independent bumps their own commit. The **lockfile is always separate and last** (see below).

3. **Order sensibly.** Foundational/prep first, the feature/fix that uses them next, mechanical/generated changes (formatting, lockfiles) last. Ideally the tree builds at every step.

4. **For each commit, prepare three things:** the gitmoji message (subject + a *why*-focused body when non-trivial), the `git add` command(s) staging exactly that group, and a signed `git commit`.

5. **Show the plan, then run it.** Present the commits as an ordered sequence so the user sees what's about to happen, then execute the `git add` / `git commit` commands in order. If a commit fails (e.g. signing not configured, pre-commit hook rejects), stop, report what happened, and don't blindly continue the sequence.

## Lockfiles go last, in their own commit

Lockfiles always get their own commit, ordered last, never mixed with source:

`package-lock.json`, `npm-shrinkwrap.json`, `yarn.lock`, `pnpm-lock.yaml`, `bun.lock`, `bun.lockb` — and other ecosystems' lockfiles (`Cargo.lock`, `poetry.lock`, `composer.lock`).

**Why:** they're large, generated, and conflict constantly. Conflicts are resolved by regenerating (`npm install`, etc.), not hand-merging. Isolating them keeps that regeneration off your real source changes.

Use 📦️ `build` (or ➕/➖/⬆️ for add/remove/upgrade), e.g. `📦️ build: update package-lock.json`.

## Sign each commit

Add `-S` to every `git commit` so commits are signed:

```bash
git commit -S -m "✨ feat: ..."
```

Check whether signing is configured **before running anything**: `git config --get commit.gpgsign` and `git config --get user.signingkey`. If neither is set, `-S` will make the commit fail — so don't run it. Instead, tell the user signing isn't set up (point to `git config user.signingkey` / `gpg.format`) and ask whether to commit without `-S` or hold off while they configure a key.

## Message format

- **Subject:** `<emoji> <type>: <description>` — e.g. `🐛 fix: handle null user in payment processor`.
  - Imperative mood ("add", "fix" — not "added"). Lowercase first word, no trailing period.
  - Aim ~50 chars, hard-stop ~72. One space after the emoji. Some emoji (🚑️ 🔒️ 📦️) carry a variation selector — copy them as-is.
- **Scope (optional):** `✨ feat(auth): add SSO login` — use when the repo has clear modules, skip when it'd be noise.
- **Body (optional, for non-trivial changes):** blank line, then the **why** and context — not a restatement of the diff. Wrap ~72 chars.
- **Breaking changes:** 💥 with `!` after the type and a `BREAKING CHANGE:` footer, e.g. `💥 feat!: drop support for Node 16`.
- **Issue refs:** footer like `Refs: #123` / `Closes: #123` when a ticket is mentioned.

## Emoji → type (common cases)

Pick the single emoji that fits the dominant change; the type follows. Full list of all 73 in `references/gitmojis.md`. When two rows fit, prefer the more specific (🚑️ over 🐛 for a hotfix; ⚰️ over 🔥 for dead code).

| Intent | Emoji | Type |
|---|---|---|
| New feature | ✨ | feat |
| Bug fix | 🐛 | fix |
| Critical hotfix | 🚑️ | fix |
| Simple non-critical fix | 🩹 | fix |
| Security/privacy fix | 🔒️ | fix |
| Fix typos | ✏️ | fix |
| Documentation | 📝 | docs |
| Comments in source | 💡 | docs |
| Refactor (no behavior change) | ♻️ | refactor |
| Improve structure / format | 🎨 | refactor |
| Remove code or files | 🔥 | refactor |
| Remove dead code | ⚰️ | refactor |
| Move or rename | 🚚 | refactor |
| Performance | ⚡️ | perf |
| Tests | ✅ | test |
| Failing test (TDD) | 🧪 | test |
| UI / style files | 💄 | style |
| Compiler / linter warnings | 🚨 | style |
| Config files | 🔧 | chore |
| Work in progress | 🚧 | chore |
| Dev scripts | 🔨 | chore |
| Logs | 🔊 | chore |
| Deploy | 🚀 | chore |
| Release / version tag | 🔖 | chore |
| Begin a project | 🎉 | chore |
| Add dependency | ➕ | build |
| Remove dependency | ➖ | build |
| Upgrade dependencies | ⬆️ | build |
| Downgrade dependencies | ⬇️ | build |
| Pin dependencies | 📌 | build |
| Lockfiles / compiled packages | 📦️ | build |
| Add/update CI | 👷 | ci |
| Fix CI build | 💚 | ci |
| Revert | ⏪️ | revert |
| Merge branches | 🔀 | (merge) |
| Breaking changes | 💥 | feat!/fix! |
| Types | 🏷️ | feat |

## Worked example

`git status` shows: a new debounce on a search input (`search.js`) + its test (`search.test.js`), the `lodash.debounce` dep it uses in `package.json`, an unrelated typo fix in `README.md`, and a regenerated `package-lock.json`.

The feature uses the new dep, so the manifest rides with it → two concerns + a lockfile → three commits, lockfile last. Show this plan, then run the commands in order:

```bash
# 1 — feature, its test, and the dependency it needs (one buildable story)
git add src/search.js src/search.test.js package.json
git commit -S -m "✨ feat(search): debounce query input" -m "Searching fired a request on every keystroke, hammering the API on fast typers. Debounce by 300ms so we only query once typing settles. Adds lodash.debounce and a test covering the timing."

# 2 — unrelated docs typo, on its own
git add README.md
git commit -S -m "✏️ fix: correct typo in README setup steps"

# 3 — lockfile last, isolated so a merge conflict can just be regenerated
git add package-lock.json
git commit -S -m "📦️ build: update package-lock.json"
```

Single-commit change, no body — one `-m` is enough:

```bash
git add report.py
git commit -S -m "♻️ refactor(report): simplify build with a line-total helper"
```

If the user wants everything as one commit, pick the emoji for the most significant change and say what you set aside.
