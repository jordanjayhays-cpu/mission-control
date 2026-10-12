# Jordan's Claude Operating System

How Claude would run itself if it were doing your work. Built from Anthropic's own engineering guidance
(context engineering, effective agents, Claude Code best practices). Sources at the bottom.

---

## The one constraint everything is designed around

Claude has no memory between chats, and gets worse as a single chat gets long and cluttered.
So the whole system is: **keep each chat small and focused, keep everything that must persist outside the chat, and load it back in on purpose.**

Anthropic's wording: context is a finite "attention budget"; aim for "the smallest possible set of high-signal tokens."

---

## The 7 layers

| # | Layer | What lives there | Loaded when | Where (claude.ai) | Rule |
|---|---|---|---|---|---|
| 1 | **Identity** | Who you are, how you want answers | Every chat | Settings > Profile > preferences | Under 20 lines. A bloated file gets ignored. |
| 2 | **Lane** | One business area's facts + rules | Every chat in that lane | One Project per lane | 3 lanes max: Placewell, Massage Club, MBA |
| 3 | **Procedures** | Repeatable workflows (step-by-step) | Only when the task matches | Skills | If you've done it 3 times, make it a skill |
| 4 | **Tools** | Access to your real data | When a step needs it | Connectors (Gmail, Granola, Notion, calendar) | Fewest tools that cover the work |
| 5 | **State** | What's true now: tasks, decisions, pipeline | Read at start, written at end | Your board + a Notion/doc "decision log" | If it's not written down, it doesn't exist |
| 6 | **Automation** | Things that run without you asking | On a schedule | Scheduled tasks / routines | Only automate what you've done by hand and trust |
| 7 | **Verification** | A check on every output | Before anything leaves | A second, fresh chat as critic | The writer never grades its own work |

**How they connect:** Identity + Lane load automatically. You trigger a Procedure. The Procedure pulls data through Tools,
writes results to State, and Verification checks the output before you send it. Automation runs Procedures on a timer.

---

## The session protocol (how Claude talks to itself)

Every working chat follows the same 5 steps.

**1. Route.** Which lane? Open that Project. One task per chat.

**2. Brief (30 seconds).** Three lines:
```
Goal: [outcome, not task]
Material: [paste or point to the real thing: notes, thread, numbers]
Done when: [how we'll both know it's right]
```

**3. Plan before doing.** "Tell me your approach in 3 bullets before you write anything."
Fixing a bad plan costs 10 seconds. Fixing a bad draft costs 10 minutes.

**4. Do, then verify with a separate critic.**
Open a *new* chat (same Project) and paste only the output:
"You are [the recipient]. What's weak, unclear, or likely to get ignored? Top 3 only."
A fresh chat isn't biased toward work it just produced.

**5. Close the loop (1 minute).**
- Anything only you can do → board.
- Anything you corrected twice → add it as a rule to the Lane instructions.
- Anything worth remembering → decision log.

**Escape rule:** corrected Claude twice on the same thing and it's still wrong? Stop. Start a fresh chat
with a better brief that includes what you learned. A clean chat with a better prompt beats a long chat with corrections.

---

## The 5 work patterns (pick one per task)

| Pattern | What it is | Use it for |
|---|---|---|
| **Chain** | Step 1's output feeds step 2 | Research company → pick angle → draft email → critique |
| **Parallel options** | Same task, 3 different takes | Subject lines, positioning, pricing proposals (safe/bold/weird) |
| **Writer + Critic** | One chat writes, a fresh one attacks | Proposals, investor or client material, anything high-stakes |
| **Interview me** | Claude asks you questions until it has enough, then writes | Big fuzzy things: a strategy, a pitch, a plan |
| **Research** | Claude searches many sources, returns a summary | Market sizing, prospect lists, competitor scans |

Anthropic's advice on all of these: use the simplest one that works. Most tasks are a single good brief.

---

## Your system, filled in

### Lane: Placewell BD
- **Procedures (skills):** `call-to-followup` (included), call prep, proposal draft
- **Tools:** Granola (call transcripts), Gmail (drafts), calendar
- **State:** pipeline list (company, stage, next step, date)
- **Automation:** every weekday morning: "Which deals have no next step dated this week?"
- **Verification:** critic chat plays the prospect's procurement lead

### Lane: Massage Club
- **Procedures:** `concierge-booking` (included)
- **Tools:** WhatsApp (manual), Supabase data
- **State:** booking log: who, when, what they said, what you learned
- **Automation:** weekly: bookings this week vs goal of 5
- **Verification:** "Does this get the next booking? If not, why are we doing it?"

### Lane: MBA
- **Procedures:** case prep (quiz me first, answer second)
- **State:** deadlines
- **Verification:** your school's AI-use policy

### Across all lanes
- **Procedure:** `weekly-retro` (included). This is the step that makes the system get better over time.

---

## The compounding loop

Week 1, you correct Claude a lot. Each correction becomes a rule (Lane) or a step (Procedure).
Week 4, most tasks are one line: "call-to-followup for the Hilton call."
The skill is not prompting better each time. The skill is **turning every correction into a permanent rule.**

---

## Setup order

1. Today: trim preferences to under 20 lines (Identity).
2. Today: create the Placewell Project (Lane) and upload `call-to-followup` (Procedure).
3. This week: run every Placewell call through it. Note every correction.
4. Friday: run `weekly-retro`. Apply its rule changes.
5. Next week: Massage Club lane plus `concierge-booking`.
6. Week 3: first scheduled task (Automation), only for a procedure you already trust.

---

## Sources
- Anthropic, "Effective context engineering for AI agents": https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- Anthropic, "Building effective agents": https://www.anthropic.com/engineering/building-effective-agents
- Claude Code best practices: https://code.claude.com/docs/en/best-practices
- Anthropic, "Equipping agents for the real world with Agent Skills": https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills
