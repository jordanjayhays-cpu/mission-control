# MASTER MAP — Jordan's system (read this first, from any session)

One source of truth for how the system fits together. Every other repo's CLAUDE.md is a lane file that
points here. If this file disagrees with the live board, the board wins. Kept deliberately short: every
line here costs every session. The stories behind the rules live in `docs/postmortems.md`; the Lovable
registry in `docs/lovable-registry.md`; the operating-system design and its sources in
`docs/claude-operating-system.md`. Pruned 2026-10-22 from 238 lines; no binding rule was dropped.

## The map (don't go looking for these)

| Thing | Where |
|---|---|
| Agent board + PISCO outreach DB | Supabase `neurodashboards` — `dprdnrgjkzgfgtcsguuq` |
| Massage Club DB (separate, keep it that way) | Supabase `jglftdstrowwckwqmpue` |
| Agent Command Center (live dashboard) | Lovable `d9cccd2d-c8f9-408f-91da-5ff739da5efd` ↔ repo `neurodash-agent-dashboard`, neurodash-agent-dashboard.lovable.app |
| Sauce Match | Lovable `dc87991c-5132-40e3-a8b0-3626e286f082` ↔ repo `sauchematch`, sauchematch.lovable.app |
| Niah live app | syncs to **Niahconnect/niah-matchmaker-pro** (private, other org); Lovable id `9eab0291-65d5-4f37-93a9-7969aa8c4393` (resolved 2026-09-15; it is NOT d9cccd2d) |
| Hermes runtime | Railway "Hermes 007" — OpenClaw gateway; its crons live on the box, not in git |
| Repos in use | `your-massage-pass`, `007-Axton`, `mission-control`, `niah-dashboard`, `neurotech-dashboard`, `neurodash-agent-dashboard`, `jordan-projects` (deliverables drop), `Philosophical-King`, `Vaya`, `pk-render-worker` |
| Amigo Sales email (live 2026-09-19) | Domain `amigosales.com` on Cloudflare (partner-administered); `jordan@`, `support@`, `cata@`. **Not wired into `send-email`**: agents still send as jordan@placewell.io |
| Kinsol contracts (drafts v1) | Notion https://app.notion.com/p/3d3efcda373d81e9a1e5c148c4eb4173 ; `jordan-projects/kinsol/contracts/` |
| Lovable (15 projects, workspace `4yaletByfIWogrs8SgdG`) | `docs/lovable-registry.md`. **Check it before editing any Lovable app.** Repo-synced projects take free git pushes to **main** only; no-repo projects need a Lovable message, which spends credits |

Key tables on `neurodashboards`: `pisco_prospects` (outreach CRM), `agent_tasks` (Jordan's to-dos;
`outcome` + `evidence` columns since 2026-10-22), `lane_metrics` (weekly numbers per lane),
`hermes_entries` (activity feed), `agent_prompts` (agent-to-agent bus), `app_secrets` (SMTP/API keys),
`pk_run_log`. Edge functions: `pisco-writer` (drafts outreach, gpt-4o-mini), `pisco-sender` (SMTP as
jordan@placewell.io), `claude-responder`, `hermes-responder`, `agent-worker`.

## Focus (binding)

Max 3 active lanes at a time (rule of 2026-09-06; it was widened the next day and the cost showed:
130 of 223 Jordan tasks cancelled, 3 of 24 sessions on BD). The current 3 are UNSURE until Jordan
names them (board, 2026-10-22). Until then: no new lane sessions, no new Routines, and every build
request gets the question "does this get the next 5 bookings / meetings / deals?" If not, it waits.

## Token discipline

Long sessions are the dominant cost. One lane per session; new topic, new session. GitHub calls:
`minimal_output: true`. SQL: named columns, always `LIMIT`; never `select *` on `pisco_prospects` or
`hermes_entries`. Logs: filter and limit. Edge functions: iterate locally, deploy once. Files: read
ranges, not wholes. Routines spawn whole sessions and are the real spend: run `list_triggers` and
count firings/day before optimising anything else; reduce cadence before killing a useful one; the
measure of a Routine is board movement per firing, not uptime.

## Delegation

Could a cheap model do it with clear instructions? Then it belongs in an edge function on
gpt-4o-mini (`pisco-writer` is the pattern), not in a Claude session. Claude is for diagnosis,
architecture and judgement. **Never load more than ~20 rows into context**: delegate to
`agent-worker` (`POST https://dprdnrgjkzgfgtcsguuq.supabase.co/functions/v1/agent-worker`, header
`x-cron-key: $CRON_KEY`, body from a file with `-d @job.json`:
`{"mode":"ask"|"fill","query":"select ...","instruction":"...","table":...,"target_column":...,"limit":25}`).

## Hard rules (each cost real money or trust; the story is in docs/postmortems.md)

- Cold outreach goes out one at a time, only to verified emails of real, named people. PISCO sends
  are `email_status='verified'` + `only_verified:true`; never loosen. Never mass-rewrite `tier`.
- Email format: greeting on its own line, one sentence per paragraph, no em dashes, rotating subject
  lines, signature `Thank you, / Jordan / Director of Business Development / placewellinternational.net`.
- Anything only Jordan can do → `agent_tasks` assigned `jordan`. If it is not on the board, it does
  not exist. Before adding to his queue, try to just do it.
- Gate before the model, never after (`hermes-responder` shape). `claude-responder` runs every 15
  min, keeps its idempotency gate, and gets no prompt caching. Do not "optimise" these again.
- Alerts are fingerprinted (`md5` over stable ids only), never throttled; re-alert only when the
  fingerprint changes, plus a 7-day repeat. Never put Jordan's own tasks in an agent-stall alert. An
  alert with no path to a human is not an alert: upsert ONE `jordan` task, refreshed in place.
- Check output, never status. A Routine that reports SUCCEEDED and changes nothing is the expensive
  kind of green. Verify the artefact (a row, a post, a moved date) and log the run.
- Railway destructive ops need Jordan + 2FA and are only *staged* over the API; read back metrics
  before calling anything deleted. Never touch `Hermes Agent`. `render-worker` and `nanobot` are
  broken-or-live, not disposable.
- One-shot `pg_cron` jobs are landmines: unschedule after they fire.
- Secrets: never in git (the repos are public) and not in Routine prompts; the home is `app_secrets`,
  referenced by key name. Rotation is Jordan's and is on the board (urgent since 2026-10-22).

## The decision queue (the operating model)

The system's throughput limit is Jordan's decisions/day, not agent capacity.
- **The only channel that reaches Jordan is a Claude chat he opens.** He does not read email or push
  (his words, 2026-10-22). Anything he must see goes into the pinned session
  `📌 Jordan's daily decisions`; a Routine whose only output is a notification produces nothing.
- The daily brief posts there at 08:52 Madrid: max 5 items, each YES / NO / KILL, item 1 always the
  revenue-direct task. He answers in shorthand ("1 yes, 2 kill, 3 later") and the board updates.
  (Routine `Jordan daily decisions → 📌 session`; the old email brief was dead for two days on a
  permission prompt nobody could answer, see postmortems.)
- **Revenue-direct first.** A task that is literally a booking, a customer, a deal or a proposal
  (`metadata.revenue_direct = true`, or the title says so) is item 1 every day until done and is
  exempt from the archive rule below.
- Queue hygiene: one task per sitting (merge batches); standing plays are docs, not queue items;
  date-bound tasks whose window passed get closed with `metadata.closed_reason`, not nagged; gated
  items get `priority=low` until the gate opens; a non-revenue task featured 3+ times unanswered is
  flagged, then archived in 7 days.
- Every task moved to done carries `outcome` (what changed in the world) and `evidence` (where to
  see it). No outcome, not done.

## Bring Jordan the frontier

A finding you have is a finding you surface, in that session, not when asked. The weekly scan is a
Routine (`AI capability scan`, Thursdays), never memory. The filter is revenue, not fashion: would it
change a real number in 30 days? Most weeks the answer is "nothing qualified" (see `EVALS.md`). Never
sell him anxiety: his bottlenecks are human (studios that do not answer, contracts unsigned, a queue
he has not cleared). Say that plainly instead of recommending a vector database.

## Operating rules — Jordan × Claude (binding in every session)

- UNSURE beats plausible: never state a fact, name or email you have not verified.
- Creative work → 3 versions (safe / bold / weird), one-line tradeoff each.
- Outward-facing work → premortem: top 3 failure modes, fix, then proceed.
- Formats come from examples: ask for one example rather than accepting a description.
- Big deliverables end with "Accept when: …".
- Context rot: if a session mixes lanes or grows long, hand Jordan a 3-line handoff.

## Session protocol (binding, every session; design and sources in docs/claude-operating-system.md)

1. **Route.** One lane per session. Wrong lane → handoff, don't mix.
2. **Brief.** Open every deliverable with three lines, Goal / Material used / Done-when, with
   [UNSURE] gaps marked. Missing information → **draft anyway**, then ask at most 3 questions. Never
   reply with questions only (tested 2026-10-12: questions-only blocked the draft).
3. **Plan first** for anything non-trivial: approach in 3 bullets, then do.
4. **Verify with a fresh critic.** Proposals, outreach and client material get a subagent or
   fresh-context review as the recipient before Jordan sees them. The writer never grades itself.
5. **Close the loop.** Jordan-only steps → `agent_tasks`. A correction Jordan makes twice → propose
   it as a one-line rule for this file or the lane file, in the same session.
6. **Two failed corrections → restart** with a better brief, not a third patch.

Repeated workflows are skills, not memory: `call-to-followup` (Placewell; voice in its
`voice-examples.md`), `concierge-booking` (Massage Club), `weekly-retro` (all lanes, Fridays: turns
corrections into rules and writes the `lane_metrics` rows).
