# Projects Overview — the full picture (2026-09-06)

**Conclusion first.** You are running ~20 projects with one operator and ~2 decisions/day of throughput.
The agents, DBs, and docs are far ahead of contact with reality: 893 Niah leads and zero sends, Puhunan
fully documented and zero loans, 75 PISCO emails sent and no reply column to know if any worked. The way
to "get them done" is not more building. It is: **3 focus lanes, a hard park on the rest, one measurable
"done" per lane, and a board cut from 62 Jordan-tasks to ~15.** Two security fixes come before any of it.

Sources: all 14 repos (audited today), `agent_tasks` + `agency_clients` + `pisco_prospects` on `neurodashboards`,
the Massage Club DB, and the 24 enabled Routines. Anything not verified is marked UNSURE.

---

## 0. Do these first (security, ~30 min, only you)

| # | What | Why |
|---|---|---|
| 1 | **Rotate every credential in `hermes-memory` and `007-Axton`**: OpenRouter keys (2), a GitHub PAT, Telegram bot tokens (2), Notion token, Railway gateway token, plus `.env`, `.git-credentials`, `telegram.token` which are git-tracked in 007-Axton | They are in GitHub history. Deleting the files does not help; rotation is the only fix. Verified today by pattern-match, values not printed. |
| 2 | **Rotate the Resend API key** in `your-massage-pass` (`.env` is git-tracked and the key is a `VITE_*` var, so it ships in the client bundle) and move email sends server-side | Anyone viewing the live site's JS can send email as you. |
| 3 | Railway: click **Apply** on the 4 staged deletions + Hermes scale-to-0 (needs your 2FA) | Hermes is still running and billing until you click. Already on the board as urgent since 08-22. |

---

## 1. The portfolio, one line each

Status key: **LIVE** = something real is running · **BUILT** = exists, not in use · **IDEA** = docs only · **DEAD** = retire.

### Own products
| Project | Repo / home | Status | Reality check (verified) | Single "done" |
|---|---|---|---|---|
| **Massage Club** | `your-massage-pass`, Supabase `jglftdstrowwckwqmpue` | LIVE | 1 onboarded studio. 39 bookings ever: 20 confirmed, 16 cancelled, 3 completed. 7 bookings in last 30 days, **0 in last 7**. Last confirmed booking 07-19. Live URL disagrees across 3 docs (UNSURE which is canonical). | 5 paying (non-reimbursed) sessions in a month |
| **Philosophical King** | `pk-render-worker`, `jordan-projects/design-system`, 5 Routines | BUILT | Design system is finished and good. Render worker on Railway never ran since 08-10 (cause unrecorded, UNSURE). Meta API token + 3 GitHub secrets still pending. No GitHub repo for PK yet. Daily pipeline Routine shows PENDING today. | One piece auto-posted end-to-end with no manual step |
| **Niah** | `niah-dashboard`, app in Niahconnect org, `niah_prospects` 893 rows | BUILT | 6 personalised drafts ready (IFEMA, CREA, CloserStill) and no send recorded. South Summit June 2026 has passed; that proposal is closed. Password-reset in review since 07-17. 7 one-shot event Routines queued Sep–Oct. | 1 organiser trials the matchmaker at a live event |
| **Vaya News / Vaya Manila** | `Vaya` | BUILT (today) | Site written. Needs GitHub Pages toggle + 5 DNS records at Squarespace. Manila pilot shoot on board. | vayanews.com resolves over HTTPS |
| **Agent Command Center** | `neurodash-agent-dashboard`, Lovable `d9cccd2d` | LIVE | Internal tool, works. Last 15 commits all grind the same Niah password-reset flow. neurodashboards.com domain unverified. | Domain live; stop the password-reset loop |
| **Sauce Match** | `sauchematch`, sauchematch.lovable.app | BUILT | Demo store, checkout charges nothing, catalog is static. Stalled 08-16 in a design revert loop. Its analytics write into the **agent board DB** with a hardcoded key. | Decide real store or demo. Recommend: park. |
| **Family Business Flourish** | `clever-family-coach` | BUILT | 2-page consulting site, dormant since 08-10. Per `GROUP-STRUCTURE.md` this IS the BD agency under its old name. Intake form writes to Supabase with no notification (UNSURE if any leads are sitting unread). | Fold into the Kinsol site; check the intake table once |
| **Neurotech tracker** | `neurotech-dashboard` | PARKED | Correctly parked in writing. Leave alone. | n/a |

### Business lines (you are the product)
| Project | Home | Status | Reality check | Single "done" |
|---|---|---|---|---|
| **Placewell / PISCO** | `pisco_prospects` (385 rows, 161 verified), pisco-writer/sender | LIVE | 75 sent since 08-18, 18 followed up, **0 replies recorded, and there is no reply field**. 22 ready_to_send, 39 to_research. Board says fee/timeline/guarantee terms and first SKU are still undefined. | 1 employer on a call about a placement |
| **BD agency (Kinsol)** | `jordan-projects/bd-agency`, `profile-stack`, `agency_clients` (13 rows) | IDEA | Name not locked (the only structural decision left). 2 pricing blanks in the rep agreement. COMARE at intake (rep identified, waiting on their pricing). Nomin at intake. 7 clients at "idea" with one-sentence info missing from you. Kinsol LLC: EIN, bank, Meta verify pending. | COMARE signs the rep-test agreement |
| **Amigo Sales** | board only | UNSURE | Board: re-engage 3 dormant clients, land 1 BD retainer. I found no repo or DB for it. | 1 signed retainer |
| **Puhunan** | `jordan-projects/puhunan`, `puhunan_deals` (0 rows) | IDEA | Agreement, borrower screen, landing, meeting sheet all done. Zero loans. | 1 loan made, 1 repayment cycle observed |
| **Manila lane** (Manila TV, food tour, Lettuce Wraps, New Bali) | board + `jordan-projects/new-bali` | IDEA | Four separate board items that are all the same trip. New Bali README already says the one action is "shoot the video". | Merge into one Manila-trip plan with one shoot |
| **Nomin (Mongolia import)** | `jordan-projects/nomin` | IDEA | Research complete; pivoted to household staples. Waiting on your brand pick. | 1 brand mandate + 1 importer sample request |
| **AI Caller** | `services/ai-phone-caller` in neurodash repo | BLOCKED | Twilio account never created. Blocked since 08-06. | Park until a lane needs calls |
| **"Snyk for vibe coders"** | board idea 09-03 | IDEA | No work done. | Park |

### Infrastructure and dead weight
| Repo | Verdict |
|---|---|
| `mission-control` | The brain. `CLAUDE.md` is excellent. `MAP.md` (05-11) and `PROJECT-INDEX.md` (05-17) contradict it; `hermes-factory-status.md` shows all-green for a system dead since May. Collapse the three maps into one, delete the factory status. |
| `jordan-projects` | Deliverables drop, working as designed. `massage-club/` and `neurodashboard/` are empty stubs. |
| `007-Axton` | Decommissioned. Extract PISCO/, PROJECTS/, contacts/, ORION, LESSONS into mission-control; then archive. `node_modules` is committed. |
| `hermes-memory` | Dead since 05-07 and full of plaintext secrets. Rotate, extract SOUL/playbook/user-prefs into mission-control, archive. |
| `nanobot` | Orphan fork. The live Railway nanobot deploys from `codestorm-official/nanobot`, not this. Extract `brains/006/`, archive. |
| `open-swarm-outputs` | Zero commits ever. Delete. |
| `pk-render-worker` | **Keep and fix.** Cleanest code in the estate and the only fully automated production pipeline you own. |

---

## 2. Recommendation: three focus lanes, everything else parked

Criteria: closest to money, uses your actual role (Director of BD), and has momentum today.

**Focus (your time goes here)**
1. **Placewell + Kinsol** (one lane, one name decision). It is your job, it has 161 verified prospects and a working sender, and it has the one structural decision that unblocks four documents.
2. **Massage Club.** Live, has real bookings, has a WhatsApp bot in progress. Concierge model is right. Needs paying sessions, not features.
3. **Philosophical King.** Once the Meta token + render worker are fixed, this runs with near-zero Jordan time. That is the definition of a good lane for you.

**Keep warm (agents run it, you spend under 30 min/week)**
- **Niah**: send the 6 drafts that already exist, let the event Routines run, hard go/no-go on **Oct 15**.
- **Vaya**: 10-minute go-live now; the pilot shoot is a Manila-trip item.
- **Puhunan**: one loan, when you are next in the Philippines or by video with family.

**Park (close the board items, keep the docs)**
Sauce Match, Family Business Flourish site (folded into Kinsol), Nomin, New Bali, AI Caller, Neurotech tracker, Lettuce Wraps, Manila food tour, "Snyk for vibe coders". Parking is reversible; a 62-item queue is not survivable.

---

## 3. How to get them done: the next 4 weeks

Each week has one decision day. Everything else is agent work that already has a home.

**Week 1 (Sep 7–13): unblock**
- Security rotations (section 0). Railway Apply.
- **Decide the agency name** and fill the two pricing blanks. Agents then finish the landing page, agreement, recruiting pitch, and COMARE rep test.
- Vaya go-live (Pages + DNS).
- PK: Meta token + 3 GitHub secrets. Agents then debug the render worker.
- MBA: ISA midterm Sep 17, China in the World deadlines, Power & Influence upload. These are real and dated; they outrank everything above on those days.

**Week 2 (Sep 14–20): first contact**
- Placewell: name ONE employer + ONE role; agents draft; you send ONE warm intro.
- Massage Club: test signup end-to-end; book 3 concierge massages for friends; Search Console (2 min).
- Niah: send the 6 drafts, one per day, verified emails only.
- Agents add a `reply_status` column to `pisco_prospects` and start logging replies. Without it the 75 sends are unmeasurable.

**Week 3 (Sep 21–27): measure**
- Placewell: first employer call, or the SKU is wrong. Decide.
- Massage Club: 5 paying sessions target check; WhatsApp bot live if Meta appeal cleared.
- PK: first fully automated post, or the worker gets one more week then parks.
- Friday ladder check-in ends Sep 26.

**Week 4 (Sep 28–Oct 4): cut**
- Anything with no signal after 3 weeks goes to Park.
- Board cut to ≤15 items. Archive the rest with a one-line reason each (agents can do this once you say yes).

---

## 4. The board is the real problem

| Assigned | Status | Count |
|---|---|---|
| jordan | queued | 58 |
| jordan | blocked | 3 |
| jordan | in_progress | 1 |
| claude | review | 5 |
| codex | review | 1 |

53 tasks were closed in the last 30 days, so the system does move. But 62 open items for one person means the
09:00 brief is choosing from a pile, and items get "featured 3+ times unanswered". Recommended cut, agents execute
on your YES:
- Merge the 4 Manila items into one. Merge the 5 Placewell items into 2 (terms; first employer). Merge the 4 Amigo items into 1.
- Close the 3 blocked AI Caller items and the 2 LinkedIn-draft items (25+ days overdue).
- Move all Park-tier items to `archived` with reason "parked 2026-09-06".
- Keep MBA deadlines as-is; they have dates.

## 5. Routines (24 enabled)

Roughly 10 sessions/day. The 5 Massage Club Routines and 4 PK Routines are the only ones producing output
that reaches a customer. The 7 Niah one-shots are fine (they self-disable). "Watch Alex + Anita WhatsApp
replies" fires hourly; UNSURE whether it still needs to. Nothing here needs cutting this week; re-audit at the
Oct 15 Niah gate.

---

## BOARD: (only you)
- BOARD: Rotate leaked credentials (hermes-memory, 007-Axton, Massage Club Resend key). Added to the board today as urgent.
- BOARD: Lock the agency name + 2 pricing blanks. (existing tasks)
- BOARD: Railway 2FA Apply. (existing, urgent since 08-22)
- BOARD: Vaya Pages + DNS. (existing, high)
- BOARD: PK Meta token + GitHub secrets. (existing, high)
- BOARD: Say YES/NO to the board cut in section 4.

## Verify before acting (discernment)
- Massage Club canonical URL: three docs disagree. Confirm before any DNS work.
- Whether the Family Business Flourish intake table has unread leads.
- PK render-worker failure cause is not recorded anywhere; the "small fix" claim is an inference.
- Amigo Sales: I found no repo or DB, only board items.

**Accept when:** every project above has a status you agree with, one "done" line, and a tier (Focus / Keep warm / Park); the two security rotations are done; and the board is under 15 open Jordan items by Oct 4.

---

## Appendix: every project ever proposed (exhaustive, 2026-09-06)

Sources: 14 repos, `agent_tasks`, `agency_clients`, 007-Axton `PROJECTS/`, `downloads/`, `parking-lot.md`,
`ideas-queue.md`, the May Trello export, mission-control `overnight-builds/` and `neuromatch/`.
Note: **Amigo Sales = the BD agency = Family Business Flourish = Kinsol.** One business, four names.

### A. Own products (software)
1. Massage Club / Massage Pass (Madrid studio marketplace; earlier: subscription "Massage Pass") — LIVE
2. Philosophical King / PK Music (philosophy music + Shorts pipeline + design system) — BUILT
3. Niah (B2B event matchmaking, Madrid; South Summit proposal) — BUILT
4. Vaya News + Vaya Manila (site, Manila films) — BUILT today
5. Agent Command Center / Neurodash (internal dashboard) — LIVE
6. Sauce Match (hot-sauce quiz store) — BUILT, stalled
7. Neurotech Funding Tracker (page) — PARKED
8. AI Caller (Twilio + OpenAI realtime phone agent) — BLOCKED
9. Lead scraper → call queue pipeline (Firecrawl + AI Caller) — BLOCKED
10. City Match Quiz (relocation quiz, 3 versions) — BUILT, orphan
11. Harvard case simulation + quiz (MBA prep tool) — BUILT, orphan
12. LeadPulse (warm outreach tracker) — overnight build
13. CommitmentClock — overnight build
14. "Jordan's Business Dashboard" analytics page — overnight build
15. Meaning Crisis Toolkit (personal page) — overnight build
16. The Passage (UNSURE what it is; a page exists in two places)
17. Daily insight dashboard / Idea scorer / Trend alert bot (INTEL ideas-queue) — IDEA only
18. "Snyk for vibe coders" (security for vibe-coded apps) — IDEA, 09-03

### B. Business lines (services, you are the product)
19. Placewell / PISCO (EU employer staffing outreach; hotel pitch; Bulgaria bulk hiring; Lithuania logistics; EU sales agents) — LIVE
20. BD / representation agency = Amigo Sales = Family Business Flourish = Kinsol (appointed reps + HeyReach) — IDEA→intake
21. COMARE (Mexico B2B maintenance client; gyms ICP; pharmacy outreach; Farmacias Similares list) — intake
22. Nomin / Mongolia import (US staples sourcing agent) — research done
23. Techanzo, Turkey project, Mining project, Box/packaging maker, Textile/uniforms, Pharmacy project, Colombian hardware — 7 agency clients at "idea", each missing one sentence from you
24. Puhunan (Philippines micro-investment / incubator) — IDEA, docs complete
25. New Bali (Philippines nomad coliving, Bantayan; content-first) — IDEA
26. Manila TV / Manila pilot (on-camera hosts, shoot) — IDEA
27. Manila food tour (pick local host) — IDEA
28. Lettuce Wraps (Manila food test; validate before hiring cook) — IDEA
29. Property / coliving (the "third thing" in GROUP-STRUCTURE) — later
30. Neurotech consulting / Neuro US Expansion partner program (bridge for non-US neuro cos entering US) — research + outreach done, no replies tracked
31. Neurotech content channel (10 scripted video titles, ego-death speech, trailer) — scripts only
32. Dealsmap 2.0 / ProfileStack (LinkedIn "profile partner" rental; Upwork post; $20/month) — kit written
33. Google Maps influencer aggregation (creators → local business marketing) — 100-PROJECTS category only
34. Dental no-show prevention, LatAm (AI reminders) — one doc
35. The Ego Death Pod / Consciousness Pod (Telegram community + launch kit) — kit written, Trello board existed
36. Skills marketplace, AI tool curation newsletter, problem-first startup course — INTEL ideas-queue, never started
37. Podcast, crypto thing (Jake), hire a VA, video editing agency — parking-lot.md, parked Mar 2026
38. ORION Lighting FCF model — MBA coursework, not a venture

### C. Infrastructure / agent projects
39. Hermes / OpenClaw (007-Axton) + Hermes Factory self-improvement — DECOMMISSIONED
40. Nanobot (Agent 006, Telegram) — orphan fork; live service deploys elsewhere
41. hermes-memory (memory sync) — dead
42. open-swarm-outputs / multi-agent swarm — never started
43. pk-render-worker (Whisper + ffmpeg Shorts renderer) — keep, fix
44. n8n Cloud Axton workflows (~16 "active", inert) — clean up someday
45. Postiz social scheduler — 2 board items, key rotation cancelled
46. Railway email cron (Gmail sender) — in mission-control
47. Chinese AI models research (Kimi/DeepSeek/Qwen) to cut cost — in review
48. "100 Projects" playbook, 100x experiments, evolver — Hermes-era exhaust, no owner

### D. Personal / MBA (dated, real)
49. IE IMBA: ISA Data Fluency midterm Sep 17, China in the World deadlines, Power & Influence canvas
50. Auto class notes Granola → Notion (Routine)
51. 30-day ladder + Friday check-in (ends Sep 26)
52. CPA cleanup (~$10k LLC income, FEIE), Kinsol LLC EIN/bank
53. Baan Bua / Achicka note (personal, urgent on board)
