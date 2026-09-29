# n8n self-hosted on DigitalOcean

Adapted from **`Jorge-AI700/n8n-infra`** ("Verus n8n"), which targets a Hetzner VPS with WireGuard.
**Nothing in it is Hetzner-specific** — it is plain Docker Compose, so a DigitalOcean droplet works
fine. Five things in the original will not start on a fresh droplet; all five are fixed here and
marked `CHANGED` in `docker-compose.yml` with the reason.

`docker compose config` validates clean.

## Why bother: cancel the n8n Cloud subscription

**This is the only good reason to do it.** Self-hosting adds no capability you do not already have on
n8n Cloud. What it does is move the same capability onto infrastructure you are already paying
DigitalOcean for, so the n8n Cloud subscription can stop.

It does not add capability and it does make you the sysadmin. Single instance, no failover, backups
you own. Worth it if DigitalOcean is otherwise idle; not worth it otherwise.

## The five things that would have broken

| # | Original | Why it fails on a droplet | Fixed here |
| --- | --- | --- | --- |
| 1 | Port binding `10.8.0.1:5679:5678` | `10.8.0.1` is a **WireGuard** address. No wg0 interface on a fresh droplet, so Docker cannot bind it and **the container refuses to start.** The most likely reason a copy-paste fails | Loopback binding only |
| 2 | `image: n8n-runners-custom:2.30.5` | Its own README admits this is a **local image, not in any registry.** `docker compose up` cannot pull it | Task-runners removed, `N8N_RUNNERS_MODE: internal` |
| 3 | `N8N_HOST` / `WEBHOOK_URL` / `N8N_EDITOR_BASE_URL` hardcoded to `n8n.wizdom-ai.com` | Every webhook URL n8n generates would point at someone else's host | Driven by `${N8N_HOST}` from `.env` |
| 4 | `./ffmpeg` and `./ffprobe` read-only bind mounts | Gitignored binaries absent from a fresh clone. Docker silently creates **directories** in their place and anything expecting an executable fails confusingly | Removed. Add back with real binaries only if you need media processing |
| 5 | Redis service running | `EXECUTIONS_MODE: regular` and every `QUEUE_BULL_REDIS_*` var commented out — **nothing connects to it.** Dead weight costing RAM | Removed |

## Droplet sizing

n8n is a Node application and Postgres 16 sits beside it. With Redis and task-runners gone you are
running two containers.

**Take a 2 GB droplet as the floor and 4 GB to be comfortable.** 1 GB will boot and then die under a
workflow that handles any real payload. **Check your own DigitalOcean account for current pricing —
I did not verify it, and you may already have a droplet running that would do.**

## Deploy

```bash
# on the droplet, as root or a sudo user
apt update && apt install -y docker.io docker-compose-v2
git clone <this repo> n8n && cd n8n/n8n/selfhost
cp .env.example .env
# fill POSTGRES_PASSWORD, N8N_HOST, N8N_ENCRYPTION_KEY
docker compose up -d
docker compose logs -f n8n     # watch it come up
```

n8n listens on `127.0.0.1:5678` only. **It is not reachable from the internet yet, which is correct.**

## Getting HTTPS in front of it

Two routes. The original uses the first.

**Cloudflare Tunnel (recommended, no open ports).** `cloudflared` runs on the droplet and dials out,
so the droplet needs no inbound firewall rule at all. Point the tunnel at `127.0.0.1:5678`. The
original's `~/.cloudflared/config.yml` restricts public paths to `/webhook/*` and `/healthz` and
returns 403 for everything else — **copy that.** It keeps the editor off the public internet, which is
the single most important security decision here.

**Caddy or nginx with Let's Encrypt.** Needs 80/443 open and a DNS A record at the droplet IP. Simpler
to reason about, larger attack surface, and you must protect the editor yourself.

## The migration gotcha — read this before you move anything

**`N8N_ENCRYPTION_KEY` is how n8n encrypts stored credentials.** Your n8n Cloud instance has its own
key that you cannot extract.

**So every credential has to be created again on the new instance.** That means re-authorising:

- Gmail OAuth (`1sXCvJvMZU4od12w`)
- Google Calendar OAuth (`Kp7yvyM5o73bO9Tg` and `XN9io426G3lFomJN`)
- Google Docs, Google Slides, Google Tasks
- GitHub OAuth, OpenAI, OpenRouter, Telegram

Workflow JSON exports and imports cleanly. **Credentials do not travel.** Budget an hour of clicking
through OAuth consent screens, and expect at least one to need a fresh app registration.

Set `N8N_ENCRYPTION_KEY` once, before first boot, and never change it.

## What else moves, and what breaks

**Export first — these are the only real assets on n8n Cloud:**

| Workflow | Nodes |
| --- | --- |
| Jarvis: productivity AI agent | 52 |
| Automated workflow backup | 38 |
| Niah Afterparty — South Summit Matchmaking | 18 |

Plus the five built on 2026-09-25 (Amigo Sales send, MC booking→calendar, MC stall digest, MC
reminder, board-urgent). Everything else in the instance is a 2–6 node stub or a duplicate.

**Webhook URLs change.** Every one moves from `neuromatch.app.n8n.cloud` to your new host. One
reference in the database has to be updated:

```sql
-- in mc_cold_requests_digest() on jglftdstrowwckwqmpue
-- 'https://neuromatch.app.n8n.cloud/webhook/mc-stall-digest'
-- becomes  'https://<N8N_HOST>/webhook/mc-stall-digest'
```

**`N8N_API_KEY` in `app_secrets` will stop working** — it is scoped to the Cloud instance. Generate a
new API key on the self-hosted instance and update `N8N_API_KEY` and `N8N_BASE_URL`.

## Backups — do not skip this

The original ships `backup.sh`: weekly Postgres dump plus `n8n_data`, retention 4, **stored locally on
the same droplet.** A local-only backup does not survive the droplet. Send it to DigitalOcean Spaces
or anywhere off the box. Droplet snapshots are a fine second layer but are not a substitute for a
database dump you can actually restore from.

## Order of work

1. **Decide whether you will use n8n at all.** The five workflows built on 25 Sept are the test. If
   nothing calls them within a month, the answer is cancel, not migrate.
2. Export the three real builds from Cloud regardless. **Do this even if you cancel** — they are the
   only assets in there and they are currently inactive and unbacked-up.
3. Only then stand up the droplet, re-authorise credentials, and move the webhook references.
4. Cancel n8n Cloud once the self-hosted instance has run a real webhook end to end.

**Step 2 is worth doing this week whatever you decide about the rest.**

## Accept when

The three real workflows exist as files outside n8n Cloud, and either the subscription is cancelled or
a self-hosted instance has served one real webhook.
