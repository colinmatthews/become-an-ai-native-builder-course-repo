---
name: weekly-critical-issues-digest
description: Use when the user wants the weekly critical-issues digest — pulls the past 7 days of Intercom conversations and PostHog event-level data, surfaces top issues with direct references, cross-references whether each issue is already in-flight, saves a dated md file in `discovery-runs/`, and tracks severity drift against prior digests in the same folder. Activates on phrases like "run the digest", "weekly critical issues", "what broke this week", "weekly issue summary", or after a user references the spec at `.claude/specs/weekly-critical-issues-digest.yaml`. Do NOT use for: one-off Intercom searches, single-tool PostHog queries, ad-hoc "what's wrong with X" questions, or general discovery work (use the `discovery` skill for that).
---

# Weekly critical-issues digest

Produces a digest md file at `discovery-runs/YYYY-MM-DD-critical-issues.md` that satisfies the spec at `.claude/specs/weekly-critical-issues-digest.yaml`. Every section is gated by binary checks — read the spec before generating.

## Hard rules

1. **Read the spec first.** Open `.claude/specs/weekly-critical-issues-digest.yaml`. Every check is mandatory. If you skip one, the digest fails.
2. **Read 100% of Intercom conversations in the window. Never sample.** If volume is high, parallelize via subagents; do not skip tickets to save context.
3. **PostHog must include event-level signals**, not just web-analytics summary. Cite specific event names, insight IDs, error-tracking IDs, or HogQL results.
4. **Every top issue gets a verbatim customer quote in double quotes** OR an explicit "no customer quote — PostHog-only signal" disclaimer.
5. **Every top issue gets a count as a specific integer** — never "roughly", "several", "a handful", "multiple".
6. **Severity vocabulary is locked to {HIGH, MEDIUM, LOW}.** No "critical", "p1", "minor", etc.
7. **Every top issue gets an in-flight-work statement** — a Linear ID, a quoted Intercom admin reply, OR explicit "no in-flight work found" with evidence (e.g., "all admin-reply timestamps null").
8. **Read every prior `*-critical-issues.md` file** in `discovery-runs/`. Do severity drift only against critical-issues digests, not against other discovery artifacts in the folder.

## Procedure

### Step 1 — Setup

In parallel:

- `Bash: ls -la discovery-runs/` — discover prior digests. Filter for `*-critical-issues.md` to get the comparison baseline. The most recent prior file is the drift target.
- Compute the time window: `now - 7 days` to `now`. Convert to Unix timestamp for the `created_at` filter (e.g., 2026-04-28 00:00 UTC = 1777334400). Compute correctly — off-by-month errors are common.
- Read `.claude/specs/weekly-critical-issues-digest.yaml` to refresh on every check.

### Step 2 — Pull data (parallel)

Issue these calls in a single tool-batch:

**Intercom — get total count first:**
```
mcp__intercom__search_conversations(
  created_at: { operator: ">=", value: <window_start_unix> },
  per_page: 1
)
```
Inspect `pages.total_pages` to get the true count. The previous query's filter may be wrong — verify the count is plausible for 7 days.

**PostHog — week-over-week event volumes (HogQL):**
```sql
SELECT event,
       countIf(timestamp >= now() - INTERVAL 7 DAY) AS this_week,
       countIf(timestamp < now() - INTERVAL 7 DAY
               AND timestamp >= now() - INTERVAL 14 DAY) AS last_week,
       this_week - last_week AS delta
FROM events
WHERE timestamp >= now() - INTERVAL 14 DAY
  AND event NOT IN ('$autocapture','$identify','$set','$web_vitals','$pageleave')
GROUP BY event
ORDER BY this_week DESC LIMIT 30
```

**PostHog — daily event breakdown (catches single-day collapses):**
```sql
SELECT toDate(timestamp) AS day, event, count() AS volume
FROM events
WHERE event IN (<your product events>)
  AND timestamp >= now() - INTERVAL 14 DAY
GROUP BY day, event
ORDER BY day DESC, volume DESC
```

**PostHog — rageclick URLs:**
```sql
SELECT properties.$current_url AS url, count() AS rageclicks
FROM events
WHERE event = '$rageclick' AND timestamp >= now() - INTERVAL 7 DAY
GROUP BY url ORDER BY rageclicks DESC LIMIT 20
```

**PostHog — error-tracking and event-definitions listings** for instrumentation gaps.

### Step 3 — Pull all conversations

Decide parallelization based on the count from Step 2:

| Volume | Strategy |
|---|---|
| 1–25 | 1 worker, read in main context (per_page=25) |
| 26–100 | 3 workers, ~30 tickets each (split by `created_at` ranges) |
| 100+ | 8 workers, ~25 tickets each |

For multi-worker runs:
- Split the time window into N equal sub-ranges.
- Spawn `Agent` (subagent_type: `general-purpose`) calls in parallel — one per sub-range.
- Each worker calls `mcp__intercom__search_conversations` with its sub-range, paginates if needed, and returns: theme labels, ticket IDs, verbatim quotes (under 15 words each, in double quotes), `statistics_first_admin_reply_at` for in-flight-work checks, root-cause attributes.
- Workers must NOT do prioritization. They report; the main thread synthesizes.

If a tool result exceeds context limits, the runtime saves it to a file. In that case, dispatch a subagent to read the file in chunks and return structured findings — do not read the raw file inline.

### Step 4 — Synthesize top issues

Cluster ticket themes. Use the `Root cause` custom attribute on tickets when present (it pre-clusters). Then add PostHog-only signals (collapses, spikes, rageclick clusters) that have no Intercom equivalent — these are often the most important findings since they're invisible to support.

For each candidate issue:

1. **Triangulate.** Does the other tool corroborate? E.g., Intercom sync complaints + PostHog `$rageclick` on a `/sync` URL → same issue, double-confirmed.
2. **Assign severity** from {HIGH, MEDIUM, LOW}:
   - HIGH: complete event collapse, >20% of tickets, paying-customer impact, or zero-event days for product-critical events.
   - MEDIUM: 10–20% of tickets or a single corroborated PostHog signal.
   - LOW: <10% of tickets, isolated, no PostHog corroboration.
3. **In-flight check.** For every cited Intercom ticket, check `statistics_first_admin_reply_at`. If null on all, write "no in-flight work found" with that evidence. If non-null on one, quote the admin reply (under 15 words) with the conversation ID.

Aim for 3–7 top issues. Fewer than 3 means signals were under-interrogated; more than 7 means the cluster step was too granular.

### Step 5 — Severity drift

Open the most recent prior `*-critical-issues.md` in `discovery-runs/`. For each issue in the prior file:

- If still appearing this week with the same severity → `UNCHANGED`
- If severity rose → `UP` (note prior level)
- If severity fell → `DOWN` (note prior level)
- If absent this week → `RESOLVED`

For every new issue this week not in the prior file → `NEW`.

If no prior `*-critical-issues.md` exists: write "N/A — first run, no prior critical-issues digest in folder." Other discovery artifacts in the folder (retention, activation, etc.) are NOT comparison baselines.

### Step 6 — Write the file

Path: `discovery-runs/YYYY-MM-DD-critical-issues.md` (end-date of window).

Use `references/template.md` as the structural skeleton. Required sections:

- H1 with explicit ISO date range: `# Weekly Critical Issues — Week of YYYY-MM-DD → YYYY-MM-DD`
- `## Methodology` — total Intercom conversations, worker count × tickets-per-worker, PostHog queries run
- `## Top issues this week` — each issue tagged with severity, includes count (integer), references (Intercom IDs and/or PostHog event names), verbatim quote (or PostHog-only disclaimer), in-flight-work statement
- `## Severity drift` — drift table or "N/A — first run"
- `## Limitations` — instrumentation gaps, sample-size caveats, missing data sources

### Step 7 — Present the summary

After writing the file, message the user with:

- Relative path to the saved md (so they can click).
- A short list (3–7 lines, one per top issue) — each line repeats the severity tag and at least one reference (Intercom ID or PostHog event name) that ALSO appears in the saved md.
- One-sentence note on severity drift outcome.
- One-sentence note on coverage (e.g., "100% of 10 Intercom tickets read; PostHog queried at event level").

Keep the summary terse. The md is canonical; the summary is a hand-off pointer.

## Verifying before handing off

Before declaring the digest done, walk every check in the spec yaml against the file you just wrote. If any check fails, fix the file and re-walk. Common misses:

- Forgetting the in-flight-work line on a PostHog-only issue (write "no in-flight work found — issue surfaced from PostHog only, no associated Intercom tickets").
- Vague quantifiers slipping in via summary phrasing ("roughly a third" → use "3 of 10 tickets (30%)").
- A quote in italics without quotation marks (the check requires `"..."`, not `*...*`).
- Severity drift section missing when prior file exists.

## Bundled references

- `references/template.md` — md skeleton with placeholders matching every spec check
- `references/posthog-queries.md` — copy-paste HogQL queries beyond the defaults above (funnel drops, retention regressions, breakdown-by-platform)
- `references/parallelization.md` — how to split a high-volume Intercom window across subagents and reconcile worker output
