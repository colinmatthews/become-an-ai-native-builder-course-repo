
You'll use the `/verify-spec` skill to interview yourself, react to example outputs, and produce a **spec** — a list of pass/fail checks any future draft must satisfy to ship.

By the end you'll have:
1. A YAML spec you can hand to teammates or AI tools.
2. A working understanding of how to apply it.
3. Concrete next steps for your own writing tasks.


## Set up

Open a fresh Claude session on the course repo.
Confirm `/verify-spec` is available. Type `/` in the input box and look for it in the skill list.

![[CleanShot 2026-05-04 at 21.41.37.png]]


Run the skill. Claude will respond with the framing question for the interview.
You should prompt with:
```
Writing user stories for product features
```

 When Claude asks for two you'd ship and two you'd reject, paste these (or substitute your own — keep them short):

**Two you'd ship as-is:**

```
1. As a marketing manager preparing for our weekly newsletter, I want to filter
   my campaign list by status (Draft, Scheduled, Sent) so I can quickly find
   the campaign I need to update without scrolling through 200+ entries.

2. As a sales rep about to call a prospect, I want to see the prospect's last
   5 product activities (page visits, demo requests) on the contact card so
   I can tailor my opening line based on what they've already explored.
```

**Two you'd reject:**

```
1. User can log in with email or SSO. Should be fast and secure.

2. As a user, I want a better dashboard so it's more useful.
```

 Critique each example in one sentence. When Claude asks *"Pass or fail? And in one sentence, because…?"*, give a tight reason. Example responses:

- Good 1: *because — specific role, specific context, specific feature, quantitative grounding (200+ entries).*
- Good 2: *because — names exactly when they'll use it (about to call) and what it unlocks (tailor opening).*
- Bad 1: *because — no role, no story shape, vague non-functional requirements.*
- Bad 2: *because — "user" is too generic, "better" and "more useful" aren't testable.*




Confirm or push back on the candidate checks Claude proposes. For each candidate, Claude shows the wording and runs it through a "stranger test" — could a no-context reader apply this rule? Examples of what you'll see:

> *"The story names a specific role (e.g., 'marketing manager'), not a generic 'user'."*

> *"The story includes a triggering context or situation (e.g., 'preparing for our weekly newsletter')."*

If a candidate is fuzzy, push back: *"'concrete outcome' is fuzzy — what would a stranger look for to apply that?"* Claude will refine the wording until it's stranger-applicable.

Stop when you have 4–6 admitted checks. The skill's documented floor is 6, but 4–5 strong checks is plenty for a 15-minute exercise. Tell Claude *"that's enough — write the spec."*

![[verified-skills-final-spec-table.png]]

## Use your spec

Open the YAML Claude wrote. The file lands at `.claude/specs/<your-slug>.yaml`.

![[verified-skills-spec-yaml.png]]

Each check has an `id`, `type`, `statement`, and `rationale`. The rationale records *why* the check exists, so future-you doesn't relitigate.

Test the spec on a fresh user story. Paste a new story and ask Claude:

```
Apply the spec at .claude/specs/user-stories.yaml to this user story:

As a finance ops lead closing the books, I want to flag transactions over
$50K that are missing a category, so I can resolve them before sending the
report to the CFO Friday morning.
```

![[verified-skills-grade-output.png]]

You'll get pass/fail per check, with specific violations called out for any failures.

If a check fires when it shouldn't (or fails to fire when it should), refine the spec — edit the YAML directly. The spec is yours; iterate on it the way you iterate on a doc.


## Your spec, going forward

**Run `/verify-spec` on another writing task you do weekly.** Status updates, release notes, retros, post-mortems, anything you write recurringly and currently grade by feel.

**Apply your spec to next week's draft.** When you produce a real draft, run it through the spec before sharing. Fix what fails before anyone else sees it.

**When the spec mismatches your taste, refine the spec — not the draft.** That's the design intent: extract your judgment into something portable.

**Share the spec with your team.** Once it captures your bar, anyone — teammate or AI tool — can produce work to that bar without you in the room.
