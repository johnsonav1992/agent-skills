# Agent skills

Skills I use with coding agents (Claude Code and others), grouped by what they're for.

| Folder | Skill | What it does |
| --- | --- | --- |
| `remix-v3/remix-app-builder` | `remix-app-builder` | Conventions for building Remix v3 apps: styling, components, pages, loading states and folder structure. |
| `clean-code/ub-mode` | `ub-mode` | Uncle Bob's "make it work, make it right, follow practice" principles. Third-party, see below. |

## Using a skill

Copy a skill's folder into a project's `.claude/skills/` (or `.agents/skills/`) folder, or into `~/.claude/skills/` to use it everywhere.

## Third-party skills

`clean-code/ub-mode` is a git submodule of [unclebob/ub-mode](https://github.com/unclebob/ub-mode) (MIT, © Robert C. Martin). It is not my work and isn't copied into this repo.

Clone with submodules:

```sh
git clone --recurse-submodules https://github.com/johnsonav1992/agent-skills.git
```

Pull the latest upstream version:

```sh
git submodule update --remote clean-code/ub-mode
git commit -am "Update ub-mode"
```
