# Digest template

Skeleton with placeholders matching every spec check. Fill placeholders, do not delete sections. If a section doesn't apply (e.g., severity drift on first run), keep the heading and write the explicit "N/A — first run" line so the grader can verify the check.

```markdown
# Weekly Critical Issues — Week of YYYY-MM-DD → YYYY-MM-DD

**Run date:** YYYY-MM-DD
**Project:** <product name> (PostHog project <id> "<project name>")
**Sources:** Intercom (<N> conversations), PostHog event-level data (HogQL queries against `events` table, error-tracking, event definitions)

---

## Methodology

- **Intercom:** <N> conversations created in window (`created_at >= <unix>`). Read **100%** via <W> worker(s) × <T> tickets each = <N>. <one line on parallelization rationale at this volume>. <one line on what fields were inspected>.
- **PostHog event-level data:**
  - `event-definitions-list` → <N> events catalogued; flagged <events with notable last_seen gaps>.
  - HogQL: week-over-week volume comparison ([query](<posthog url>)).
  - HogQL: daily volume breakdown for past 14 days.
  - HogQL: rageclick URLs over past 7 days (<N> results).
  - `error-tracking-issues-list` → <N> tracked issues <(instrumentation gap, see Limitations) if zero>.
  - `insights-list` → <N> saved insights, <description>.
- **In-flight work cross-reference:** For each issue, checked Intercom `statistics_first_admin_reply_at`, `statistics_count_assignments`, `last_admin_reply_at`. <Linear access status>.

---

## Top issues this week

### 1. <Issue title> — *severity: HIGH | MEDIUM | LOW (NEW | UP from <prior> | UNCHANGED | DOWN from <prior>)*

- **Count:** <integer> <(of <total>, <pct>%) if a fraction makes sense>.
- **Week-over-week:** <PostHog deltas with absolute and percent>.
- **PostHog references:** event name `<name>`, insight `<short_id>`, error-tracking issue `<id>`, or HogQL result.
- **Intercom references:** `<conversation_id>`, `<conversation_id>`, `<conversation_id>`.
- **Customer quote:** "<verbatim under 15 words>" (ticket `<id>`).
  OR (for PostHog-only signals): no customer quote — PostHog-only signal.
- **In-flight work:** <one of>
  - Linear `<TICKET-ID>` opened <date> — admin reply on conversation `<id>`: "<quoted snippet>".
  - "no in-flight work found" + evidence (e.g., "all cited tickets have `statistics_first_admin_reply_at: null`").

### 2. <Issue title> — *severity: ...*

[same structure]

[3–7 issues total]

---

## Severity drift

<one of>:

**N/A — first run, no prior critical-issues digest in folder.**

OR

**Comparison vs `<prior_filename>.md`:**

| Issue | Prior severity | Current severity | Status |
|---|---|---|---|
| <issue> | HIGH | HIGH | UNCHANGED |
| <issue> | MEDIUM | HIGH | UP |
| <issue> | HIGH | (absent) | RESOLVED |
| <new issue> | (none) | MEDIUM | NEW |

---

## Limitations

- <instrumentation gaps>
- <sample-size caveats>
- <missing data sources, e.g., no Linear access, no admin replies, localhost-only events>
- <correlational signals where causation is unverified>
```

## Anti-patterns to refuse

- Severity tags outside {HIGH, MEDIUM, LOW}. Even "Critical" fails.
- Italicized quotes without quotation marks (`*foo*` fails — use `"foo"`).
- "Roughly", "a handful", "several", "multiple", "a third of" anywhere in the Top issues section. Use integers.
- Skipping the in-flight-work line because "no admin replies on any ticket made it obvious." The check still requires the line.
- Comparing severity drift against an unrelated discovery file (retention, activation). Only compare against prior `*-critical-issues.md`.
