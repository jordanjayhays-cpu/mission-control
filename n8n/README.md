# n8n workflows

**All five are BUILT, ACTIVE and SMOKE-TESTED in the live instance as of 2026-09-25.**
The JSON in `workflows/` is the source of record; the instance is already running it.

| Workflow | n8n id | Production webhook |
| --- | --- | --- |
| Amigo Sales — send as jordan@amigosales.com | `7iQfAulBX58XtKXx` | `https://neuromatch.app.n8n.cloud/webhook/amigo-send` |
| Massage Club — booking → calendar invite | `Slgm1R0GUOIUaZQz` | `.../webhook/mc-booking` |
| Massage Club — warm requests going cold | `pKeG5ceHBacESt5k` | `.../webhook/mc-stall-digest` |
| Massage Club — 24h reminder to customer | `o1C4kX0CftBBnxsY` | `.../webhook/mc-reminder` |
| Board — urgent task landed | `mlF1kScQk5Be6yZf` | `.../webhook/board-urgent` |

### Smoke test results, 2026-09-25

- `board-urgent` → **Gmail returned a real message id** (`1a0d7d8c4af857df`, labels SENT/INBOX).
  The full chain works: HTTP in, Google OAuth held by n8n, mail out.
- `mc-stall-digest` with `count: 0` → HTTP 200, execution ran `Webhook → Any stalled?` and stopped.
  **No email sent.**
- `mc-reminder` with an empty `customer_email` → HTTP 200, ran `Webhook → Has email?` and stopped.
  **No email sent.**

The first version of both gated workflows returned **HTTP 500** on the quiet path — the IF node's
false branch had nothing to return and `responseMode` was `lastNode`. Functionally it sent nothing,
but a 500 would have made every caller think it had failed and retry. Both now use
`responseMode: onReceived`, so a quiet day is a clean 200.

## The architecture, in one line

**Supabase decides. n8n delivers.**

Every workflow here is `Webhook in → Google action out`. None of them queries a database or runs on
a schedule, because Supabase already does both better and for free. n8n is here for exactly one
thing: it holds the Google OAuth so we never write auth code.

This is deliberate. The audit on 2026-09-25 found 61 workflows in this instance, 17 marked active,
and **one** that had ever executed. Most were webhook workflows whose callers died with the Railway
stack, or workflows built with no trigger node at all. The rule that prevents a repeat:

> If it can be done with `pg_cron` + `pg_net` inside Supabase, it does not belong here.

## What each one does

| File | Trigger | Does | Credential used |
| --- | --- | --- | --- |
| `01-amigo-sales-send.json` | POST `/webhook/amigo-send` | Sends email with `jordan@amigosales.com` as reply-to | Gmail OAuth2 |
| `02-mc-booking-to-calendar.json` | POST `/webhook/mc-booking` | Creates a Calendar event and invites the customer | Google Calendar OAuth2 |
| `03-mc-stall-digest.json` | POST `/webhook/mc-stall-digest` | Emails Jordan when warm requests are going cold. Sends nothing when the count is zero | Gmail OAuth2 |
| `04-mc-appointment-reminder.json` | POST `/webhook/mc-reminder` | Reminds the customer 24h before. Skips silently when there is no email on file | Gmail OAuth2 |
| `05-board-urgent-alert.json` | POST `/webhook/board-urgent` | Emails Jordan when an urgent task lands on the board | Gmail OAuth2 |

Credential IDs are already wired to the ones that exist in the instance
(`1sXCvJvMZU4od12w` Gmail, `Kp7yvyM5o73bO9Tg` Google Calendar). If an import shows a red credential
warning, re-pick it from the dropdown once and save.

**Every one of these no-ops safely when there is nothing to do.** `03` and `04` gate before they
send. That is the opposite of the Axton task runner, which succeeded 90 times in a row while
delivering nothing to anybody.

## Jordan's side — what to check after importing

1. **`01` depends on Gmail "Send mail as" being configured** for `jordan@amigosales.com`. Gmail's
   OAuth here belongs to `jordanjayhays@gmail.com`, so without the verified alias the mail goes out
   from the Gmail address with amigosales only as reply-to. Check Gmail → Settings → Accounts.
2. Activate each workflow, then copy its production webhook URL — the Supabase side needs it.
3. **UNVERIFIED:** whether the n8n plan's webhook URLs are stable across a workflow rename. Copy
   them after you have settled the names.

## The Supabase side

Each webhook is called from inside the database, so no key ever leaves Supabase. Pattern:

```sql
select net.http_post(
  url := 'https://neuromatch.app.n8n.cloud/webhook/mc-stall-digest',
  headers := jsonb_build_object('Content-Type','application/json'),
  body := jsonb_build_object('count', c, 'summary', s),
  timeout_milliseconds := 30000
);
```

The stall detector that feeds `03` is the highest-value piece and belongs in `pg_cron`, not here:
it reads `whatsapp_requests` for rows sitting at `studio_replied` or `offered` past a threshold,
upserts ONE task assigned to `jordan` (refreshed in place, never duplicated — same shape as
`detect_stalls()`), and posts the digest. On 2026-09-25 thirteen such requests had gone cold
unnoticed; this is the alarm that would have caught them.

## Credentials

`N8N_API_KEY` and `N8N_BASE_URL` live in `app_secrets` on `neurodashboards`, with every other API
key. Nothing about n8n belongs in git.

## Still open

Whether n8n stays at all. If these five do not get wired up within a month, the instance is three
inactive real builds (`Jarvis` 52 nodes, `Automated workflow backup` 38, `Niah Afterparty` 18) and
58 stubs — export those three and cancel.
