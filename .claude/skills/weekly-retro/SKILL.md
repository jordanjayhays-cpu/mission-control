---
name: weekly-retro
description: Weekly review that improves Jordan's Claude system itself. Turns the week's corrections into permanent rules and procedures. Use when Jordan says "weekly retro", "review my week", "what should I change", or on Fridays.
---

# Weekly retro

Purpose: the system gets better only if corrections become rules. This is where that happens.

## Ask Jordan (one message)
1. What did you use Claude for this week? (rough list)
2. Where did you have to correct it, rewrite, or give up?
3. What task did you do by hand that felt repetitive?
4. Results, as numbers: Placewell (meetings booked, proposals sent, deals signed), Massage Club (bookings made vs goal of 5), any third lane.

## Produce
1. **Scoreboard:** 3 lines, results vs goals. No spin. Then write it where it lasts: one row per lane per metric in `lane_metrics` on Supabase `dprdnrgjkzgfgtcsguuq` (columns: week = this Monday's date, lane, metric in {meetings_booked, bookings_made, deals_signed, routine_firings}, value, delta vs the previous week's row, note). Four weeks of rows is what lets each Routine earn or lose its place.
2. **New rules** (max 3): each correction that happened twice, rewritten as a one-line rule, labeled with where it goes:
   - Preferences (applies everywhere)
   - Project: Placewell / Massage Club / MBA
3. **Rules to delete:** any instruction that didn't change behaviour. Shorter instructions get followed better.
4. **Procedure candidate:** any task done 3+ times → draft a skill outline (name, trigger, steps).
5. **Delegation check:** one thing Jordan did that Claude should do, and one thing Claude did that Jordan should own.
6. **Next week's one focus.**

## Rules
- Max 1 page.
- Every recommendation must be something Jordan can paste or do in under 5 minutes.
- Anything only Jordan can do: "BOARD: ...".
