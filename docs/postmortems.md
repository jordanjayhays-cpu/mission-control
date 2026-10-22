# Post-mortems and the stories behind the hard rules

Moved out of `CLAUDE.md` on 2026-10-22 so the master file stays short enough to be obeyed. Each rule in
`CLAUDE.md` → "Hard rules" has its story here. Read this when you are about to change a Routine, a
cron, an alert, or infrastructure, not at the start of every session.

## 2026-10-22 — Usage audit findings (graded report in Jordan's reports folder)

- **The 09:00 decision brief was silently not delivering.** On 2026-10-21 and 2026-10-22 its run went
  to "requires action" within ~13 seconds with zero tokens used: the session never ran the model. The
  Routine carried five connectors including Railway, which needed re-authorisation; the Routines with
  the same model and no Railway connector ran normally. Recreated the brief with Supabase only, in
  Madrid time, with the revenue-direct and auto-close rules; the old Routine is disabled, not
  deleted, so its run history stays. Lesson: a Routine's `last_run` says delivered, not completed.
  Check `get_session` status_bucket and `context_usage.used_tokens`, not the run status.
  Follow-up the same day: the replacement (a Routine waking the pinned session `📌 Jordan's daily
  decisions`) has the Supabase connector but hit the SAME prompt class on its first SQL
  (`decision_reason_rule_source: mcpServerPolicy`), even in auto mode. The difference is that a
  bound session keeps Jordan's approval: he taps Approve once in that session and later wakes run
  without asking. A fresh-session Routine asks every morning and nobody is there. Rule: anything
  that must touch the database on a timer runs in a bound session Jordan has approved once, or
  over HTTP to an edge function. Also learned: `fire_trigger` on a bound Routine spawns a separate
  fresh session (origin `force_run_trigger`) with no connectors; test bound Routines with
  `send_message` into the bound session instead.
- **Jordan does not read the alerts.** His words, 2026-10-22: "I don't even look at those alerts." Every
  notification-only Routine (the email brief, the night shift's morning email, the daily Instagram
  follow prompt) was therefore producing nothing. Replaced by one pinned Claude session,
  `📌 Jordan's daily decisions`, that the brief Routine wakes each morning; the night shift and the
  follow prompt are paused. Rule: the channel is a chat he opens, never a notification.
- **The hourly WhatsApp watch was 24 of ~37 daily firings**, all into a 123-day-old session. Cut to
  seven business-hour slots (09:43 to 21:43 Madrid, every two hours) rather than killed, per the
  "reduce cadence before killing" rule. Unbinding it to a fresh session with a bookmark is the next
  step and needs its prompt rewritten without inline keys.
- **The Friday ladder check-in fired four weeks past its own end date** (its prompt said delete me
  after 26 Sep). Disabled. Every dated Routine gets a stop date on creation from now on.
- **The night shift "succeeds" in ~60 seconds** by filing four generic documents a night: Stoic
  captions for a music project, pitches with `[one-liner]` placeholders, a one-pager with the price
  blank, plus new board tasks. It is a source of review load, not revenue. Pause or redesign is
  Jordan's call (lane decision).
- **Board**: 302 tasks, 223 to Jordan, 130 cancelled, 57 done, 36 open; 13 warm Massage Club requests
  open since 5 Sep; "rotate leaked credentials" at priority low since 6 Sep (raised to urgent). The
  evals layer designed 7 Sep had one row: added `outcome`/`evidence` columns and made `weekly-retro`
  write `lane_metrics`.
- **Instruction corpus**: 1,080 lines across 7 files, a stale 007-Axton map and a May index.
  Pruned `CLAUDE.md` to ~115 lines; deleted `MAP.md` and `PROJECT-INDEX.md`; registry and stories
  moved here and to `docs/lovable-registry.md`.

## 2026-09-15 — The Lovable registry exists because of a misfire

A session pushed a neurotech data build into the Agent Command Center because the map listed only two
Lovable projects and the real target was missing. The full registry is `docs/lovable-registry.md`.
Which projects sync from GitHub matters more than the id: a repo-synced project (Sauce Match, the
Agent Command Center) takes free git pushes to its **main** branch and a feature branch is invisible
to Lovable; a project with no repo (brain-vista-hub) can only be changed by a Lovable message, which
spends credits. Confirm which before promising an update.

Neurotech data now lives in Supabase: `neurotech_entries` on `neurodashboards` (kind / sort_order /
data jsonb, 233 rows, public read, writes denied to anon). brain-vista-hub fetches it at runtime, so
refreshing the data is a free SQL update, no Lovable credits.

## 2026-09-07 — "Bring Jordan the frontier"

Jordan's complaint, verbatim intent: *"Why do I have to come to you and say 'here's some AI to
research'? You should come to ME."* He was right, and the failure was not a capability limit: the gap
(no evals) was already written in PROJECTS-OVERVIEW.md that same morning and reported only when he
raised it. Hence the rule: a finding you have is a finding you surface, and the weekly scan is a
Routine (`AI capability scan`, Thursdays 08:00 Madrid), because memory does not survive a session and
a cron does. The scan files at most ONE YES/NO task titled `AI SCAN:`.

## 2026-08-22 — Cron and alerting (learned the hard way)

**Gate before the model, never after.** `hermes-responder` returns early when its inbox is empty and
only then calls the LLM. `claude-responder` v13 did the opposite and re-priced identical input every
15 min, up to 96x/day at ~21.5k tokens each. Fixed in v14 with an idempotency gate keyed on
`agent_state['claude-responder:last_seen'].entry_id`. It runs every 15 min; do not put it back to 5.

**Rate limits are not deduplication.** `detect_stalls()` had "one alert max per 2 hours", a throttle
that re-sent the same three blocked tasks 12x/day for weeks (245 rows archived). The fix is a
fingerprint: `md5()` over stable identity keys (task id + kind) only. Never let volatile text into the
hash. Re-alert only when the fingerprint changes, plus a 7-day `repeat_interval`.

**Never put Jordan's own tasks in an agent-stall alert.** His to-dos reach him via the 09:00 brief; the
stall alert is for agent work that is stuck. **An alert with no path to a human is not an alert.**
`detect_stalls()` upserts ONE `assigned_to='jordan'` task, refreshed in place, never duplicated.

**Prompt caching does not apply to claude-responder.** The cacheable prefix (~714 tok) is under the
1024-token minimum and the 15-min cron exceeds the 5-min default TTL. Enabling it would add a ~1.25x
write premium on 100% misses.

**Keep the reasoning window conversational.** MODE 3 excludes `agent='system'` and truncates bodies to
400 chars.

**Telemetry prunes itself.** `prune_telemetry()` (cron `prune-telemetry-daily`, 03:20) drops
`agent_heartbeats` >14 days and completed `agent_prompts` >30 days.

**`linkedin-poster-daily` was a zombie.** Fired daily for a month, posted nothing (both queued items
held for Jordan's approval). Unscheduled. Check output, never status.

## 2026-08-22 — Infra cleanup

**Railway destructive ops need 2FA and cannot be done over an API/MCP token.** `removeServiceTool`
reports "marked for removal" but the change is only staged. Always re-check `get-service-metrics`
afterwards. Only Jordan can Apply, from the dashboard, with 2FA. Never report a Railway deletion as
done without that read-back.

**Verify a service's config before deleting it, not just its metrics.** `render-worker`
(pk-render-pipeline) is the Whisper transcription worker, repo `pk-render-worker`, never ran since
2026-08-10: broken, not disposable. `nanobot` (abundant-radiance) has a live public domain, admin
creds and a persistent volume at `/data`, deploying from `codestorm-official/nanobot`, not Jordan's
fork. `Hermes Agent` is the live OpenClaw gateway (~5 GB RAM, active): never touch it.

**One-shot pg_cron jobs are landmines.** Cron has no concept of "once": six PISCO sends pinned to
`... 18 8 *` would have re-fired 2027-08-18. All unscheduled.

## 2026-08-22 — Where the tokens actually go: Routines, not cron

Each Routine firing spawns a whole Claude session. Edge-function crons are pennies by comparison.
Audit: 28 triggers ~16 sessions/day → 21 triggers ~10 sessions/day. `Neurodash PM + worker` cut from
8x/day to 2x/day (`0 7,15 * * *`). Reduce cadence before killing a useful agent. Measure value as
board movement per firing, not uptime. (By 2026-10-22 the count had drifted back to ~37 firings/day,
24 of them one hourly watch; see the top of this file.)

## 2026-08-22 — Outreach safety and retention

**PISCO sends are verified-only.** `pisco_daily_send()` and `pisco_daily_followup()` filter
`email_status = 'verified'` AND send `only_verified:true`. This protects the jordan@placewell.io sender
reputation (Google junks a domain whose spam rate passes 0.30%).

**Never mass-rewrite `tier` to force a pick.** The old code flattened the whole queue to tier 5 on
every send, destroying the priority ranking. Demote only the previous target (`AND tier = 0`).

## Notes that used to live in CLAUDE.md

- n8n Cloud: the ~16 "active" Axton workflows from May 4 never execute; their triggers point at the
  dead Railway stack. Inert; clean up in an n8n-lane session someday.
- The 30-day "1-to-10 Ladder" ran 2026-08-22 to 09-26 with a Friday check-in Routine. The Routine
  kept firing until 2026-10-22, when it was disabled. The ten drills live in the 09:00 brief's prompt.
