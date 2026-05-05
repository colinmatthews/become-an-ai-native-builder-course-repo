## Setup & Context

- **Run `/init` on every project** — Claude scans your files and creates a `CLAUDE.md` cheat sheet so you don't have to re-explain the project each session.
- **Keep `CLAUDE.md` lean (150–200 lines max)** — It loads into every conversation, so route to other files (style guides, business context) instead of bloating it.
- **Refresh `CLAUDE.md` over time** — Log new patterns, gotchas, and conventions discovered during sessions so Claude gets smarter about your work.

## Working Smarter, Not Harder

- **Keep context small** — Don't dump everything in. Break big problems into focused steps; less noise = better output.
- **Compact at ~60% context** — Use `/compact` (you can tell it what to preserve, e.g., "keep the API decisions"). Use `/clear` when switching to a new task entirely.
- **Always start in plan mode** — Let Claude outline steps and ask questions before writing anything. Dramatically reduces rework.
- **Treat Claude like a junior teammate** — Pose problems ("how should we handle X?") instead of just commands. Better reasoning = better output.
- **Make Claude ask questions** — Tell it: "Keep asking until you're 95% confident you understand what I need."
- **Auto Review** - You can let Claude decide when to escalate permission checks and when to continue running on its own.
- Claude Code Desktop can interact with your app automatically; built this in to your validation loop


## Quality Control

- **Build self-checks into to-do lists** — Have it screenshot, verify, and confirm each step before moving on.
- **Exit early and re-prompt** — If Claude goes the wrong direction, hit escape and correct course immediately. Don't wait.
- **Use `/rewind` for quick undos** — Roll back without starting over.

## Visual & Voice

- **Use voice input** — Try `/voice` (or a dictation app) instead of typing.
- **Feed it screenshots** — Claude can see. Share error messages, designs, or inspiration sites.
- **Self-check loop with screenshots** — "Take a screenshot and tell me if the layout looks right." It will iterate visually.
- **Clone inspiration sites** — Show a site you like and ask Claude to recreate the patterns (avoids generic AI look).

## Power Features

- **Custom skills** — Create reusable workflows (e.g., a "code review" skill) that run consistently every time.
- **Preview** - Claude Code desktop can run a preview of your app inline, allowing you to use a selector to pick individual elements you want to change and giving claude access to easily take screenshots and capture logs.
- **Sub-agents for parallel work** — Tell Claude to use sub-agents on complex tasks; they each work independently and report back.
- **Notification hooks** — Get a sound when Claude finishes so you can run multiple sessions and switch tasks.
- **Remote control from your phone** — Start a task at your desk, keep steering it from your phone on a walk.
- **"Ultrathink"** — Type the word for max reasoning budget on hard problems or when normal prompts aren't working.
- **Context7 MCP** — Install it so Claude pulls current documentation instead of relying on possibly-outdated training data.