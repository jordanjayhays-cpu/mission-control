# n8n audit — brief for a fresh session

Written 2026-09-19. Jordan asked to start using n8n; the connector dropped mid-session and a
remote session cannot re-run OAuth. **Start a new session and work this brief.**

## Why this exists

n8n loaded 37 tools at the start of the 2026-09-19 session, then disconnected and began requiring
authentication. Jordan had already authorised it — nothing was wrong on his side. Re-authorising is
not the fix; a fresh session that re-establishes the connection is.

## The known problem

From the master map, recorded 2026-08-22:

> the ~16 "active" Axton workflows from May 4 never execute — their triggers point at the dead
> Railway stack. Inert; clean up in an n8n-lane session someday.

So the subscription is paying to host roughly sixteen workflows that fire at nothing, dead since
early May.

## What the fresh session should do, in order

1. `search_workflows` — list every workflow, active flag, and last execution time.
2. `search_workflow_executions` — for each active one, when did it last actually run? A workflow
   marked active with no executions since May is the zombie pattern.
3. For each zombie, establish what it was *meant* to do before it is archived. Some may be worth
   rebuilding against live infrastructure rather than deleting.
4. `list_credentials` — check what is still attached. Dead workflows may hold live credentials.
5. Produce a one-page verdict: **rebuild / archive / cancel the subscription.**

## The question behind the question

Jordan already automates with two layers that work:

- **Supabase edge functions + pg_cron** — the senders, the board jobs, `agent-worker`. Pennies.
- **Claude Routines** — 26 of them, roughly ten sessions a day.

n8n would be a **third** layer. The audit should answer honestly whether it earns its place, or
whether the right recommendation is to cancel. Do not invent a use for it.

Where n8n genuinely wins over the other two: connecting third-party SaaS that has no API worth
writing against, and visual flows a non-developer can edit. Weigh it on that, not on enthusiasm.

## Verdict shape

One page. Three columns: workflow name, last real execution, recommendation. Then one sentence on
whether to keep paying.
