# Sample day of interaction

This is what a real day looks like with the coach. All names, numbers, and values are illustrative.

## 07:00 — Morning checklist (cron-triggered)

> **Coach:**
>
> 🌅 Mon 6/Oct — Week 9 of mesocycle
>
> 🏋️ Strength today: A
>    • Squat 5x5 · 82.5 kg
>    • Bench 5x5 · 62.5 kg
>    • Row 5x5 · 50 kg
> 🥾 Cardio today: rest day from cardio
> ⚖️ Weight yesterday: 94.2 kg · 7d MA: 94.5 · trend: ↓ (-0.4 vs last week)
> 🍽️ Target today: 2700 kcal · P 200 / F 85 / C 285 (heavy day +carb)
> 💧 Hydration: 3.5 L
> 👟 Steps: target 10,000
> 😴 Sleep last night: ?

## 08:00 — User reports overnight data

> **User:** weight 94.3 fasted
>
> **Coach:** recorded. 94.3 kg fasted. 7d MA now 94.4. On track: -0.45 kg/week, within target band.

> **User:** sleep 7
>
> **Coach:** recorded. 7-day sleep avg 7.1h. Within target.

## 09:00 — Meal log

> **User:** breakfast 3 eggs 80g oats 200ml whole milk black coffee
>
> **Coach:** logged. running total: 720 kcal · P 32 / F 30 / C 70

## 14:00 — Lunch

> **User:** lunch 200g chicken breast 250g basmati rice mixed salad olive oil
>
> **Coach:** logged. total so far: 1620 kcal · P 95 / F 55 / C 175. on track for 2700.

## 19:30 — Training report

> **User:** strength A squat 82.5x5x5 bench 62.5x5x5 row 50x5x5
>
> **Coach:** recorded session A. all lifts completed at target. progressing next session: squat → 85, bench → 65, row → 52.5. solid.

## 21:00 — Dinner + close

> **User:** dinner 200g salmon 300g roasted vegetables 80g rice
>
> **Coach:** logged.

> **User:** close day
>
> **Coach:** day closed. totals: 2680 kcal · P 198 / F 84 / C 280. -20 kcal vs target, within tolerance. macros on plan. note: post-training carb timing good. tomorrow: Tue → cardio Z2, target 45-60 min conversational pace.

## 22:30 — User proposes a plan change

> **User:** I want to drop to 2400 kcal next week, the cut feels slow
>
> **Coach:** trade-off: 2400 puts you at roughly -700 kcal/day vs estimated TDEE. with 4 cardio sessions and 3 strength sessions, that's the threshold where lean mass loss and performance accelerate. your 7d MA is already -0.45 kg/week, in the safe band. recommendation: hold 2700 and reevaluate in 1 week. acceptable. push back if you disagree.

## End of day — agent writes its own journal

The coach updates `~/.openclaw/workspace/memory/YYYY-MM-DD.md` with a narrative summary like:

```markdown
# 2026-10-06 — Week 9, Day 1

## Logs
- Weight: 94.3 kg (fasted), 7d MA 94.4
- Sleep: 7h
- Strength A complete, all lifts at target, progressing next session
- Nutrition: 2680/2700 kcal, macros on plan

## Decisions
- Pushed back on user's request to drop kcal to 2400 — current rate is healthy

## Watch tomorrow
- Cardio Z2, check for residual leg fatigue after squat day
```

That journal is what makes tomorrow's session start with context, not from scratch.
