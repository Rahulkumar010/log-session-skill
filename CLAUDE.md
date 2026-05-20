# CLAUDE.md — log-session skill repo

## What this repo is

A single-skill Claude Code plugin. The deliverable is `skills/log-session/SKILL.md` — a pure-prompt skill with no executable code.

---

## Rules when working in this repo

- **No code files.** This is a prompt-only skill. Do not create `.py`, `.ts`, `.js`, or any executable files unless explicitly asked.
- **`skills/log-session/SKILL.md` is the source of truth.** All edits to skill behaviour go there. Do not duplicate skill logic into README or EXAMPLES.
- **`README.md` describes, `SKILL.md` defines.** Keep them in sync but don't copy-paste between them.
- **`EXAMPLES.md` illustrates, never prescribes.** Examples should reflect what the skill actually produces — update them if SKILL.md changes.
- **`CLAUDE.md` is for contributors.** Do not surface its contents to end users.

---

## File responsibilities

| File | Purpose |
|---|---|
| `skills/log-session/SKILL.md` | Skill definition — trigger, steps, output format |
| `README.md` | Installation, usage, platform compatibility |
| `EXAMPLES.md` | Worked examples of skill input → output |
| `.claude-plugin/plugin.json` | Plugin manifest for Claude Code registry |
| `.claude-plugin/marketplace.json` | Marketplace listing metadata |
| `CLAUDE.md` | Contributor guidelines (this file) |
| `LICENSE` | MIT |

---

## Making changes

- Trigger logic changes → update `SKILL.md` `## When to trigger` and `README.md` trigger table
- Output format changes → update `SKILL.md` `## Output format` and `EXAMPLES.md`
- New platform support → update `README.md` `## Compatibility` and `plugin.json` keywords
- Version bumps → update `plugin.json` version field
