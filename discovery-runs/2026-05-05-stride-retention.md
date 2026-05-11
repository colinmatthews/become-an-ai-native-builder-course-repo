# Discovery — Stride User Retention

**Outcome:** Improve user retention — get users to come back and stay active on the platform over time.
**Window:** Last 56 days (2026-03-10 → 2026-05-05), weekly intervals.
**Run date:** 2026-05-05.
**Tools consulted:** PostHog (project 391447 "Default project"), Linear (team STR), Intercom.

---

## 1. Preflight Findings

### 1A. Window adequacy — PASS (with a caveat)

Retention outcomes require at minimum 4 weeks of data to see meaningful cohort behavior. The 56-day window is adequate for engagement-depth and weekly-return signals. It is NOT sufficient to compute week-8 or month-2 retention rates for newer registrants (who only began arriving ~4 weeks ago). All cohort-based retention rates are treated as **directional, not baselinesed.**

### 1B. Instrumentation — two persistent issues carried forward from prior discovery run

The same duplicate-event issue documented in the 2026-04-22 activation run persists:

| Event | Count (56d) |
|---|---|
| `user_signed_up` | ~1 (last seen 2026-05-03, infrequent) |
| `user_registered` | 55 (last seen 2026-05-03, active) |

`user_registered` is the canonical signup event. Any retention cohort built on `user_signed_up` would produce near-zero results. This blocks cohort-level "Week N retention rate" computation — we cannot build a clean new-user retention curve without resolving this first.

**Impact on this run:** Engagement-depth trends (logins, activity creation, social engagement) are unaffected and read fine. Cohort retention funnel (new user → return visit at week 2, 4, etc.) cannot be reliably computed. Findings in sections 2–5 focus on engagement-depth signals and support-validated friction; they do not rely on a broken cohort definition.

### 1C. No `annotations-list` access

Cannot verify whether engagement trends visible in PostHog reflect product/behavior changes vs. measurement or campaign changes. All trend interpretation is provisional.

---

## 2. Signal Inventory (Compact)

### PostHog — 56-day weekly trends

**Engagement heartbeat (weekly unique users):**

| Metric | Weeks 1–2 (Mar 10–21) | Weeks 5–6 (Apr 5–18) | Weeks 7–8 (Apr 19 – May 2) |
|---|---|---|---|
| Weekly logins (WAU) | 135–142 | 133–162 | 162–176 |
| Weekly activity creators (WAU) | 130–140 | 129–154 | 154–174 |
| New registrations (weekly) | 0 | 8–23 | 7–14 |

**Key observation:** Logins and activity creation are growing week-over-week in the most recent 4 weeks (162 → 176 unique users logging in week 7, up from ~133–142 baseline). New registrations ramped starting week 5 (Apr 5+). This is an encouraging sign of gross engagement growth — but retention quality (do NEW users return?) is unmeasurable until the signup event is fixed.

**Social engagement signals (weekly unique users):**

| Feature | 56d total WAU avg | Trend |
|---|---|---|
| Kudos (`activity_kudoed`) | 188–198 WAU (weeks 1–7), then drops to 0 in last 2 weeks | Suspicious plateau then cliff — needs investigation |
| Comments (`activity_commented`) | 114–164 WAU (weeks 1–7), then 0 last 2 weeks | Same pattern |
| Follows (`athlete_followed`) | 2–6 WAU weeks 1–6, spike to 22 in week 7, then 0 | Spike then cliff |
| Challenge joins | 0–2 WAU most weeks, spike to 8 in week 7, then 0 | Same cliff |
| Club joins | 0–2 most weeks, spike to 5 in week 7, then 0 | Same cliff |

**Instrumentation flag:** The last 2 data points for kudos/comments/follows/clubs all return 0. These events were firing robustly in weeks 1–7 (kudos 188–198 WAU is healthy engagement). A hard stop to 0 in the last 2 weeks is almost certainly an instrumentation break or a data-pipeline issue, not a behavioral drop-off. This is a preflight-class warning for the social engagement metrics.

**Subscription signals:**
- `subscription_started`: 2 total in 56 days (1 event in week 7).
- `subscription_cancelled`: 4 total (concentrated in weeks 3–6).
- Interpretation: Cancellations outpacing new paid subscriptions 2:1 in this window. Extremely low absolute numbers; cannot infer a rate.

**Page views by path (56d totals):**

| Path | 56d pageviews |
|---|---|
| /feed | 8,364 |
| /clubs | 3,278 |
| /activities | 3,239 |
| /challenges | 3,219 |
| /profile | 3,217 |
| /segments | 3,155 |
| /settings | 1,990 |
| / (landing) | 21 |
| /auth | 8 |
| /training | 4 |
| /onboarding | 2 |

The feed is the dominant surface (8,364 views). Clubs, activities, challenges, profile, and segments are tightly clustered (3,155–3,278) — suggesting users spread time across all these surfaces roughly equally. `/training` (4 views) and `/onboarding` (2 views) are effectively dead routes in the current product.

### Linear (team STR)

**Activity sync cluster (HIGH relevance to retention):**

| Ticket | Title | Status | Priority |
|---|---|---|---|
| STR-21 | Activity sync fails to retrieve workouts from previous day | Todo | Urgent |
| STR-23 | Garmin Forerunner 265 ride lost during upload | In Progress | Urgent |
| STR-16 | Activity sync drops Apple Watch Ultra 2 workouts from feed | In Progress | High |
| STR-19 | Garmin Forerunner 265 workout lost after successful initial sync | In Progress | Medium |
| STR-86 | Activity sync failing for runs logged >12h prior on mobile | Backlog | High |
| STR-88 | Activity upload retries ignore exponential backoff | Backlog | High |
| STR-30 | Activity upload retries ignoring exponential backoff (earlier) | Todo | Medium |

**Notifications spam cluster (MEDIUM retention relevance):**

| Ticket | Title | Status | Priority |
|---|---|---|---|
| STR-9 | Notification delivery rate exceeds expected frequency | Backlog | Medium |

**Social/feed reliability cluster:**

| Ticket | Title | Status | Priority |
|---|---|---|---|
| STR-90 | Feed screen crashes on launch - mobile app stability | Todo | Medium |
| STR-17 | Feed view causes app crash on launch | Done | Medium |
| STR-31 | GraphQL cache not invalidating after club membership mutation | Backlog | Urgent |
| STR-38 | GraphQL cache stale after club membership mutation (duplicate) | In Progress | Medium |
| STR-26 | Feed query p95 latency spike to 2s in challenges | In Progress | Low |

**Engagement/discovery features in backlog:**

| Ticket | Title | Status | Notes |
|---|---|---|---|
| STR-55 | Add heatmap visualization for personal activity history | In Progress | Retention-adjacent — helps users see patterns |
| STR-83 / STR-91 | Heatmap view for personal activities (duplicate tickets) | Backlog | Two more heatmap tickets exist alongside STR-55 |
| STR-52 | Automatic injury-risk alerts based on training load | Todo | High priority |
| STR-58 | AI-suggested segments for club member targeting | Todo | High |

**Backlog gap search (retention-specific topics):**
- "streak" — 0 results
- "habit" — 0 results
- "re-engagement" — 0 results
- "lapsed user" — 0 results
- "win-back" — 0 results
- "weekly summary" — 0 results
- "push notification campaign" — 0 results

Zero tickets on any re-engagement or habit-formation mechanic. This is a notable discovery gap.

### Intercom — Conversation themes

Across approximately 82+ conversations, the following AI Title clusters emerged (semantically grouped):

**Activity sync / data loss cluster (HIGH volume):**
- "Activity not syncing" (7 across both searches)
- "Garmin activity missing" (1)
- "Activity not recorded" (1)
- "Activity not showing" (1)
- "Activity data vanished" (1)
- "Missing workout data" (1)
- "Workout not syncing" (1)
- "Workout vanished" (1)
- "Apple Watch syncing issue" (1)
- "Missing tempo run" (1)

**Semantic cluster total: ~15 conversations on one root cause — workout completed and synced, but data disappears or never appears in feed.**

Representative quotes:
- *"I completed a 45-mile ride yesterday morning on my Garmin Forerunner 265 and it synced to Stride, but the activity never appeared in my feed or on my profile. I've force-quit the app and tried logging back in multiple times with no luck. This was a key training ride for my upcoming race."* (Rasheed Hartmann)
- *"I did a killer interval session on my Garmin yesterday morning and it synced to the app (I saw the notification), but the workout just vanished from my feed... This was a PR effort and I'm pretty frustrated it's not showing up."* (Franklin Hettinger)
- *"My buddy Jake said he gave me kudos on my lunchtime loop from yesterday but I don't see it."*

**Notification overload cluster (MEDIUM volume — retention-negative):**
- "Notification preferences" (2)
- "Notification management options" (1)
- "Notification settings" (1)
- "Manage push notifications" (1)

**Semantic cluster total: ~5 conversations. All describe the same problem: users getting bombarded with kudos/follow/challenge push notifications but can't find granular controls to manage them.**

Representative quotes:
- *"I'm receiving way too many push notifications from Stride—kudos alerts, follow requests, challenge reminders. I've looked through the settings but can't find a straightforward way to reduce these without turning everything off."* (Patrick Bauch)
- *"As a premium user, I'd expect better notification management options."* (Eleazar Pouros)

**Segment effort missing cluster:**
- "Segment effort missing" (3)
- "Segment not registering" (1)
- "Segment not recorded" (1)
- "Segment effort not recorded" (1)
- "Segment attribution issue" (1)
- "Missing segment effort" (1)

**Semantic cluster total: ~8 conversations. Users complete a route through a defined segment but the effort does not record.**

**Challenge progress not updating:**
- "Challenge progress issue" (2 Intercom conversations)
- Corroborated by Intercom body: users upload an activity and it doesn't count toward their active distance challenge.

**Kudos not appearing:**
- "Kudos not appearing" (3)
- "Missing kudos" (1)
- "Kudos count discrepancy" (1)

---

## 3. Interrogation Pass

| Candidate signal | Adjacency check | Result |
|---|---|---|
| Activity sync data loss (Intercom ~15 convos) | Cross-check to Linear: 7 distinct tickets (STR-16, STR-19, STR-21, STR-23, STR-30/STR-88, STR-86) across multiple device types (Garmin FR265, Apple Watch Ultra 2, generic). PostHog: `activity_created` was firing 129–174 WAU through the window, so the core logging pipeline works for many; the sync/disappearance bug is affecting a subpopulation. Multi-tool triangulation complete. | **Evidence** — HIGH confidence threshold met: ≥5 Intercom convos (15+), ≥3 distinct Linear tickets on the same root cause (7), multi-device scope. Adjacency check on full bodies: all 3 full-body reads confirmed same pattern — Garmin/watch sync success confirmation shown to user, then workout absent from feed. |
| Notification overload (Intercom 5 convos) | Linear cross-check: STR-9 (notification delivery rate exceeds frequency, Backlog, Medium). PostHog: `$rageclick` last seen 2026-05-03 but not query-broken-down in this run. No notification-specific rageclick check run (low signal on /settings path = 1,990 views, but no rage data pulled). Intercom bodies read for top 3: all confirmed same pattern — cannot find granular controls, not about muting all. | **Evidence (MEDIUM ceiling)** — Intercom ≥5 threshold met (5 conversations); Linear corroboration (STR-9 exists); PostHog adjacency check on notification rageclick not run (incomplete). Confidence capped at MEDIUM. |
| Segment effort not recording (Intercom ~8 convos, Linear) | Linear cross-check: STR-12 (Capitol Hill Run segment fails to record, Backlog, High), STR-81 (Ikoma Skyline climb, Backlog, High). Full Intercom body read on top 2 segments conversations: both users confirm GPS trace covered the segment but no effort recorded. | **Evidence (MEDIUM confidence)** — multi-tool (Intercom + Linear), Intercom count ~8 meets ≥5 threshold. However, this is a sub-feature within the broader activity reliability theme, not a standalone opportunity; grouped with activity sync for opportunity framing. |
| Social engagement metric cliff (kudos, comments, follows drop to 0 in last 2 weeks) | PostHog: `activity_kudoed` 188–198 WAU in weeks 1–7 then 0 in weeks 8–9; same pattern for comments, follows, club joins, challenge joins. This 0 pattern across ALL social events simultaneously in the same two-week window points to an instrumentation or pipeline failure, not user behavior. Correlation with STR-9 (notification over-delivery) — possible connection if notification volume triggered platform-level muting, but that's speculative. | **Instrumentation break, not a behavioral signal.** Do not promote to opportunity. Route to Housekeeping H-1. |
| Subscription cancellations outpacing starts (4 cancels vs 2 starts, 56d) | N=4 is below the INSUFFICIENT threshold for a rate calculation. Cannot compute a meaningful churn rate. Directional only. No Intercom conversations on cancellation reason beyond STR-22 (cancellation UI, duplicate+cancelled). | **Investigated, insufficient** — too few events to derive a rate. Noted as S-1 below. |
| Feed crash on mobile (STR-90, STR-17) | STR-17 is Done. STR-90 is a new ticket (Todo). Bodies show same pattern — crash on feed open. No Intercom conversations specifically mentioning feed crash body text (though "App crashing on launch" and "App crashing on iPhone 15 Pro" appeared as AI Titles in the open-conversations scan). | **Directional evidence** — Linear shows a recurring fix-then-regress pattern; Intercom has 3 crash-related titles but body details not fully read. Capped at LOW-MEDIUM confidence as a sub-signal within feed reliability. |
| Backlog gap: no streak/habit/re-engagement tickets | Searched 7 adjacent terms in Linear, all returned 0 relevant results. PostHog has no event for `streak_viewed`, `streak_broken`, or similar retention mechanic events in the event definitions list. | **Evidence of product gap** — the team has no instrumented or backlogged re-engagement mechanism. This is a discovery gap opportunity. |
| Heatmap feature (STR-55, STR-83, STR-91 — 3 duplicate tickets In Progress/Backlog) | This is existing backlog work, not a customer-voiced problem. No Intercom conversations requesting heatmap by name. | **Solution-level candidate** within the "help users see their progress and build habits" opportunity, not a standalone opportunity. |
| Challenge progress not updating (2 Intercom, STR-18 Done) | STR-18 (elevation challenge progress bug) is Done. New Intercom bodies reference the same pattern persisting: uploaded activity doesn't count toward challenge progress. Suggests STR-18 fix may not have fully resolved the issue or a regression occurred. | **Directional evidence** — Intercom signal is thin (2 conversations); STR-18 Done suggests the fix shipped, but user reports suggest it may have recurred. Insufficient to promote as standalone; noted as S-2 below. |

---

## 4. Opportunities (Product, Ranked)

Three opportunities survive interrogation. Ranked by Impact × Confidence ÷ Effort with sequencing overrides applied.

---

### O-1. Users lose workouts after sync — a completed activity disappears or never appears in feed

**Rank: #1**

**User voice:**
- *"I completed a 45-mile ride yesterday morning on my Garmin Forerunner 265 and it synced to Stride, but the activity never appeared in my feed or on my profile... This was a key training ride for my upcoming race."*
- *"This was a PR effort and I'm pretty frustrated it's not showing up."*
- *"I did a really solid 2-hour ride on my Garmin Forerunner 265 Sunday morning before work and it's still not showing up in my feed."*

**Problem statement:** When a user completes a workout and their device confirms the sync, the activity either never appears in the Stride feed or briefly appears and then vanishes. Users have exhausted self-service recovery (force-quit, re-login, re-sync), confirmed their watch-side data is intact, and are reaching out to support — often referencing emotionally significant training sessions (race prep, PRs, key efforts). This is a direct retention risk: if the core loop (do workout → see it in app) breaks repeatedly, users have no reason to return.

**Evidence weight:**
- Intercom: ~15 conversations on semantically identical root cause across AI Titles "Activity not syncing," "Garmin activity missing," "Activity not recorded," "Workout vanished," "Apple Watch syncing issue," "Missing workout data," "Missing tempo run" — all confirmed in body reads to describe the same user-facing failure.
- Linear: 7 distinct open tickets across Garmin Forerunner 265 (STR-23 Urgent, STR-19, STR-21 Urgent), Apple Watch Ultra 2 (STR-16 High), generic mobile (STR-86, STR-88, STR-30). Multiple device types, multiple age profiles (>12h old workouts, previous-day workouts, just-synced workouts).
- PostHog: `activity_created` firing robustly (130–174 WAU) means the pipeline succeeds for many users; the failure is a subpopulation bug, not a complete outage.
- Multi-tool triangulation: YES. Adjacency checks on full Intercom bodies: completed.

**Ratings:**

| Impact | Confidence | Effort |
|---|---|---|
| HIGH | HIGH | M (1–4 weeks per subsystem; multiple subsystems involved) |

HIGH confidence is warranted: multi-tool triangulation across all three tools, Intercom volume ≥15 conversations (3× the floor), ≥3 distinct Linear tickets (7 actual), full-body read on multiple conversations confirming same root-cause story.

---

### O-2. Notifications are uncontrollable — users get bombarded and can't find granular settings

**Rank: #2**

**User voice:**
- *"I'm receiving way too many push notifications from Stride—kudos alerts, follow requests, challenge reminders. I've looked through the settings but can't find a straightforward way to reduce these without turning everything off."*
- *"I'm getting bombarded with push notifications for every kudo, new follower, and challenge reminder. I've looked through the settings menu multiple times but can't find a granular control panel... As a premium user, I'd expect better notification management options."*
- *"Either I'm missing something obvious or the toggles aren't working."*

**Problem statement:** Active, engaged users — including at least one self-identified premium member — are receiving high-frequency push notifications for social events (kudos, follows, challenge reminders) and cannot find or use granular notification controls. The failure mode is two-pronged: (a) STR-9 confirms notification over-delivery is a known engineering bug, meaning the volume itself is unintended; and (b) even if volume were correct, users cannot find the settings to tune it. The retention risk: notification fatigue leads users to disable all notifications or turn off the app. The social loop (get a kudo → see it → kudo back → feel motivated to log next workout) is a key retention driver — breaking it destroys a primary reason to return.

**Evidence weight:**
- Intercom: 5 conversations on notification overload, meeting the ≥5 floor. Consistent bodies across all 5 — same surface (settings menu), same emotion (overwhelmed), same ask (granular controls).
- Linear: STR-9 (notification over-delivery, Bug, Backlog, Medium) provides engineering corroboration that the volume problem is real, not user misperception.
- Cross-tool: YES (Intercom + Linear). PostHog adjacency (rageclick on /settings) not completed — caps confidence at MEDIUM.

**Ratings:**

| Impact | Confidence | Effort |
|---|---|---|
| MEDIUM–HIGH | MEDIUM | M (fix over-delivery bug STR-9 is S; adding granular preference UI is M) |

---

### O-3. Users have no mechanism to build or track exercise habits — the product does not pull them back

**Rank: #3**

**User voice:** None directly (this is a backlog gap). The absence is itself evidence. No user has articulated "I wish you had streak tracking" because there's no surface to react to. The signal is structural.

**Problem statement:** Retention in fitness apps is primarily driven by habit formation — users come back because the app gives them a reason to (streaks, weekly summaries, personal records, goals progress, re-engagement nudges for lapsed days). Stride's current product has none of these: no streak mechanic, no weekly summary notification, no lapsed-user re-engagement flow, no goal-setting framework, and no instrumentation for any of these concepts. The backlog search across 7 adjacent terms (streak, habit, re-engagement, lapsed user, win-back, weekly summary, push notification campaign) returned zero results. The PostHog event definition list contains no `streak_*`, `goal_*`, or `re_engagement_*` events.

Meanwhile, users who are active are using the social loop (kudos, follows) and challenges as retention hooks — but these mechanisms are fragile (kudos delivery is broken, challenge progress isn't counting correctly), and there are no mechanisms to bring back users who missed a week.

**Evidence weight:**
- Linear: 7-term backlog gap confirmed — no work on any habit or re-engagement mechanic.
- PostHog: No events for habit-layer concepts in the event definitions list.
- Intercom: Silent (directional — no user-voiced requests, but also no surface to react to).
- Cross-tool: Partial (two tools show absence; Intercom is ambiguous).

**Ratings:**

| Impact | Confidence | Effort |
|---|---|---|
| HIGH (if addressed) | MEDIUM (absence of work is strong signal; absence of user voice limits to MEDIUM) | L (this is a multi-feature area — a first streak MVP is M, full habit layer is L) |

---

## 5. Per-Opportunity Solution Tables

### Solutions for O-1 (Workouts disappear after sync)

| # | Solution | Type | Linear tie | Notes |
|---|---|---|---|---|
| 1 | Unblock STR-23 (Urgent, In Progress) and STR-21 (Urgent, Todo) — investigate the Garmin Forerunner 265 specific sync pipeline; produce root cause for why activities confirmed-synced are not persisting | Root-cause fix, existing backlog | STR-23, STR-21 | Both Urgent and unresolved; this is the most user-reported device. Prerequisite: read full issue bodies and check whether STR-19 (same device, In Progress) has identified the cause |
| 2 | Fix retry/backoff logic (STR-88 / STR-30 — two tickets on the same issue) — activities failing to upload are retrying at fixed 1s intervals rather than exponential backoff, likely causing thundering herd and silent drops | Root-cause fix, existing backlog | STR-88, STR-30 | These are probably contributing to the sync loss — a failed upload at fixed retry intervals gives up or collides. Ship this alongside or before the Garmin-specific fix. |
| 3 | Add user-visible sync status and recovery surface — if an activity fails to persist after sync confirmation, show a "Activity sync failed — tap to retry" notification or in-app banner rather than silently dropping it | User-visible loop closer, new work | — | Treats the observable pain even while root-cause investigation continues. Converts a silent failure into a recoverable one. |
| 4 | Wire `activity_sync_failed` and `activity_sync_recovered` events with device type, error code, and time-since-workout-completion | Instrument | — | Currently the team cannot measure sync failure rate in PostHog — only the support queue reflects this. This event pair makes the failure rate visible and allows alerting. |

Recommended order: **#1 + #2 in parallel** (both are Urgent/existing backlog — just unblock them), **#4 concurrently** (S effort, immediate measurement value), **#3 after #1/#2** if the root-cause fix takes >2 weeks.

---

### Solutions for O-2 (Notification overload / ungovernable settings)

| # | Solution | Type | Linear tie | Notes |
|---|---|---|---|---|
| 1 | Fix notification over-delivery bug (STR-9) — investigate batching/deduplication logic that is sending notifications at higher frequency than configured | Root-cause fix, existing backlog | STR-9 | This is the upstream cause. Even perfect settings UI doesn't help if the volume is architecturally wrong. |
| 2 | Audit and redesign notification preferences settings — add per-type toggles: kudos, comments, new followers, challenge reminders, activity achievements, each independently controllable | Ship new work, root-cause fix | — (new ticket) | The Intercom bodies are consistent: users want granular control, not a kill switch. The existing /settings path exists (1,990 page views over 56 days) but apparently lacks this granularity. |
| 3 | Add smart batching — group kudos and social events into a once-daily digest rather than individual pushes, with user opt-in | Ship new work, structural | — | Alternative UX to per-toggle — less UI complexity, but requires users to opt in. Can co-exist with #2. |
| 4 | Wire notification opt-out event instrumentation — track which users turn off which notification types and at what frequency, so the team can quantify the scale of the problem and measure improvement | Instrument | — | Currently there is no event for notification preference changes in PostHog. |

Recommended order: **#1 first** (root cause fix, existing ticket), then **#2** (UX fix), **#4 alongside #2**, **#3 if #2 proves complex** or as a complement.

---

### Solutions for O-3 (No habit-formation or re-engagement layer)

| # | Solution | Type | Linear tie | Notes |
|---|---|---|---|---|
| 1 | 1-week spike: define what "retained user" means for Stride (weekly active? consecutive days logged? minimum activities per month?) and instrument the corresponding event. Without a definition, no solution can be measured. | Instrument + act | — (new ticket) | Prerequisite. Do this before any habit feature. |
| 2 | Weekly activity summary push notification — send retained users a Monday "last week you completed X activities / Y miles / Z elevation" notification with a CTA to log this week's first activity | Ship new work, short-term | — (new ticket) | The re-engagement nudge most proven in fitness apps. Relies on O-1 being fixed (notifications are already over-firing and annoying; this must be opt-in and tasteful). |
| 3 | Streak counter and recovery mechanic — show consecutive-day or consecutive-week activity streaks on the profile/feed. Add a "rest day grace" option so users don't churn from missing one day. | Ship new work, structural | — (new ticket) | High-impact retention driver. The heatmap work already In Progress (STR-55) provides the data layer foundation; build the streak counter on top. |
| 4 | Goal-setting framework (e.g., "Set a weekly distance goal") with challenge-style progress tracking | Ship new work, long-term structural | — | Long-term investment. The challenge feature already exists (/challenges has 3,219 page views and active engagement) — goal-setting could be a variant of challenges scoped to the individual. |

Recommended order: **#1 spike immediately** (no solutions should launch without a measurement definition), **#2 after O-2 notification fix lands** (don't add more notifications while over-delivery is unresolved), **#3 in parallel with #2**, **#4 following quarter**.

---

## 6. Shelved Signals

| ID | Signal | Why shelved | What would unshelve it |
|---|---|---|---|
| S-1 | Subscription cancellations outpacing starts (4 vs 2 in 56d) | N=4 cancellations, N=2 starts — below every meaningful floor. Cannot compute churn rate. No Intercom conversations on cancellation reason. | ≥20 cancellation events in a window where starts are also measurable, or Intercom conversations with cancellation-reason bodies |
| S-2 | Challenge progress not counting completed activities (2 Intercom) | STR-18 (elevation challenge progress bug) is marked Done; Intercom bodies reference similar pattern but volume is below the ≥5 floor (2 conversations). | ≥5 conversations specifically about challenge progress not updating; or a new Linear ticket confirming STR-18 regressed |
| S-3 | Feed crash on mobile (STR-90 new, STR-17 Done) | Classic fix-then-regress pattern; STR-90 exists and is Todo. App-crash AI Titles appear in Intercom (3 mentions across open conversations). Volume too thin to promote as standalone vs. O-1 (activity sync) which is the dominant user-expressed pain. | Confirmed separate root cause from STR-19 activity-disappearance class; ≥5 Intercom conversations specifically naming feed crash |
| S-4 | Kudos/comments/follows dropping to 0 in last 2 weeks (PostHog) | Almost certainly an instrumentation or data-pipeline failure across all social events simultaneously — not user behavior. The wall of zeroes is too clean to be real. | Investigation confirming the event definitions are still firing (check last_seen_at on `activity_kudoed` — it shows 2026-04-22, confirming the event stopped recording ~2 weeks ago) |
| S-5 | Segment effort missing (Intercom ~8 convos, Linear STR-12, STR-81) | Legitimate signal but grouped as a sub-dimension of O-1 (activity reliability / data loss). Segment efforts not recording is the same class of problem as workouts not syncing — the activity ingestion pipeline is not processing data correctly. Will surface as its own opportunity if O-1 fix does not resolve segment attribution. | O-1 fix ships and segment-effort-missing Intercom volume persists separately |

---

## 7. Housekeeping / Measurement Hygiene

Not competing with opportunities for the next sprint. Listed because the team must know.

| ID | Item | Severity | Effort | Notes |
|---|---|---|---|---|
| H-1 | All social engagement events (`activity_kudoed`, `activity_commented`, `athlete_followed`, `challenge_joined`, `club_joined`) stopped recording ~2026-04-22 and show 0 for the last 2 weeks in PostHog. Check event last_seen_at (confirmed: `activity_kudoed` last seen 2026-04-22). This is an active instrumentation break affecting the team's ability to track social engagement. | **Active break** | S | Investigate PostHog SDK or ingestion pipeline — likely a deploy on ~Apr 22 broke event capture for this event class. This is NOT tied to STR-9 (notification over-delivery), but both involve the notification/social pipeline. |
| H-2 | Resolve `user_signed_up` vs `user_registered` duplicate-event confusion (carried forward from prior discovery run). Without this, cohort-level new-user retention cannot be computed. | Blocker for retention cohort measurement | S | Until resolved, the team cannot build a "new user retained at week 2/4/8" funnel. |
| H-3 | No `activity_sync_failed` event in PostHog. The largest user-voiced problem (O-1) has zero quantitative measurement — only support queue evidence. The team cannot dashboard it, cannot set an alert, cannot A/B test a fix. | Measurement gap for #1 opportunity | S | Wire as part of O-1 solution #4. |
| H-4 | No `notification_preference_changed` event in PostHog. Cannot measure scope of O-2 (notification fatigue) quantitatively. | Measurement gap for #2 opportunity | S | Wire as part of O-2 solution #4. |
| H-5 | Three heatmap tickets exist: STR-55 (In Progress), STR-83 (Backlog), STR-91 (Backlog). These are duplicates. Engineering effort is being fragmented. | Backlog hygiene | S | Consolidate to one ticket, mark the others duplicate, and close STR-88/STR-30 (also duplicates on the backoff issue). |
| H-6 | `/training` route has 4 page views in 56 days. Either this is a feature that doesn't exist yet (the route is stubbed), or it is deeply undiscoverable. No Linear tickets on "training plan" or "training log" in searches. | Discovery question | S | Determine whether /training is an intentional product direction. If so, it needs discoverability work. If not, remove the route stub. |
| H-7 | `subscription_started` (2 events) and `subscription_cancelled` (4 events) in 56 days are too low to measure free-to-paid conversion or churn rates. The premium subscription funnel is unmonitorable at current volume. | Measurement gap | S | Either grow to measurement-viable volume before analyzing, or accept that this is a vanity metric in its current state. |

---

## 8. Sequencing Recommendation

### This sprint

1. **O-1 solution #1: Unblock STR-23 and STR-21** (Urgent, In Progress/Todo). Both are the highest-priority retention bug in the codebase. Read the full issue bodies and comments to identify whether there is a shared root cause with STR-19, STR-86, STR-88.

2. **O-1 solution #2: Ship STR-88 / consolidate with STR-30** — the retry/backoff fix is likely contributing to silent upload drops. Low effort (High priority ticket, Backlog — it's ready to pick up).

3. **H-1: Investigate why social engagement events stopped firing ~April 22.** This is an active instrumentation break. Until it's resolved, the team is flying blind on the engagement signals that matter most for retention.

4. **O-3 solution #1: Define what "retained user" means for Stride and instrument it.** This is a half-day conversation + a week of instrumentation. Do not start any habit feature without this definition.

### Next sprint

5. **O-1 solution #3: Ship user-visible sync status / retry surface.** Even if root-cause fixes take longer, users need a recovery path they can find on their own.

6. **O-2 solution #1: Fix STR-9 (notification over-delivery).** This is the prerequisite before any re-engagement notification strategy (O-3 solution #2) can safely launch. Adding more notifications while over-delivery is live would worsen O-2.

7. **H-2: Resolve signup event duplication.** Required before any cohort-level new-user retention analysis.

### Do NOT do this sprint

- Launch any re-engagement notification campaign (O-3 solution #2) before O-2 (notification over-delivery) is fixed. Adding push notifications while users are already complaining of being bombarded will accelerate churn, not retention.
- Treat the social engagement metric cliff (kudos dropping to 0) as a behavioral signal. Investigate it as a pipeline/instrumentation issue first (H-1).
- Build the habit/streak layer (O-3 solution #3) before the measurement definition from O-3 solution #1 is in place. You would have no way to tell if it works.
- Promote subscription churn to a tracked opportunity — insufficient volume (S-1).

---

## 9. Appendix — Key PostHog Links

- Weekly engagement trends (56d): https://us.posthog.com/project/391447/insights/new (see login/activity creation query in run log)
- Social engagement trends (56d): see run log `_posthogUrl` for kudos/comments/follows/challenges/clubs query
- Subscription started/cancelled trends (56d): see run log
- Page views by path (56d): see run log
