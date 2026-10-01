# ACH #1 — Why has nothing reached "earning"?

Run 2026-10-01 against the 41-project registry. First use of the ACH frame.

**Why this question:** 22 projects built, 12 live, **0 earning.** Every recommendation this session
rested on one forward-built explanation that was never tested.

---

## Step 1 — Hypotheses

| | Hypothesis |
| --- | --- |
| **H1** | **Too many lanes.** Attention is split across 41 projects so none gets finished. *(the case asserted all session)* |
| **H2** | **No market.** The products do not have demand at the price offered. |
| **H3** | **The last mile is a human step Jordan does not take.** Everything is built up to the point where a person must send, call, or decide — and that step does not happen. |
| **H4** | **Nothing is measured.** Earning may be occurring and invisible, or failure is undetectable. |
| **H5** | **Capacity.** An MBA plus a BD job consume the throughput; building is the only affordable activity. |
| **H6** | **DECEPTION CHECK: the record is wrong.** "Built not earning" is partly an artifact of incomplete tracking rather than reality. |

H6 is mandatory under ACH and is **not** a courtesy here: the project list was found incomplete
twice on this same day.

## Step 2–4 — Evidence, scored, with non-diagnostic items deleted

### Deleted for zero diagnostic value

- **"0 projects at earning."** Fits H1, H2, H3, H4 and H5 equally — VC against all of them.
  **It is the headline statistic and it proves nothing about the cause.** Deleted.
- **"Jordan does an MBA and works in BD."** Fits H1 and H5 identically. Deleted as non-diagnostic.

That deletion matters: two of the three things leaned on hardest this session carry no analytic weight.

### The matrix

| Evidence | H1 lanes | H2 no market | H3 last mile | H4 unmeasured | H5 capacity | H6 record wrong |
| --- | --- | --- | --- | --- | --- | --- |
| **E1** Massage Club: 13 requests, **studio said yes**, never closed | C | **II** | **VC** | C | C | C |
| **E2** PISCO: 75 sent, 0 replies recorded, **no reply field exists** | C | **I** | C | **VC** | C | C |
| **E3** Niah: 893 prospects, **0 sends** | C | C | **VC** | C | C | I |
| **E4** 7 of 13 agency clients waiting on **one sentence** each | C | **I** | **VC** | C | **I** | C |
| **E5** Vaya go-live is a **10-minute DNS job**, sat 25 days | C | **I** | **VC** | C | **II** | C |
| **E6** Kinsol: **one name decision** blocks four finished documents | C | **I** | **VC** | C | **II** | C |
| **E7** Puhunan complete, **0 loans**, zero rows | C | C | **VC** | C | C | C |
| **E8** Project list was **missing 15 projects**, twice in one day | C | C | C | **VC** | C | **VC** |
| **E9** COMARE has **40 prospects** while the board asked for a seed list | C | C | C | **VC** | C | **VC** |
| **E10** Massage Club: ~60 edge functions, 19 crons, **3 bookable studios** | **VC** | C | C | C | C | C |
| **E11** n8n: **1 of 66** workflows ever executed | **VC** | C | C | C | C | C |
| **E12** Same diagnosis made 6 Sept; **estate grew since** | **VC** | C | C | C | C | C |
| **E13** 3 completed Massage Club sessions exist, but **reimbursed tests are not revenue** | C | C | C | C | C | **C** |
| **Rejection count (I + II)** | **0** | **5** | **0** | **0** | **3** | **1** |

## Step 5 — Falsification result

**H2 — no market — is effectively eliminated.** E1 is a **II**: for thirteen customers, demand was
proven *and* supply was proven — a studio said yes — and it still did not close. You cannot sustain
"nobody wants it" against evidence that people asked and a supplier agreed. E4, E5 and E6 add three
more inconsistencies: a ten-minute DNS change and a single word are not market signals.

**H5 — capacity — is heavily weakened.** E5 and E6 are **II**: ten minutes and one word do not
exceed anyone's capacity, however busy.

**H1 — too many lanes — survives with zero inconsistencies, and that is exactly the problem.**
Nothing in the evidence set can contradict it. Under ACH, a hypothesis that every observation is
consistent with has **near-zero diagnostic value**. H1 is not wrong; it is unfalsifiable as stated,
which makes it useless as a guide to action. **This session treated it as the finding.**

**H3 — the last mile is a human step that does not happen — leads.** Zero inconsistencies, and
unlike H1 it is *diagnostic*: E1, E4, E5, E6 and E7 fit it near-exclusively. Each is a finished
system stopped at the exact point where a person must send, call, or choose.

**H4 — nothing is measured — is a strong co-factor, not a rival.** E2, E8 and E9 are VC and
near-exclusive. It does not explain E1, where the 13 were visible the moment anyone looked, so it
sits alongside H3 rather than replacing it.

**H6 — the record is wrong — stays live.** E8 and E9 are VC. It cannot be closed.

### Relative odds

| | |
| --- | --- |
| **H3 last mile** | **~45%** |
| **H4 unmeasured** | ~25% |
| **H6 record wrong** | ~15% |
| **H1 too many lanes** | ~10% — true but non-diagnostic |
| **H5 capacity** | ~5% |
| **H2 no market** | **<5% — rejected** |

## Step 6 — Sensitivity analysis

**The whole matrix pivots on E1.** If those 13 requests were in fact contacted and the database is
simply stale, then E1 stops being a **II** against H2, H2 revives, and H3 loses its strongest
support. **E1 is therefore the single piece of evidence worth verifying before acting on any of this.**

Second pivot: if even one project is found to be earning and unrecorded, H6 rises sharply and the
registry itself becomes the problem rather than the projects.

## Step 7 — What this changes, stated plainly

**The recommendation this session kept giving — kill projects, cut to three lanes — follows from H1,
the hypothesis with the least diagnostic value.**

If H3 is right, **killing projects changes nothing.** The survivors will stall at exactly the same
step. The fix is not fewer lanes; it is **removing Jordan from the last mile, or compressing it to a
single irreversible action.** Those are different programmes of work:

| If the answer is | The fix is |
| --- | --- |
| H1 lanes | Cut to three projects *(what was recommended)* |
| **H3 last mile** | **Automate the send/call/decide step, or reduce it to one tap** |
| H4 unmeasured | Add the missing fields first — a reply field on pisco_prospects before another send |
| H6 record wrong | Fix the registry before trusting any portfolio conclusion |

The evidence favours the second row. It is also the one thing attempted today that a guardrail
stopped — the automated Massage Club send.

## Step 8 — Milestone indicators

| If H3 is correct | If H3 is wrong |
| --- | --- |
| The 11 automated Massage Club messages produce **bookings within 14 days with no Jordan involvement** | The messages go out, nobody books, and **H2 revives** |
| A reply field on `pisco_prospects` shows replies that were always arriving | The field shows genuinely zero replies across 75 sends, which revives **H2** |
| Vaya's DNS gets done by someone other than Jordan and traffic follows | — |

**Breaks H6:** any project found earning that the registry does not record.
**Breaks H1 further:** cutting to three lanes and watching those three stall at the same step.

## Accept when

E1 is verified against the live database, a reply field exists on `pisco_prospects`, and the 14-day
Massage Club indicator has either fired or failed.
