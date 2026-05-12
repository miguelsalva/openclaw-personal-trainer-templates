# Telegram input cheatsheet

These are the fast-path formats the agent understands. Natural language also works — these are just shortcuts.

## Body data

```
weight 80.4
weight 80.4 fasted
sleep 7
sleep 6.5 bad
steps 11500
```

## Strength sessions

```
strength A ok
```
Records full session at current targets from `strength/targets.json`.

```
strength A squat 75x5x5 bench 57.5x5x4 (last set failed) row 45x5x5
```
Records with explicit reps × sets × weight. Notes go in parentheses.

## Cardio sessions

```
trail z2 8km 200d+ 45min
```
Type, distance, elevation gain, duration.

```
trail quality 6km 100d+ 35min @ 5x800m hard
```
Free text after the basics for workout details.

```
trail long 18km 700d+ 2h05min
```

## Nutrition

During the day, log each meal:

```
breakfast 3 eggs 100g oats 200ml whole milk coffee
snack 1 apple 30g almonds
lunch 200g chicken breast 200g rice salad olive oil
dinner 200g salmon 300g vegetables
```

At end of day:

```
close day
```
The agent aggregates kcal/macros and writes one line to `nutrition/days.jsonl`.

## Plan changes

```
swap today: trail z2 today, strength A tomorrow
```
Agent updates `memory/YYYY-MM-DD.md` so context persists.

```
deload week
```
Agent flags `state.deload_flag = true` and adjusts the week.

## Queries

```
what's my training today?
how's my weight trending?
am I on track for [race name]?
what did I eat this week?
show me my last 5 squat sessions
```

## Special commands

```
[morning_checklist]
```
The morning routine (cron fires this automatically; you can also type it manually).

```
/diagnostics
```
Owner-only OpenClaw command for system diagnostics.

```
/model opus
/model sonnet
```
Switch the underlying LLM if you've configured aliases.
