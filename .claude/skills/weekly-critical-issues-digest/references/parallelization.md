# Parallelizing high-volume Intercom reads

The spec requires 100% coverage of Intercom conversations in the window. For weeks with 100+ tickets, reading inline blows out main-context tokens. Use parallel subagents.

## When to parallelize

Decided after `Step 2` returns the true `total_pages` count from the `per_page: 1` probe.

| Volume | Workers | Tickets per worker |
|---|---|---|
| 1–25 | 1 (main context, `per_page: 25`) | all |
| 26–100 | 3 | ~30 |
| 100–250 | 8 | ~25 |
| 250+ | 12 | ~25 |

Math must reconcile: `workers × tickets_per_worker ≈ total` (within rounding). The Methodology section needs to state these numbers.

## How to split the window

Don't paginate via `starting_after` cursors across workers — cursors are sequential and you can't predict them. Split by `created_at` time-range instead:

1. Compute the window boundaries: `start_ts = now - 7 days`, `end_ts = now`.
2. Divide the window into N equal chunks: `chunk_size = (end_ts - start_ts) / N`.
3. Each worker gets `chunk_start` and `chunk_end` and queries:

```
mcp__intercom__search_conversations(
  created_at: { operator: ">=", value: chunk_start },
  // No "<" operator on created_at — workaround: filter by both >= start and updated_at filter, OR
  // run with both bounds in a follow-up query, OR
  // accept that boundary tickets may appear in two chunks (dedupe by ID in synthesis).
)
```

If the Intercom search tool supports only `>=`, the simpler approach is: each worker queries `created_at >= chunk_start` with `per_page: 25`, paginates until it hits a ticket older than `chunk_end`, then stops. Workers report `(chunk_start, chunk_end, ticket_ids_returned)` so the main thread can verify no overlap.

## Worker prompt

Each subagent prompt MUST include:

- Exact time range (`chunk_start` Unix → `chunk_end` Unix).
- Exact list of fields to extract per ticket: `id`, `ticket.custom_attributes._default_title_.value`, `ticket.custom_attributes._default_description_.value`, `source.body` (HTML — strip tags), `ticket.custom_attributes."Root cause".value`, `ticket.custom_attributes.Platforms.value`, `statistics.first_admin_reply_at`, `statistics.count_assignments`, `last_admin_reply_at`, `priority`, `state`, `company`.
- Output format: a structured table with one row per ticket. No prioritization or theme synthesis at the worker level.
- Instruction to NOT read the full conversation thread for every ticket — the search payload is enough. Only fetch full conversation parts via `get_conversation` for tickets the main thread later flags as candidates for the in-flight-work admin-reply quote.

Example worker prompt skeleton:

```
You are processing Intercom chunk <i> of <N> for the weekly critical-issues digest.

Window: created_at >= <chunk_start> AND created_at < <chunk_end> (Unix).

Call mcp__intercom__search_conversations with per_page: 25, paginate via starting_after until you've read every ticket in the window. For each ticket, extract: <field list>.

Return a markdown table sorted by created_at desc. Do NOT cluster, theme, or prioritize — that's the main thread's job. Quote customer language verbatim, under 15 words per quote, in straight double-quotation marks.

If a tool result exceeds context limits and gets saved to disk, read it in chunks of ~190 lines and continue. Do not skip tickets.
```

## Reconciling worker output

After all workers return:

1. **Dedupe by `id`** — boundary tickets may appear twice if the chunking missed.
2. **Verify total** — sum of unique IDs from all workers should equal the `total_pages` count from Step 2's probe (within ±2 for race conditions during the run).
3. **If short by >2:** investigate before synthesizing. Likely cause: a worker hit a token limit and silently dropped tickets. Re-spawn that worker with a tighter chunk.
4. **Cluster themes** in main context using the `Root cause` attribute as the seed when present.
5. **Pull full conversation parts** only for tickets you intend to cite as in-flight-work evidence (those with non-null `last_admin_reply_at`).

## When NOT to parallelize

- Volume ≤ 25. The overhead exceeds the benefit.
- The week's total is unknown. Run the `per_page: 1` probe first; don't guess.
- The spec check `methodology_parallelization_stated` accepts `1 worker × N tickets = N` — single-worker is a valid strategy for small volumes, just state it.
