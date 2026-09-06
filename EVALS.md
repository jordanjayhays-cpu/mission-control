# Evals + observability for Jordan's agent system (2026-09-07)

**The gap, in one number:** 64 tasks marked done in the last 30 days; **22 recorded any output**.
42 were closed with no evidence of what changed in the real world. The system can currently
report a perfect week while nothing moved. That is the failure mission-control's own CLAUDE.md
already names — "check output, never status" — but nothing enforces it.

Everything below is deliberately small. No vector DB, no fine-tuning, no gateway. Those would not
earn a euro this month. Evals and observability would.

## 1. Every closed task must say what changed

Add to `agent_tasks`:
- `outcome` (enum): `shipped` (code/config live), `answered` (a decision recorded), `contacted`
  (a real person reached), `no_change` (looked, nothing to do), `blocked`.
- `evidence` (text): the URL, commit sha, row id, or message id that proves it. One line.

Rule: an agent may not set `status='done'` without `outcome` and `evidence`. A task closed as
`no_change` is a fine and honest result — a task closed with an empty `evidence` is not.

## 2. The lane scorecard — the only eval that matters

One row per venture per week, written by a Sunday job into `lane_metrics`
(`week, lane, metric, value, delta`). Each lane has ONE real-world number:

| Lane | The number |
|---|---|
| Massage Club | confirmed bookings (funnel_events `confirmed`, MC DB) |
| Placewell / PISCO | replies received (needs `reply_status`, already queued) |
| Amigo Sales | client conversations held |
| Sauce Match | paid orders |
| Philosophical King | pieces published |
| Niah | organiser replies (from Instantly) |
| Vaya | services delivered |
| Puhunan | loans made |

If a lane's number is flat for 3 weeks, the brief asks Jordan one question: change it or park it.
This replaces "the agents were busy" with "the business moved".

## 3. Agent eval: judge the drafts, not the sends

For every drafting agent (`pisco-writer`, the Niah pitches, the Amigo check-ins), store the
template version on each send and report **reply rate by template**. Two templates, ten sends
each, one wins. That is the whole eval; it needs no framework.

For code-writing sessions, the eval is already there and unused: did the branch merge, and did
the funnel number move in the following week.

## 4. Observability that reaches a human

Tables already exist and nobody reads them: `agent_heartbeats` (215 rows this week),
`codex_spend` (96 rows), `agent_efficiency`, `ai_calls`. The Venture Board (queued) surfaces the
lane numbers. Add to the Sunday job a **three-line weekly note** in the brief:
lanes that moved, lanes that did not, spend for the week. Nothing else.

## What we are deliberately NOT building
Graph engineering, RAG 2.0, vector DBs, fine-tuning, synthetic data, distillation, AI gateways,
prompt-optimization tooling. Jordan's system is already ahead on harness, context, MCP,
multi-agent and cost. Adding the fashionable middle of the stack would add cost and no revenue.
Revisit only when a lane's bottleneck is genuinely retrieval or model quality.

**Accept when:** no task can be closed without evidence; `lane_metrics` has 4 weeks of rows; the
Sunday brief carries the three-line note; and at least one lane has been changed or parked
because its number stayed flat.
