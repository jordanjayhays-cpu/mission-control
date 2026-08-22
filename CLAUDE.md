# MASTER MAP — Jordan's system (read this first, from any session)

This is the one source of truth for how the whole system fits together. Every other repo's
CLAUDE.md is a lane file that points here. Mission Control is the right home: it is the
master-overview repo. If this file disagrees with the live board, the board wins.

## The map (don't go looking for these)

| Thing | Where |
|---|---|
| Agent board + PISCO outreach DB | Supabase `neurodashboards` — `dprdnrgjkzgfgtcsguuq` |
| Massage Club DB (separate, keep it that way) | Supabase `jglftdstrowwckwqmpue` |
| Agent Command Center (live dashboard) | Lovable project `d9cccd2d-c8f9-408f-91da-5ff739da5efd` → syncs to **Niahconnect/niah-matchmaker-pro** (private, other org) |
| Hermes runtime | Railway "Hermes 007" — OpenClaw gateway; its crons live on the box, not in git |
| Repos in use | `your-massage-pass`, `007-Axton`, `mission-control`, `niah-dashboard`, `neurotech-dashboard` |

Key tables on `neurodashboards`: `pisco_prospects` (outreach CRM), `agent_tasks` (Jordan's to-dos),
`hermes_entries` (activity feed), `agent_prompts` (agent-to-agent bus), `app_secrets` (SMTP/API keys).

Edge functions: `pisco-writer` (drafts outreach, gpt-4o-mini), `pisco-sender` (SMTP send as
jordan@placewell.io), `claude-responder`, `hermes-responder`, `agent-worker`.

## Token discipline

Long sessions are the dominant cost — every turn re-reads the whole conversation. Keep a session to
one lane (build / outreach / planning). Start a new session when the topic changes.

When calling tools:
- GitHub: always `minimal_output: true` or an explicit `fields` list.
- SQL: name the columns, always `LIMIT`. Never `select *` on `pisco_prospects` or `hermes_entries`.
- Logs: filter and limit. Fetching a log stream wholesale costs ~15k tokens.
- Edge functions: iterate on the source in a local file, then deploy **once**.
- Reading files: request the range you need, not the whole file.

## Delegation

Ask: *could a cheap model do this with clear instructions?* If yes, it belongs in an edge function
on gpt-4o-mini, not in a Claude session.

- **gpt-4o-mini (edge functions):** drafting from a template, summarising, classifying, reformatting,
  bulk generation. `pisco-writer` is the working example.
- **Claude:** diagnosis, architecture, judgement calls, anything where being wrong is costly.

### The rule: never load bulk rows into context

**If a task means reading more than ~20 rows, delegate it to `agent-worker` instead of SELECTing
them.** One measured call scanned 346 prospect rows and returned ~800 tokens of answer.

```bash
# ask — analyse rows, get a short answer back
curl -s -X POST https://dprdnrgjkzgfgtcsguuq.supabase.co/functions/v1/agent-worker \
  -H "Content-Type: application/json" -H "x-cron-key: $CRON_KEY" -d @job.json
# job.json: {"mode":"ask","query":"select ... from ...","instruction":"..."}
# fill mode: {"mode":"fill","query":"...","table":"...","target_column":"...","instruction":"...","limit":25}
```

Write the JSON to a file and use `-d @file` — inline quoting mangles the SQL.

Trigger delegation when: the query returns more than ~20 rows; the task is repetitive across
records; or the session is already long. Never delegate judgement or architecture.

## House rules learned the hard way

- Cold outreach goes out **one at a time** and only to verified emails of real, named people.
- Email format: greeting on its own line, one sentence per paragraph, no em dashes, rotating subject
  lines, signature = `Thank you, / Jordan / Director of Business Development / placewellinternational.net`.
- Anything only Jordan can do goes in `agent_tasks` assigned to `jordan` — his 09:00 Madrid brief
  reads that table. If it is not on the board, it does not exist.
- Cron jobs that call Claude are the expensive ones. `claude-responder` runs every 15 min; do not
  put it back to every 5.

## Cron + alerting rules (learned 2026-08-22, the hard way)

**Gate before the model, never after.** `hermes-responder` returns early when its inbox is empty and
only then calls the LLM — copy that shape. `claude-responder` v13 did the opposite and re-priced
identical input every 15 min, up to 96x/day at ~21.5k tokens each. Fixed in v14 with an idempotency
gate keyed on `agent_state['claude-responder:last_seen'].entry_id`.

**Rate limits are not deduplication.** `detect_stalls()` had "one alert max per 2 hours" — a
throttle that re-sent the same three blocked tasks 12x/day for weeks (245 rows archived). The fix
is a **fingerprint**: `md5()` over stable identity keys (task id + kind) only. Never let volatile
text into the hash. Re-alert only when the fingerprint *changes*, plus a 7-day `repeat_interval`.

**Never put Jordan's own tasks in an agent-stall alert.** His to-dos reach him via the 09:00 brief —
the stall alert is for *agent* work that is stuck.

**An alert with no path to a human is not an alert.** `detect_stalls()` upserts ONE
`assigned_to='jordan'` task (refreshed in place, never duplicated) so it lands in the brief.

**Prompt caching does not apply to claude-responder.** The cacheable prefix (~714 tok) is under the
1024-token minimum and the 15-min cron exceeds the 5-min default TTL. Enabling it would add a
~1.25x write premium on 100% misses. Do not "optimise" this again.

**Keep the reasoning window conversational.** MODE 3 excludes `agent='system'` and truncates bodies
to 400 chars.

## Infra cleanup rules (learned 2026-08-22)

**Railway destructive ops need 2FA and CANNOT be done over an API/MCP token.** `removeServiceTool`
reports "marked for removal" but the change is only *staged*. Always re-check `get-service-metrics`
afterwards. Only Jordan can Apply, from the dashboard, with 2FA. Never report a Railway deletion as
done without that read-back.

**Verify a service's config before deleting it, not just its metrics.**
- `render-worker` (pk-render-pipeline) — Whisper transcription worker, repo `pk-render-worker`.
  Never ran since 2026-08-10. Broken, not disposable.
- `nanobot` (abundant-radiance) — live public domain + admin creds + a persistent volume at `/data`.
  Deploys from `codestorm-official/nanobot`, not Jordan's fork.
- `Hermes Agent` is the live OpenClaw gateway (~5 GB RAM, active) — never touch it.

**One-shot pg_cron jobs are landmines — unschedule them after they fire.** Cron has no concept of
"once": six PISCO sends pinned to `... 18 8 *` would have re-fired 2027-08-18. All unscheduled.

## Where the tokens actually go: Routines, not cron

Each Routine firing spawns a whole Claude session. Edge-function crons are pennies by comparison.
Before optimising anything else, run `list_triggers` and count firings per day.

Audit 2026-08-22: 28 triggers ~16 sessions/day → 21 triggers ~10 sessions/day. `Neurodash PM +
worker` cut from 8x/day to 2x/day (`0 7,15 * * *`) — reduce cadence before killing a useful agent.
Measure value as *board movement per firing*, not uptime. A Routine that runs perfectly and changes
nothing is the most expensive kind of green dashboard.

## Outreach safety + retention (2026-08-22)

**PISCO sends are verified-only — do not loosen this.** `pisco_daily_send()` and
`pisco_daily_followup()` filter `email_status = 'verified'` AND send `only_verified:true`. This
protects the jordan@placewell.io sender reputation.

**Never mass-rewrite `tier` to force a pick.** The old code flattened the whole queue to tier 5 on
every send, destroying the priority ranking. Demote only the previous target (`AND tier = 0`).

**Telemetry prunes itself.** `prune_telemetry()` (cron `prune-telemetry-daily`, 03:20) drops
`agent_heartbeats` >14 days and completed `agent_prompts` >30 days.

**`linkedin-poster-daily` was a zombie — unscheduled.** Fired daily for a month, posted nothing
(both queued items held for Jordan's approval). Check output, never status.

## The decision queue (the operating model)

The system's throughput limit is Jordan's decisions/day, not agent capacity.

- **The 09:00 brief is a DECISION brief, max 5 items,** each phrased YES/NO/KILL (trigger
  `trig_01WnxP7aL5oYfSQMHCGsG95A`). It also carries a rotating drill of the day.
- **Queue hygiene:** one task per sitting (merge batches), standing plays are process docs not
  queue items, date-bound tasks whose window passed get closed not nagged, gated items get
  priority=low until the gate opens.
- **A task featured 3+ times unanswered gets flagged, then archived in 7 days.**
- **Before adding to Jordan's queue, try to just do it.**
- n8n Cloud note: the ~16 "active" Axton workflows from May 4 never execute — their triggers point
  at the dead Railway stack. Inert; clean up in an n8n-lane session someday.

## Operating rules — Jordan × Claude (binding in every session)

- **Ambiguous ask → interview first.** Up to 5 short questions before producing anything big.
- **UNSURE beats plausible.** Never state a fact, name, or email you haven't verified.
- **Creative work → 3 versions** (safe / bold / weird), one-line tradeoff each.
- **Outward-facing work → premortem.** Top 3 failure modes, fix, then proceed.
- **State acceptance criteria.** Big deliverables end with "Accept when: …".
- **Call context rot.** If the session mixes lanes or grows long, hand Jordan a 3-line handoff.
- **Formats come from examples.** Ask for one example rather than accepting prose descriptions.

Jordan is running the 30-day ladder (the "1-to-10 Ladder" artifact). The 09:00 brief carries a
daily drill; a Friday check-in (ends Sep 26) emails the weekly habit score. Remind, don't lecture.
