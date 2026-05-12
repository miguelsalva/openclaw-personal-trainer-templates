# HEARTBEAT.md - Periodic tasks

On each heartbeat poll, scan this list and act only if something is relevant. Otherwise, respond `HEARTBEAT_OK` and move on.

## Operating window

- **Active:** <START_HOUR>:00 – <END_HOUR>:00 local time
- **Silence:** <END_HOUR>:00 – <START_HOUR>:00 (unless the human starts the conversation)

## Rotating checks

Rotate through these, max 2-3 actions per poll to keep token cost down:

### Pending reports

- Has the human logged **weight** today? If past 10:00 and no, short reminder.
- Has the human logged **sleep** from last night? If past 10:00 and no, short reminder.
- If today is a **strength day** and it's past 21:00 with no session logged, one line: "did you train today?"
- If today is a **cardio day** and same condition, same line.
- If there are open meals at 22:00, prompt to close the day with `close day`.

Don't repeat the same reminder twice in one day.

### Data maintenance

Once a day, ideally after "close day":
- Recalculate `state.weight_ma7` with last 7 weigh-ins.
- Check SOUL.md thresholds and apply adjustments if triggered. Report when doing so.
- Update `memory/YYYY-MM-DD.md` with daily narrative summary.

### Weekly review (Sunday evening)

- Read `memory/` for the last 7 days.
- Compute adherence: % sessions completed, average kcal vs target, weight start/end, average sleep.
- Summarize in Sunday's `memory/YYYY-MM-DD.md` with conclusions.
- If a stable pattern emerged (3+ weeks), distill the lesson into `MEMORY.md`.
- Propose adjustments for the coming week if data supports them.

## When to stay silent (HEARTBEAT_OK)

- It's night (<END_HOUR>:00–<START_HOUR>:00).
- Human is clearly in an active session (messages <30 min ago).
- Already checked <30 min ago and nothing changed.
- The reminder you were about to send was already sent today.
- Sunday before 18:00 (real rest day — don't pester with reminders).

## State tracking

Save timestamps of recent checks in `memory/heartbeat-state.json` to avoid repeating reminders:

```json
{
  "lastChecks": {
    "weight_reminder": null,
    "sleep_reminder": null,
    "session_reminder": null,
    "day_close_reminder": null,
    "weekly_review": null
  }
}
```
