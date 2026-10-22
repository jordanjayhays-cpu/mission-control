# Lovable projects — the full registry (verified 2026-09-15)

All 15 live in workspace `4yaletByfIWogrs8SgdG` ("Jordan's Lovable"). **Check this table before
editing any Lovable app.** A session once pushed a neurotech data build into the Agent Command Center
because the map listed only two projects and the real target was missing (see `docs/postmortems.md`).

| Live URL | Display name | Project id | Last edited |
|---|---|---|---|
| brain-vista-hub.lovable.app | Neuro Dashboard Live — **the neurotech tracker** | `621d7a65-d180-49a3-919f-cead001b15d8` | 2026-09-15 |
| neurodash-agent-dashboard.lovable.app | Agent Command Center | `d9cccd2d-c8f9-408f-91da-5ff739da5efd` | 2026-09-03 |
| sauchematch.lovable.app | Sauce Match | `dc87991c-5132-40e3-a8b0-3626e286f082` | 2026-09-11 |
| massage-madrid-magic.lovable.app | Your Massage Pass (preview; production is book.massageclub.io on Vercel) | `13ab3b1d-1034-4ac7-b40c-8e51807e553c` | 2026-09-08 |
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
Agent Command Center) takes free git pushes to its **main** branch; a feature branch is invisible to
Lovable until merged. A project with no repo (brain-vista-hub) can only be changed by a Lovable
message, and that spends credits. Confirm which before promising an update.

**Neurotech data lives in Supabase.** `neurotech_entries` on `neurodashboards` (kind / sort_order /
data jsonb, 233 rows, public read, writes denied to anon). brain-vista-hub fetches it at runtime, so
refreshing the data is a free SQL update.
