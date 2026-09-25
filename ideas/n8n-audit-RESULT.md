# n8n audit — result

Run 2026-09-25 against `https://neuromatch.app.n8n.cloud` via the public REST API.
Key and base URL now live in `app_secrets` on `neurodashboards` as `N8N_API_KEY` / `N8N_BASE_URL`,
alongside every other API key. Nothing about n8n is in git.

## The headline

**61 workflows exist. 17 are marked active. Exactly one has ever executed.**

| | |
| --- | --- |
| Total workflows | 61 |
| Marked active | 17 |
| Inactive | 44 |
| Workflows with any execution history | **1** |
| Executions returned (last ~30 days) | 90, all `success` |

## The one that runs

**`Axton Work Unit — Autonomous Task Runner`** (`YR9XOt3YLHTAZgH5`)

- Three nodes: `Every 6 Hours` schedule trigger → `Prepare Task` code node → **`Notify Jordan` Telegram**
- Last edited **26 April 2026**. Untouched for five months.
- Fires ~3x/day, every day since at least 26 August. 90 consecutive successes.

**It is a zombie in the exact shape the master map warns about:** green every single run, and its
only output is a Telegram message. Jordan's system moved to Claude and the 09:00 decision brief
months ago. The open question is one line: **does he read that Telegram channel?** If not, this has
sent ~90 unread messages in a month and should be switched off.

## The other 16 "active" workflows have never run

Two reasons, both structural:

1. **Webhook-triggered with nothing calling the webhook.** Query Hub (×4), Blackboard to Notion
   (×4), Massage Pass — New Partner Alert, Create Google Calendar Event v3, Idea Capture Bot.
   Their callers were on the dead Railway stack.
2. **No trigger node at all** — a workflow with no trigger can never fire. Axton — Daily Digest,
   Partner Lead Gen, Booking Dashboard, Churn Risk, Social Announcer, Content Pipeline. Each is two
   nodes and cannot run by construction.

## Duplication is most of the clutter

- `Axton — Supabase Query Hub` × 4 (plus `Query Hub Simple`, `Query Hub v3`)
- `Axton — Blackboard to Notion` × 4
- `Create Google Calendar Event` v1 / v2 / v3 / v4
- `Axton — Reddit Publisher` × 2, `Axton — Daily Digest` × 2, `Idea Capture Bot` × 2
- Telegram calendar bots × 5 under different names

## Worth keeping a note of

- `Jarvis: productivity AI agent` — **52 nodes**, off. The largest thing built here.
- `Automated workflow backup with intelligent change detection` — 38 nodes, off.
- `Niah Afterparty – South Summit Matchmaking` — 18 nodes, off. Real Niah work.

These three are the only substantial builds in the instance. Everything else is 2–6 nodes.

## Recommendation

1. **Answer one question:** do you read the Axton Telegram channel? No → deactivate the task runner
   and the instance goes fully silent.
2. **Deactivate the 16 active-but-never-executing workflows.** They cannot do anything, and open
   webhook endpoints sitting on an active workflow are surface for no benefit. Reversible in one
   click each, or in one API pass.
3. **Delete the duplicates** once 1 and 2 are settled. 61 → roughly 10 keeps the instance legible.
4. **Then decide whether to keep paying for n8n at all.** One Telegram ping every six hours is not
   worth a subscription. Jarvis and the Niah matchmaking flow are the only assets — export them
   before cancelling anything.

## Accept when

The instance has zero workflows that are active but cannot run, and a yes/no on whether n8n stays.

---

## ACTIONED 2026-09-25

**Jordan: "axton's telegram is ded."** The one live workflow was sending ~3 Telegram messages a day
into a channel nobody reads, and had been for at least 30 days.

**All 17 active workflows deactivated via the API.** Every call returned 200; a read-back confirms
**61 workflows, 0 active.** The instance is now silent.

Deactivated:

| Workflow | ID |
| --- | --- |
| Axton Work Unit — Autonomous Task Runner *(the only one that ran)* | `YR9XOt3YLHTAZgH5` |
| Idea Capture Bot (Telegram) | `24gRjyhfQFHQHusu` |
| Axton — Query Hub v3 | `8Ncc34N0XvTXnTXR` |
| Axton — Supabase Query Hub | `FcAGEm2gfPuRbYwc` |
| Axton — Supabase Query Hub | `yOxOGBytJjnUQvbd` |
| Axton — Query Hub Simple | `SKKLwLZUmZmFLT4y` |
| Axton — Blackboard to Notion | `QPxx0ADty4SCwIMX` |
| Axton — Blackboard to Notion | `UHND4T2NXRiVH34R` |
| Axton — Blackboard to Notion | `hVnJCn3YC28Q8nZy` |
| Axton — Daily Digest | `Son0qpyTolUbqEv3` |
| Axton — Content Pipeline | `Z9kOwFm6Fa9b3zOU` |
| Axton — Booking Dashboard | `dEI9XITRlt9hq2jH` |
| Axton — Social Announcer | `qpdwe8u4J1PAalPe` |
| Axton — Churn Risk | `tBPR5gEsdO6VqMTs` |
| Axton — Partner Lead Gen | `yuf6So8SlV8TgCCQ` |
| Massage Pass — New Partner Alert | `dMjvYUbrVIpdzSJy` |
| Create Google Calendar Event v3 | `icBE9flyvtKtsnOY` |

**Reversible.** `POST /api/v1/workflows/{id}/activate` with `X-N8N-API-KEY` turns any of them back on.
Nothing was deleted.

**Still to decide:** whether n8n stays at all. Export `Jarvis` (52 nodes),
`Automated workflow backup` (38) and `Niah Afterparty — South Summit Matchmaking` (18) before
cancelling — they are the only substantial builds in the instance.
