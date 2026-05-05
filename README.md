# Become an AI Native Builder — course repo

Project-scoped agent customizations used while taking the *Become an AI Native Builder* course. The same four skills auto-load in **Claude Code**, **Cursor**, and **Codex CLI** — no config required in any of them.

## What's in here

```
.claude/
├── agents/                     # Claude-only subagents
│   ├── grade-against-spec.md
│   └── stranger-test.md
└── skills/<name>/SKILL.md      # Claude Code reads this path natively

.cursor/
└── skills/<name>/SKILL.md      # Cursor reads this path natively

.agents/
└── skills/<name>/SKILL.md      # Codex CLI reads this path natively
```

The three `skills/` folders hold identical copies of the same `SKILL.md` files. Independent copies (rather than symlinks) so each tool stays self-contained.

## Skills

| Skill | Triggers on |
|-------|-------------|
| `discovery` | "discovery", "opportunity", "roadmap", "what should we build next" |
| `mp-prototype` | wanting a Magic Patterns prototype that matches a real React + Tailwind + Shadcn codebase |
| `verify-spec` | defining verifiable completion criteria for a fuzzy writing/strategy/design task |
| `write-guide` | writing a step-by-step instructional guide as a single `.md` file |

## Agents (Claude Code only)

| Agent | Role |
|-------|------|
| `grade-against-spec` | given one admitted check + an artifact, returns pass/fail |
| `stranger-test` | given a candidate check, decides whether a stranger could apply it |

These are the "applier" and "admitter" companions to the `verify-spec` skill. Cursor and Codex CLI don't have a subagent equivalent, so the agent-spawning steps inside `verify-spec` only work in Claude Code; the skill body still loads in the other two tools.

## Usage

Open this directory in any of the three tools. Skills load automatically and are invoked by description match or with `/<skill-name>` (e.g. `/verify-spec`).

## Keeping the three copies in sync

Edit a skill in one folder, then mirror it to the other two:

```bash
rsync -a --delete .claude/skills/ .cursor/skills/
rsync -a --delete .claude/skills/ .agents/skills/
```
