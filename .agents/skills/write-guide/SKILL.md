---
name: write-guide
description: Use when the user wants to write an instructional guide — a markdown article that explains how to do something step-by-step. Takes varied input (idea, notes, transcripts, partial drafts, research links, screenshots) and produces one complete .md file that satisfies the course-guide spec at `.claude/specs/course-guide.yaml` if present. Do NOT use for newsletters, blog posts, opinion pieces, essays, or one-off READMEs — this skill is specifically for instructional how-to guides where the reader is meant to follow steps.
---

# write-guide

Take varied input and produce one complete instructional guide as a markdown file. The guide must satisfy the course-guide spec (`.claude/specs/course-guide.yaml`) before it ships, if that spec exists in the project.

## Procedure

### Step 1 — Frame the guide

One question per turn:

1. **Topic.** *"What does this guide explain how to do?"* Capture for the file slug (lowercase, hyphens).
2. **Reader.** *"Who's reading this? Beginner with no context? Intermediate? A specific persona?"* Save as the calibration for tone and depth.
3. **Output path.** *"Where should it land?"* Default to `./guides/<slug>.md` if a `guides/` directory exists, otherwise `./drafts/<slug>.md` if `drafts/` exists. If neither exists, ask — don't invent a directory.

### Step 2 — Inventory what the user has

Ask:

> "What do you have to work with? Paste any notes, transcripts, links, partial drafts, screenshots, or other source material. If you only have a topic, say so and we'll research before drafting."

If they have source material: parse it for facts, code samples, structure cues, terminology to preserve.

If they have only a topic: research before drafting. Pick the right tool for the gap:

- **Library / framework / SDK / API / CLI questions** → Context7 MCP (`resolve-library-id` → `query-docs`).
- **Current events, news, recent product docs, thought leadership** → WebSearch + WebFetch.
- **Big or open-ended research (multi-source synthesis)** → spawn a general-purpose subagent so the main thread doesn't bloat with fetched pages.

Surface what you found. Confirm scope with the user before drafting.

### Step 3 — Outline

Propose a top-level section structure organized by the reader's **task surface** — concrete things they're doing or working with (tool names, product names, stages of a workflow). Not abstract categories like "Setup", "Tips", "Troubleshooting".

Show the outline. Ask:

> "Does this match how a reader would walk through it top-to-bottom? Anything to reorder, add, or drop?"

Do not draft until the outline is confirmed.

### Step 4 — Draft section by section

Show each section to the user **as it's drafted**, not the full guide at the end. This lets them redirect mid-draft instead of accepting a finished piece they have to rewrite.

Each section follows the spec's structural rules:

- **Action-oriented body.** Sentences describe what the reader does ("Run X.", "Click Y."), not questions to answer or conditions to evaluate.
- **Visual rhythm.** Every action is followed by a visual artifact within ≤2 sentences — a screenshot placeholder, code block, or example output.
- **Verbatim commands/UI labels.** Show exact commands and UI element names; don't describe them abstractly.
- **No walls of text.** Body content does not exceed ~100 words between visual breaks. Bulleted lists count as body text, not as a break.
- **Linear progression within each section.** No "Choose your path" sub-headings. Inline conditionals in body text are fine.

Format conventions:

- **Image placeholders:** `![[<slug>-<step-name>.png]]` (Obsidian-style — easy for the user to replace with real screenshots later).
- **Code samples:** fenced blocks with the language tag.
- **Inline UI labels:** quoted exactly (`'Save Plugin'`) or in code formatting (`/init`).

If the user redirects mid-section, finish the current paragraph, then incorporate the redirect. Do not silently restructure prior sections without naming what changed.

### Step 5 — Run the spec via subagents

If `.claude/specs/course-guide.yaml` exists, apply every check against the draft using the `grade-against-spec` subagent — **one call per check, batched in parallel**.

**Why subagents.** The main Claude wrote the draft, so it's biased toward grading its own work favorably. Each `grade-against-spec` invocation has no conversation context — it sees only the check + the draft, and applies the check fresh. This is the same pattern `verify-spec` uses with `stranger-test` for admitting candidate checks; here we use it for *applying* admitted checks.

For each check in the spec:

1. Prepare a prompt with: the check statement, the draft (or the relevant excerpt — sections most affected by that check), and any additional context the check requires (e.g., the original outline for `task_surface_organization`).
2. Spawn `Agent(subagent_type: "grade-against-spec", prompt: <prompt>)`.
3. Batch all check invocations into a single message with multiple Agent tool calls so they run in parallel.

Collect verdicts. Show the user a table:

| Check | ✅/❌ | Subagent notes |
|---|---|---|
| `action_oriented_sections` | ... | ... |
| `visual_rhythm` | ... | ... |
| `task_surface_organization` | ... | ... |
| `no_walls_of_text` | ... | ... |
| `linear_progression` | ... | ... |

If any check fails, fix the failing sections and re-run **just the failed checks** (not all of them). Do not finalize a draft that fails any check.

If the user disagrees with a subagent's verdict (e.g., a check fired but they think it shouldn't have), surface the disagreement explicitly — that's a signal the check itself may need refinement, which is a `verify-spec` task, not a `write-guide` task. Note it for follow-up; don't silently override.

If `.claude/specs/course-guide.yaml` doesn't exist in the project, run a basic structural sanity pass yourself against the rules in Step 4. Tell the user: *"No spec found at `.claude/specs/course-guide.yaml` — running an internal sanity check only. Build a spec with `/verify-spec` if you want enforced consistency across guides."*

### Step 6 — Write the file

Write the guide to the agreed path. Tell the user:

- The path.
- A two-line summary of what's in it.
- Any open TODOs — image placeholders to fill, fact-checks pending, sections marked WIP.

## Hard rules

- Output is **exactly one markdown file**. No companion README, no index, no asset folder.
- Image references use `![[<slug>-<step>.png]]` — Obsidian-style, consistent across guides.
- Never finalize without running the spec (if `.claude/specs/course-guide.yaml` exists).
- If the user changes the topic mid-draft, restart from Step 3 — do not stitch two outlines together.
- One question per turn during the interview steps. Never bundle.

## When to use vs not

Use when:
- The user wants an instructional how-to guide.
- The reader is meant to follow it step by step.
- Recurring task — multiple guides over time, consistency matters.

Don't use when:
- It's a newsletter (different shape, different cadence).
- It's an opinion piece, essay, or thought-leadership blog post.
- It's a one-off README or quick internal doc — overkill.
- The user wants something other than a single .md file as output.
