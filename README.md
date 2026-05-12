# OpenClaw Coach — Self-hosted AI personal trainer

Companion repo to the Medium article *["My Personal Trainer Is a Node Process Running on My NUC"](#)*.

This is **not a turnkey installer**. These are the configuration files I use, anonymized, so you can adapt them to your own setup. The article explains the *why*; this repo gives you the *what to copy*.

## What you'll need

- A Linux box that's always on (Raspberry Pi 4 minimum, a NUC or similar is better)
- Node.js 20+
- An API key from one LLM provider (Anthropic, OpenAI, or Ollama for fully local)
- A Telegram account (or Discord, WhatsApp, Signal — OpenClaw supports several)
- About 2-4 hours for the first setup
- Comfort editing JSON/Markdown without panic

## What this repo contains

```
.
├── README.md                    ← you are here
├── SETUP.md                     ← step-by-step install walkthrough
├── workspace/                   ← files that go in ~/.openclaw/workspace/
│   ├── IDENTITY.md             ← who your agent is
│   ├── SOUL.md                  ← philosophy and hard rules
│   ├── USER.md                  ← your profile (template)
│   ├── AGENTS_coach_append.md   ← append to AGENTS.md
│   └── HEARTBEAT.md             ← periodic tasks
├── state/                       ← starting files for ~/.openclaw/state/coach/
│   ├── profile.json
│   ├── state.json
│   ├── program.md
│   └── strength/targets.json
├── cron/
│   └── morning-checklist.cron   ← cron line for 7AM trigger
└── examples/
    ├── telegram-inputs.md       ← quick-input formats the agent understands
    └── sample-day.md            ← what a day of interaction looks like
```

## How to use this repo

1. Read `SETUP.md` end-to-end before touching anything.
2. Fill in **every `<PLACEHOLDER>`** in the files. Search for `<` to find them all.
3. Copy files to the right paths on your machine.
4. Restart the gateway.
5. Iterate. These files are a starting point, not a finish line.

## What I'm NOT shipping here

- My actual personal data (weight, schedule, real targets)
- API keys, tokens, chat IDs
- The full plan I'm running (it's mine, calibrated to my age/lifts/race)
- Strava/Garmin integration (still WIP on my side)

## License

MIT. Use, fork, adapt freely. Don't ship a SaaS that just wraps this — that misses the whole point.

## Disclaimer

This is what works for me. I'm not a doctor, dietitian, or certified coach. Adapt with judgment, and if you have any medical conditions, talk to a real human professional before letting an AI tell you what to eat or how to train.

