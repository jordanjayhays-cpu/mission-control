# MASTER MAP — Jordan's system (read this first, from any session)

This is the one source of truth for how the whole system fits together. Every other repo's
CLAUDE.md is a lane file that points here. Mission Control is the right home: it is the
master-overview repo. If this file disagrees with the live board, the board wins.

## The map (don't go looking for these)

| Thing | Where |
|---|---|
| Agent board + PISCO outreach DB | Supabase `neurodashboards` — `dprdnrgjkzgfgtcsguuq` |
| Massage Club DB (separate, keep it that way) | Supabase `jglftdstrowwckwqmpue` |
| Agent Command Center (live dashboard) | Lovable `d9cccd2d-c8f9-408f-91da-5ff739da5efd` ↔ repo `neurodash-agent-dashboard`, live at neurodash-agent-dashboard.lovable.app (verified 2026-08-22 via commit sync) |
| Sauce Match (Lovable) | Lovable `dc87991c-5132-40e3-a8b0-3626e286f082` ↔ repo `sauchematch`, live at sauchematch.lovable.app. Lovable syncs the repo's **main** branch only; a feature branch is invisible to Lovable until merged. Git pushes cost no credits; Lovable edits do. |
| Niah live app | syncs to **Niahconnect/niah-matchmaker-pro** (private, other org). Its Lovable project id is UNSURE — verify in Lovable before any edit; do NOT assume it is d9cccd2d (an old docs error said so; it is not) |
| Hermes runtime | Railway "Hermes 007" — OpenClaw gateway; its crons live on the box, not in git |
| Repos in use | `your-massage-pass`, `007-Axton`, `mission-control`, `niah-dashboard`, `neurotech-dashboard`, `neurodash-agent-dashboard`, `jordan-projects` (deliverables drop), `Philosophical-King` (PK design system + music docs, created 2026-09-07), `Vaya`, `pk-render-worker` |
| Kinsol contracts (Placewell referral + Amigo Sales billing, drafts v1) | Notion: https://app.notion.com/p/3d3efcda373d81e9a1e5c148c4eb4173 ; Word + md in `jordan-projects/kinsol/contracts/` |

Key tables on `neurodashboards`: `pisco_prospects` (outreach CRM), `agent_tasks` (Jordan's to-dos),
`hermes_entries` (activity feed), `agent_prompts` (agent-to-agent bus), `app_secrets` (SMTP/API keys).

Edge functions: `pisco-writer` (drafts outreach, gpt-4o-mini), `pisco-sender` (SMTP send as
jordan@placewell.io), `claude-responder`, `hermes-responder`, `agent-worker`.

## Lovable projects — the full registry (verified 2026-09-15)

All 15 live in workspace `4yaletByfIWogrs8SgdG` ("Jordan's Lovable"). **Check this table before
editing any Lovable app.** A session once pushed a neurotech data build into the Agent Command
Center because the map listed only two projects and the real target was missing.

| Live URL | Display name | Project id | Last edited |
|---|---|---|---|
| brain-vista-hub.lovable.app | Neuro Dashboard Live — **the neurotech tracker** | `621d7a65-d180-49a3-919f-cead001b15d8` | 2026-09-15 |
| neurodash-agent-dashboard.lovable.app | Agent Command Center | `d9cccd2d-c8f9-408f-91da-5ff739da5efd` | 2026-09-03 |
| sauchematch.lovable.app | Sauce Match | `dc87991c-5132-40e3-a8b0-3626e286f082` | 2026-09-11 |
| massage-madrid-magic.lovable.app | Your Massage Pass | `13ab3b1d-1034-4ac7-b40c-8e51807e553c` | 2026-09-08 |
| clever-family-coach.lovable.app | Family Business Flourish (folding into Amigo Sales) | `3c4a237b-8b84-4763-a55b-a19d5b83e4a5` | 2026-08-10 |
| niah-matchmaker-pro.lovable.app | Niah Matchmaker — **resolves the old UNSURE** | `9eab0291-65d5-4f37-93a9-7969aa8c4393` | 2026-07-22 |
| dream-pitch-made.lovable.app | Remix of Pitch Perfect Plan | `851f9897-c313-4160-bfd6-54cd3ad373d0` | 2026-07-09 |
| niahonepagerr.lovable.app | One pager Pitch | `e92af7fb-9b8e-42c9-aa39-d6ff19ab2931` | 2026-07-09 |
| (unpublished) | Pitch Perfect Summary | `3902f527-7bbd-4347-b23a-6ab418eddb59` | 2026-07-08 |
| (unpublished) | Pitch Perfect Visuals | `2949ce20-d860-4590-a9e4-9a701684aea1` | 2026-07-04 |
| startupmetricsniah.lovable.app | Startup metrics | `3dd21321-2afb-4375-8f1c-e49a0b8e043a` | 2026-06-29 |
| openclaw-dream-map.lovable.app | Map Collective | `70566da2-4160-45d1-bf09-50bbbbc7ae7a` | 2026-06-20 |
| vivid-persona-builder.lovable.app | Profile Stack Studio | `3722bffb-1f65-4bb3-961e-e579fa548367` | 2026-05-17 |
| build-a-bit-further.lovable.app | Build & Grow | `fb041128-f4c1-4e1b-a46f-588db27e2a3b` | 2026-05-06 |
| green-spark-planner.lovable.app | EV Charger Navigator | `05eab428-45ed-49df-b429-27ec91124571` | 2026-04-18 |

**Which ones sync from GitHub matters more than the id.** A repo-synced project (Sauce Match, the
Agent Command Center) takes free git pushes to its **main** branch — a feature branch is invisible
to Lovable. A project with no repo (brain-vista-hub) can only be changed by a Lovable message, and
that spends credits. Confirm which before promising an update.

**Neurotech data now lives in Supabase.** `neurotech_entries` on `neurodashboards`
(kind / sort_order / data jsonb, 233 rows, public read, writes denied to anon). brain-vista-hub
fetches it at runtime, so refreshing the data is a free SQL update — no Lovable credits.

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

## Bring Jordan the frontier — he should never have to ask

Jordan's complaint, 2026-09-07, verbatim intent: *"Why do I have to come to you and say 'here's some AI
to research'? You should come to ME."* He was right, and the failure was not a capability limit — the
gap (no evals) was already written in PROJECTS-OVERVIEW.md that same morning and reported only when
he raised it.

Binding from now on:
- **A finding you have is a finding you surface.** If a session notices a capability gap, a cheaper
  model, a better pattern, or a tool that would move a lane, it says so in that session — not when asked.
- **The scan is scheduled, not remembered.** `AI capability scan` Routine (`trig_01KFcirbEmiZ6ZTXg6Ms5Ys3`,
  Thursdays 08:00 Madrid) researches what changed and files at most ONE YES/NO task titled `AI SCAN:`.
  Memory does not survive a session; a cron does. Anything Jordan should hear regularly gets a Routine.
- **The filter is revenue, not fashion.** Would it change a real-world number in 30 days? If not, discard
  it and say nothing. Most weeks the honest answer is "nothing qualified" — see `EVALS.md` for what we
  deliberately do not build and why.
- **Never sell him anxiety.** He is ahead on harness, context engineering, MCP, multi-agent and cost
  discipline. His bottlenecks are human: studios that do not answer, contracts unsigned, stock unbought.
  Say that plainly instead of recommending a vector database.

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
