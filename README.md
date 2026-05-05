# Become an AI Native Builder — course repo

Working repo for the *Become an AI Native Builder* course. Contains the lesson guides, exercises, and the agent customizations (skills + Claude subagents) referenced throughout the course.

## What's in here

```
guides/                          # course lessons + exercises (markdown + screenshots)

.claude/
├── agents/                      # Claude-only subagents
│   ├── grade-against-spec.md
│   └── stranger-test.md
└── skills/<name>/SKILL.md       # Claude Code reads this path natively

.cursor/
└── skills/<name>/SKILL.md       # Cursor reads this path natively

.agents/
└── skills/<name>/SKILL.md       # Codex CLI reads this path natively
```

The same four skills are committed to all three skill folders so they auto-load in **Claude Code**, **Cursor**, and **Codex CLI** with no config in any of them.

## Guides

15 numbered lessons + 7 exercises in [guides/](guides/). Highlights:

| | |
|---|---|
| [1 — Setting Up Your First MCP Connection](guides/1%20-%20Setting%20Up%20Your%20First%20MCP%20Connection.md) | First MCP server hookup |
| [2 — Creating Your First Skill](guides/2-%20Creating%20Your%20First%20Skill.md) | Authoring a `SKILL.md` |
| [3 — Connecting PostHog, Intercom, and Linear](guides/3-%20Connecting%20PostHog%2C%20Intercom%2C%20and%20Linear.md) | Discovery-stack MCPs |
| [4 — Creating Your First Plugin](guides/4%20-%20Creating%20Your%20First%20Plugin.md) | Plugin packaging |
| [5 — Code-based Prototypes](guides/5%20-%20Code-based%20Prototypes.md) | Prototyping flow |
| [6 — Create Your First Background Agent](guides/6%20-%20Create%20Your%20First%20Background%20Agent.md) | Background work |
| [7 — Pull Changes Locally & Iterate](guides/7%20-%20Pull%20Changes%20Locally%20%26%20Iterate.md) | Local iteration loop |
| [8 — Submitting Your Pull Request](guides/8-%20Submitting%20Your%20Pull%20Request.md) | PR workflow |
| [9 — Setting Up Scheduled Tasks](guides/9%20-%20Setting%20Up%20Scheduled%20Tasks.md) | Cron-style routines |
| [10 — Connect Magic Patterns MCP](guides/10-%20Connect%20Magic%20Patterns%20MCP.md) | MP integration |
| [11 — Figma Loop](guides/11%20-%20Figma%20Loop.md) | Design-to-code |
| [12 — Background agent & GitHub integration](guides/12%20-%20Background%20agent%20%26%20Github%20integration%20%28Codex%2C%20Claude%20Code%2C%20Cursor%29.md) | Codex / Claude Code / Cursor |
| [13 — FAQ](guides/13%20-%20FAQ.md) | Common questions |
| [14 — 30-Day AI-Native Builder Rollout Plan](guides/14%20-%20Your%2030-Day%20AI-Native%20Builder%20Rollout%20Plan.md) | Adoption playbook |
| [15 — Claude Code Tips & Tricks](guides/15%20-%20Claude%20Code%20Tips%20%26%20Tricks.md) | Power-user moves |

Exercises: [1 — Connect to Course MCP](guides/Exercise%201%20-%20Connect%20to%20Course%20MCP.md), [2 — Create a skill](guides/Exercise%202%20-%20Create%20a%20skill.md), [3 — Create a plugin](guides/Exercise%203%20-%20Create%20a%20plugin.md), [4 — Code-based prototype](guides/Exercise%204%20-%20Create%20a%20code%20based%20prototype.md), [5 — Worktrees](guides/Exercise%205%20-%20Worktrees.md), [6 — Verifiable Outputs](guides/Exercise%206%20-%20Verifiable%20Outputs.md), [7 — Scheduled task](guides/Exercise%207%20-%20Create%20a%20scheduled%20task.md).

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

Open this directory in any of the three tools — skills load automatically and are invoked by description match or with `/<skill-name>` (e.g. `/verify-spec`).

## Keeping the three skill copies in sync

Edit a skill in one folder, then mirror it to the other two:

```bash
rsync -a --delete .claude/skills/ .cursor/skills/
rsync -a --delete .claude/skills/ .agents/skills/
```
