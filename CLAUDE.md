# Working notes for Claude sessions — Mission Control

Read this first; it saves every session from rediscovering the setup.

## This lane
- Jordan's master overview page. Content is DERIVED — the sources of truth are:
  - `agent_tasks` on Supabase `neurodashboards` (`dprdnrgjkzgfgtcsguuq`) — the only to-do list
  - his artifact gallery (Command Map, Operator's Manual, 1-to-10 Ladder)
  - the 09:00 decision brief (max 5 items, YES/NO/KILL)
- Never invent status here; read it from the board. If this page disagrees with the board,
  the board wins.

## Operator rules (binding)
- Ambiguous ask → ask up to 5 short questions before building. Never guess constraints.
- Never state a fact you haven't verified — write UNSURE and flag it.
- Creative work → 3 versions (safe/bold/weird), one-line tradeoffs.
- Big deliverables end with "Accept when: …".
- Minimal surgical edits; verify with a read-back before claiming success.
- Anything only Jordan can do → task on `agent_tasks` assigned `jordan` — his 09:00 brief reads it.
  If it is not on the board, it does not exist.

## Token discipline
One lane per session. Name columns + LIMIT in SQL. Bulk row analysis (>~20 rows) → the
`agent-worker` edge function on `neurodashboards`, not SELECTs into context.

The full system map lives in `niah-dashboard/CLAUDE.md`.
