# PostHog queries for the digest

All queries assume `mcp__posthog__query-run` with a HogQL kind. Replace event names with your project's product events (discoverable via `mcp__posthog__event-definitions-list`).

## Default queries (always run)

### Week-over-week event volumes

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

Look for:
- Events that went `>0 → 0` (instrumentation broke or feature unused).
- Events with negative delta >30% week-over-week.
- New events that appeared this week (could be a deploy artifact).

### Daily breakdown — catches single-day collapses

```sql
SELECT toDate(timestamp) AS day, event, count() AS volume
FROM events
WHERE event IN (<your product events>)
  AND timestamp >= now() - INTERVAL 14 DAY
GROUP BY day, event
ORDER BY day DESC, volume DESC
```

Look for: any day where a previously-active event has 0 rows. A single zero day for a daily-fired event is HIGH severity.

### Rageclick URLs

```sql
SELECT properties.$current_url AS url, count() AS rageclicks
FROM events
WHERE event = '$rageclick' AND timestamp >= now() - INTERVAL 7 DAY
GROUP BY url ORDER BY rageclicks DESC LIMIT 20
```

Cross-reference URLs against Intercom themes — `/auth` rageclicks alongside password-reset tickets, `/sync` rageclicks alongside sync-failure tickets, etc. Note `localhost` URLs indicate the production-vs-dev environment may not be cleanly separated.

## Optional queries (run when initial signals warrant)

### Funnel drop — identify a step regression

Use the InsightVizNode FunnelsQuery shape. Pick a known multi-step product flow (signup, checkout, activation) and compare its conversion against the prior 7 days using `compareFilter`. Investigate any step with >10pp absolute drop.

### Breakdown by platform — separate iOS / Android / Web

```sql
SELECT properties.$os AS os,
       event,
       count() AS volume
FROM events
WHERE event IN ('$rageclick', '<crash event>', '<error event>')
  AND timestamp >= now() - INTERVAL 7 DAY
GROUP BY os, event
ORDER BY volume DESC
```

Surfaces platform-specific issues (e.g., the iPhone 15 Pro crash class).

### Retention regression — week 1 retention drop

Use the canonical signup event (verify via `event-definitions-list` — duplicate-event issues are common, e.g. `user_signed_up` vs `user_registered`). Compare the cohort that signed up two weeks ago to the cohort that signed up the prior week. If week-1 return rate dropped >5pp, that's an issue.

### Top errors

```
mcp__posthog__error-tracking-issues-list(limit: 50)
```

If this returns 0, that itself is a finding — likely instrumentation gap. Note in Limitations.

### Subscription churn signals

```sql
SELECT event, count() AS volume
FROM events
WHERE event IN ('subscription_started', 'subscription_cancelled', 'subscription_paused')
  AND timestamp >= now() - INTERVAL 14 DAY
GROUP BY event
```

Compare cancellations week-over-week — a spike is HIGH severity.

## How signals translate to severity

| Signal | Default severity |
|---|---|
| Daily zero-event day for a previously-active product event | HIGH |
| Week-over-week event volume drop >40% on a core engagement event | HIGH |
| Week-over-week event volume drop 20–40% | MEDIUM |
| Rageclick cluster on a single URL with corroborating Intercom tickets | MEDIUM (HIGH if >20% of tickets) |
| Rageclick cluster with no Intercom corroboration | LOW (worth flagging, not promoting) |
| `error-tracking-issues-list` returning 0 when crashes are reported in tickets | This is a Limitations note, not a top issue |

These are floors. Severity can be raised based on customer impact, paid-tier exposure, or week-over-week direction (rising is worse than steady).
