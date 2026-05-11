# Weekly Critical Issues — Week of 2026-04-28 → 2026-05-05

**Run date:** 2026-05-05 (scheduled re-run; supersedes prior 2026-05-05 digest with Linear cross-references and a missed account-deletion cluster)
**Project:** Stride (PostHog project 391447 "Default project")
**Sources:** Intercom (10 conversations), PostHog event-level data (HogQL queries against `events` table, error-tracking, event definitions), Linear (Stride team, last 14 days)

---

## Methodology

- **Intercom:** 10 conversations created in window (`created_at >= 1777334400`). Read **100%** via 1 worker × 10 tickets = 10 (no parallelization required at this volume; spec's parallelization rule designed for high-volume weeks of 100+ tickets). All 10 ticket bodies, AI titles, root-cause attributes, `statistics_first_admin_reply_at`, `statistics_count_assignments`, and `last_admin_reply_at` inspected.
- **PostHog event-level data:**
  - `event-definitions-list` → 19 events catalogued; flagged `activity_created` last_seen 2026-05-03 (2 days ago), `activity_kudoed` last_seen 2026-04-22 (13 days ago), `activity_commented` last_seen 2026-04-21 (14 days ago).
  - HogQL: week-over-week volume comparison for 11 product events ([query](https://us.posthog.com/project/391447/insights/new)).
  - HogQL: daily volume breakdown for past 14 days across all non-system events.
  - HogQL: rageclick URLs over past 7 days (5 results).
  - `error-tracking-issues-list` → 0 tracked issues (instrumentation gap, see Limitations).
- **In-flight work cross-reference:** For each issue, checked Intercom `statistics_first_admin_reply_at`, `statistics_count_assignments`, and `last_admin_reply_at`. Linear (Stride team) `list_issues` for last 14 days returned 8 issues; matched IDs against ticket clusters by title and `Root cause` attribute. (Prior digest incorrectly stated Linear was inaccessible.)

---

## Top issues this week

### 1. Engagement collapse extending from 2026-05-04 into 2026-05-05 — *severity: HIGH (UP from prior — collapse now spans 2 calendar days, not 1)*

- **Count:** 0 events of `activity_created`, `user_logged_in`, `bootstrap_loaded`, and `$pageview` on 2026-05-04 (vs. prior 7-day daily values of 96, 91, 104, 94, 85, 48 for `activity_created` from 2026-04-28 to 2026-05-03); on 2026-05-05 only 1 `user_logged_in`, 1 `onboarding_completed`, 3 `$rageclick` events through the run timestamp.
- **Week-over-week:** `$pageview` 2114 → 1421 (−693, −33%); `activity_created` 727 → 428 (−299, −41%); `user_logged_in` 708 → 464 (−34%); `bootstrap_loaded` 706 → 463 (−34%). Daily breakdown shows the drop began 2026-05-03 (52 logins, half-baseline) and hit zero on 2026-05-04.
- **PostHog references:** event name `activity_created`, event name `user_logged_in`, event name `$pageview`, event name `bootstrap_loaded`. HogQL daily-breakdown result confirmed zero events on 2026-05-04 across all four; 2026-05-05 partial-day total is 5 events of any kind.
- **Customer quote:** no customer quote — PostHog-only signal.
- **In-flight work:** no in-flight work found. No Intercom tickets reference an outage on 2026-05-04 or 2026-05-05. No Linear issue references "outage", "deploy regression", or "tracking pipeline" in the last 14 days. All 10 weekly tickets have `statistics_first_admin_reply_at: null`.

### 2. Social engagement events stopped firing entirely — *severity: HIGH (UNCHANGED)*

- **Count:** 0 events of `activity_kudoed` and 0 events of `activity_commented` in past 7 days, vs. 85 and 27 in the prior 7-day window respectively (−100%).
- **PostHog references:** event name `activity_kudoed` (last_seen 2026-04-22), event name `activity_commented` (last_seen 2026-04-21). 13–14 day gap with zero firing.
- **Customer quote:** no customer quote — PostHog-only signal.
- **In-flight work:** no in-flight work found. No Linear issue in the last 14 days mentions kudo, comment, or social-graph instrumentation. The prior digest noted the related `user_signed_up` vs `user_registered` duplicate-event issue from `2026-05-05-stride-retention.md`, but kudo/comment events specifically are unowned.

### 3. Activity sync failure — wearable to Stride feed — *severity: HIGH (UNCHANGED — but in-flight Linear work now identified)*

- **Count:** 3 of 10 Intercom tickets (30%) — Apple Watch Ultra 2, Garmin Forerunner 265, and a Mont-Royal segment-attribution failure.
- **Intercom references:** `215474159770073`, `215474159770111`, `215474159770137`.
- **PostHog corroboration:** event name `$rageclick` fired on URL `http://localhost:5173/record?sync=degraded` (1 rageclick) — a UI state explicitly named for sync degradation.
- **Customer quote:** "synced to the watch activity, but it's not showing up anywhere in my Stride feed" (ticket `215474159770073`).
- **In-flight work:** Linear `STR-96` "Flaky segment matching test in clubs integration suite" (priority High, status Backlog, created 2026-05-03) — directly addresses the segment-attribution failure pattern (`Root cause: segment-mismatch` on ticket `215474159770137`). No Linear issue covers the wearable-to-feed sync path itself. All 3 tickets have `statistics_first_admin_reply_at: null`.

### 4. Account deletion / churn requests — *severity: MEDIUM (NEW — missed in prior digest)*

- **Count:** 2 of 10 Intercom tickets (20%) — both explicitly request permanent account + data deletion.
- **Intercom references:** `215474159770136` (Tony Shields, "decided the app isn't for me right now"), `215474159770164` (iOS, `Root cause: account-deletion`).
- **PostHog corroboration:** `subscription_started` 0 → 1 in past 7 days vs. prior week, plus `subscription_cancelled` event last_seen 2026-04-21 (instrumentation may not be capturing cancellations beyond that date — see Limitations).
- **Customer quote:** "I'd like to permanently delete my Stride account along with all my personal data and activity history" (ticket `215474159770136`).
- **In-flight work:** Linear `STR-94` "Account deletion request requires data purge workflow implementation" (priority High, status In Progress, started 2026-05-03) — explicitly covers the workflow gap surfacing in these tickets. Both tickets still have `statistics_first_admin_reply_at: null` (workflow exists in dev, not yet user-facing).

### 5. Password reset email not delivered — *severity: MEDIUM (UNCHANGED)*

- **Count:** 2 of 10 Intercom tickets (20%).
- **Intercom references:** `215474159770071`, `215474159770079`.
- **PostHog corroboration:** event name `$rageclick` fired on `/auth` URLs (3 rageclicks total: 2 on `localhost:5177/auth`, 1 on `localhost:5173/auth`), consistent with users repeatedly attempting auth and failing.
- **Customer quote:** "I've been trying to reset my password for the past hour but I haven't received any email from Stride" (ticket `215474159770071`).
- **In-flight work:** no in-flight work found. Both tickets have `statistics_first_admin_reply_at: null`. No Linear issue in the last 14 days mentions auth, password reset, or transactional email delivery.

### 6. App crash on iPhone 15 Pro feed view — *severity: MEDIUM (UNCHANGED — in-flight Linear work now identified)*

- **Count:** 1 of 10 Intercom tickets (10%).
- **Intercom reference:** `215474159770110`.
- **PostHog corroboration:** event name `$rageclick` fired on `/onboarding` (2 rageclicks) — not a direct match for the feed crash. PostHog `error-tracking-issues-list` returned 0 issues — client-side crash instrumentation is not wired up (instrumentation gap).
- **Customer quote:** "Stride keeps crashing the moment I try to view my feed or check any of my recent runs" (ticket `215474159770110`).
- **In-flight work:** Linear `STR-90` "Feed screen crashes on launch - mobile app stability" (priority Medium, status Todo, created 2026-04-26) — directly matches the reported symptom. Ticket `215474159770110` still has `statistics_first_admin_reply_at: null`.

### 7. Billing refund request — *severity: LOW (NEW — missed in prior digest)*

- **Count:** 1 of 10 Intercom tickets (10%).
- **Intercom reference:** `215474159770158`.
- **PostHog corroboration:** none direct. `subscription_started` registered 1 event in past 7 days, but no `subscription_cancelled` events firing since 2026-04-21 (potential instrumentation gap — see Limitations).
- **Customer quote:** "Could you please process a refund for the most recent charge?" (ticket `215474159770158`).
- **In-flight work:** Linear `STR-93` "Process refund for erroneous charge in billing system" (priority High, status In Progress, started 2026-05-03) — directly addresses the refund-handling workflow surfaced by this ticket. Conversation has `statistics_first_admin_reply_at: null`.

---

## Severity drift

**Comparison vs `2026-05-05-critical-issues.md` (prior version of today's digest):**

| Issue | Prior severity | Current severity | Status |
|---|---|---|---|
| Engagement collapse on 2026-05-04 | HIGH | HIGH | **UP** — collapse now extends into 2026-05-05 (5 events total today vs. zero baseline disruption from prior week). Window changed from 1 day to 2 calendar days. |
| Social engagement events stopped firing | HIGH | HIGH | UNCHANGED — `activity_kudoed`/`activity_commented` still 0 in past 7 days. |
| Activity sync failure — wearable to feed | HIGH | HIGH | UNCHANGED — same 3 tickets (30%); Linear `STR-96` now identified as adjacent in-flight work. |
| Password reset email not delivered | MEDIUM | MEDIUM | UNCHANGED — same 2 tickets (20%); still no in-flight work. |
| App crash on iPhone 15 Pro feed view | MEDIUM | MEDIUM | UNCHANGED — same 1 ticket; Linear `STR-90` now identified as in-flight. |
| Account deletion / churn requests | (none) | MEDIUM | **NEW** — 2 of 10 tickets (20%) requesting permanent deletion. Missed in prior digest because each ticket was attributed to its own root-cause attribute rather than clustered. Linear `STR-94` is in-flight. |
| Billing refund request | (none) | LOW | **NEW** — 1 of 10 tickets (10%). Missed in prior digest. Linear `STR-93` is in-flight. |

No prior issues moved to RESOLVED.

---

## Limitations

- **PostHog error tracking is empty:** 0 tracked issues. The iPhone 15 Pro crash should have surfaced as a tracked error but did not. Client-side crash reporting needs wiring up before sentinel signals are reliable.
- **`localhost` URLs in rageclick data:** All 5 `$rageclick` URLs are `localhost:5173` / `localhost:5177`, indicating events are being captured from development environments — production URL coverage may be incomplete or misconfigured.
- **No admin replies on any of the 10 tickets:** `statistics_first_admin_reply_at` is null on every conversation, so Intercom-side "in-flight work" cross-reference is uniformly negative. The Linear cross-reference partly compensates, but support staff triage happening in Slack or other channels is invisible to this digest.
- **Companies = null on all tickets:** No B2B / enterprise account flagging possible. All 10 reporters are individual consumer users.
- **Engagement collapse is correlational:** The 2026-05-04 zero-event day plus 2026-05-05 near-zero day could be (a) genuine outage, (b) deploy regression breaking event firing, (c) tracking pipeline failure, or (d) `now()` clock drift in HogQL. PostHog data alone cannot distinguish these.
- **Subscription event coverage:** `subscription_started` (1 event this week) and `subscription_cancelled` (last_seen 2026-04-21) volumes are too low to corroborate churn signals from account-deletion or refund tickets — instrumentation may be partial.
- **Re-run filename collision:** This digest overwrites the earlier 2026-05-05 file; severity drift was computed against the prior content before write.
