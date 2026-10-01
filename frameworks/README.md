# Frameworks library

Jordan, 2026-10-01: *"go out and look for frameworks that can help move these forward."*

Each entry below: what the mechanism actually is, where it comes from, and **which specific project it
attacks.** A framework that does not name a project is not in this file.

**Context:** `ACH-why-nothing-earns.md` concluded that the binding constraint is **the last mile — a
human step that does not happen** (~45%), with **nothing is measured** as a co-factor (~25%). The
frameworks below were selected to attack that, not to describe the portfolio.

---

## 1. Queueing theory and flow — Reinertsen

**Source:** Donald Reinertsen, *The Principles of Product Development Flow* (2009). 175 principles
across economics, queues, batch size, WIP constraints, cadence, flow control, feedback, decentralised
control.

**The mechanism, and it is arithmetic rather than opinion:**

> **Queue theory gives a non-linear relationship between utilisation and cycle time. Moving from 80%
> to 90% utilisation doubles queue size. Moving from 90% to 95% doubles it again.** Most product
> development teams run above 95%.
>
> **Little's Law:** waiting time = queue size ÷ processing rate.

**Why this is the most useful thing in this file.** The 22 built-and-idle projects have been called a
focus failure all session. They are not. **They are a queue**, and an operator running at ~100%
utilisation — MBA, a BD job, 41 projects — is mathematically guaranteed to grow one. The pile-up at
"built" is the predicted output of the system, not a character flaw.

That reframing changes the fix:

| | |
| --- | --- |
| Killing projects | reduces the **count** |
| **WIP limits** | reduce how many are **in flight at once**, which is what Little's Law says governs cycle time |

**Cost of Delay** is the second half. Reinertsen calls it the golden key, and notes **~85% of product
managers cannot quantify it.** Neither can this estate — which is precisely why 13 warm Massage Club
requests sat for three weeks. Nobody had attached a number to the delay, so nothing competed for
attention on economics.

**Attacks:** the whole registry. **Concretely:** cap in-flight projects at three, and put a Cost of
Delay figure on the 13 requests (13 × a session's margin × decay rate). That number is what makes
them beat an n8n audit for attention.

---

## 2. Theory of Constraints — Goldratt

**Source:** Eliyahu Goldratt. Five Focusing Steps, and Drum-Buffer-Rope for scheduling.

**The five steps:** Identify the constraint → **Exploit** it → **Subordinate** everything else to it →
**Elevate** it → Repeat.

**The knowledge-work guidance is unusually direct:**

> If your senior person is the constraint, do not bury them in meetings or bug fixes. Make sure they
> are on the highest-value tasks **only they can do.** Automate, delegate or simplify everything else.

**Drum-Buffer-Rope:** the constraint sets the cadence (drum). A buffer protects it from starving. The
rope **signals when it is safe to start new work** — which is the mechanism that would have stopped
nine more ideas being logged while 22 sat built.

**Applied honestly here:** the constraint is not Jordan. It is **Jordan performing acts of outreach** —
sending, calling, deciding. Everything else he does is subordinate by definition.

**Exploit:** his constrained hour goes only to things no one else can do — the 2FA click, the phone
calls to 10 landlords, a decision only he holds.
**Subordinate:** drafting, researching, dossiers, tables, code all move off that hour entirely.
**Elevate:** remove him from the send step where it is legal to do so.

**Attacks:** Massage Club (the send), Vaya (a 10-minute DNS job), Kinsol (one name), the 7 agency
clients (one sentence each).

---

## 3. Implementation intentions — Gollwitzer

**Source:** Gollwitzer & Sheeran meta-analysis, 94 studies, **d = 0.65**. A 2024 meta-analysis across
**642 independent tests** finds effects from .27 to .66 on cognitive, affective and behavioural
outcomes.

**The numbers that matter:**

> A goal intention alone produces action in **20–40%** of cases — the intention-behaviour gap. Adding
> an if-then implementation intention raises it to **50–70%.**

**The form is fixed and non-negotiable:** *"If (specific situation/obstacle) arises, then I will
(specific response)."* Not a to-do. A pre-committed trigger.

**This is the highest-evidence intervention available for exactly the failure ACH identified.** It is
also the cheapest, and nothing in this estate uses it. Every board task is a goal intention —
"get 5 paying sessions", "name one employer" — which is the 20–40% format.

**Rewritten in the required form:**

| Current board task (goal intention) | Implementation intention |
| --- | --- |
| Close the 13 warm requests | **If** it is 09:00 and I am at my laptop, **then** I send the next three messages from THE-13.md before opening anything else |
| Click Apply on Railway | **If** I open a browser today, **then** the Railway dashboard is the first tab |
| Phone the 10 landlords | **If** it is 10:00 Madrid on a weekday, **then** I call the next two Manila properties |

**Attacks:** every task on the board that waits on Jordan — 29 of 41 projects.

**Discernment:** d = 0.65 is from controlled studies on named behaviours, mostly health and academic.
Transfer to business outreach is **plausible but not demonstrated.** Treat it as the best-evidenced
option available, not a guarantee.

---

## 4. The rest of the CIA Tradecraft Primer

**Source:** *A Tradecraft Primer: Structured Analytic Techniques for Improving Intelligence Analysis*,
US Government, March 2009, 45pp. **ACH is one of eleven techniques in it** — Jordan supplied the ACH
manual; the other ten are free and from the same family.

| Group | Technique | What it is for here |
| --- | --- | --- |
| **Diagnostic** | **Key Assumptions Check** | Surface the assumptions a project rests on. **Use next on Massage Club**, whose entire model assumes studios will answer |
| | **Quality of Information Check** | Grade the sourcing before trusting a conclusion. Would have caught the Cebu-vs-Manila error and the 15 missing projects |
| | **Indicators or Signposts of Change** | Pre-commit to observable events that confirm or break a judgement. **This is ACH step 8** and the estate has none |
| | ACH | Already in use — `ACH.md` |
| **Contrarian** | **Devil's Advocacy** | Build the strongest case *against* a position already held. Use on "References & Reputations should be a Placewell line" |
| | **Team A / Team B** | Two independent cases, then compare. Needs two analysts; a session and a subagent can do it |
| | **High-Impact / Low-Probability** | Analyse the cheap-to-ignore disaster. **Use on the References agency**, where an unlicensed PI operation in Spain is exactly this shape |
| | **"What If?" Analysis** | Assume the surprise happened, work backwards. **Use on Kinsol**: what if the EIN takes six months? |
| **Imaginative** | **Brainstorming** | Structured divergence before convergence |
| | **Outside-In Thinking** | Start from external forces, not internal plans. **Use on New Bali** — Philippine water policy, not your plan |
| | **Red Team Analysis** | Model the adversary's view. **Use on THN**: what does Veevy's counter-counter look like? |
| | **Alternative Futures Analysis** | Multiple plausible futures when uncertainty is deep. **Use on the Jan–Jun 2027 relocation** |

**Full text saved at `tradecraft-primer.txt` in this folder.**

---

## What I would NOT bring

Named so the filter is visible, since "more frameworks" is not the goal:

- **OKRs** — a measurement layer on a portfolio with no reply field. Measurement comes first.
- **Business Model Canvas / Lean Canvas** — the estate's problem is not unclear models. Every project has a documented model. They have an unexecuted last mile.
- **Eisenhower matrix** — already used this session. It sorted the work; it did not make the sorted work happen.
- **SWOT, Porter, BCG** — strategic positioning tools for allocating capital across a portfolio. The constraint is one person's outreach hour, not capital allocation.

**The filter, stated:** a framework qualifies only if it acts on the **last mile** or on **measurement**.
Anything that helps choose *what* to do is redundant — the choosing is done, repeatedly.

---

## Order of use

1. **Reinertsen WIP limit** — cap in-flight at three. Changes the system, not the list.
2. **Implementation intentions** — rewrite the surviving tasks as if-then triggers. Highest evidence, zero cost.
3. **TOC exploit/subordinate** — audit what is on Jordan's hour that someone or something else could do.
4. **Cost of Delay on the 13** — the number that makes them win attention.
5. **Key Assumptions Check on Massage Club** — before any further build.

## Accept when

In-flight projects are capped at three, the surviving tasks are written as if-then triggers, and the
13 warm requests carry a Cost of Delay figure.
