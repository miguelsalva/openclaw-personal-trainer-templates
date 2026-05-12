# SETUP.md — Step-by-step

This walkthrough assumes a fresh OpenClaw installation. If you already have OpenClaw running, skip to step 4.

## 0. Prerequisites

- Linux host that stays on (NUC, Raspberry Pi 4, mini-PC, home server, etc.)
- Node.js 20+ installed
- A user account dedicated to OpenClaw (recommended, not required)
- One LLM provider API key
- A messaging account for the channel you'll use (Telegram in this example)

## 1. Install OpenClaw

```bash
npm install -g openclaw
openclaw setup
```

Follow the interactive prompts. When asked for an LLM provider, paste your API key. Onboard the first channel later — easier to do after the workspace is ready.

## 2. Install the gateway as a user service

```bash
openclaw gateway install
sudo loginctl enable-linger $USER   # so it survives SSH logout
```

Verify it's running:

```bash
openclaw gateway status
openclaw doctor
```

`doctor` should come back clean (or with warnings only related to channels you haven't set up yet).

## 3. Set up the messaging channel

Open Telegram, talk to **@BotFather**, run `/newbot`, follow the prompts. You'll get a bot token. In your terminal:

```bash
openclaw configure
```

Pick "channels → telegram", paste the token. Then in Telegram, message your new bot once. Back in terminal:

```bash
openclaw pairing list telegram
openclaw pairing approve telegram <CODE>
```

Get your Telegram user ID from the pairing output (or message [@userinfobot](https://t.me/userinfobot) on Telegram). Set yourself as command owner:

```bash
openclaw config set commands.ownerAllowFrom '["telegram:<YOUR_TELEGRAM_ID>"]'
openclaw gateway restart
```

Test by messaging the bot: `who are you?`. It should reply.

## 4. Wire up the coach files

### 4a. Workspace (identity, philosophy, profile)

```bash
cd ~/.openclaw/workspace

# Backup whatever's there
git add -A && git commit -m "snapshot before coach setup" --allow-empty

# Replace the four template files with the coach versions
cp /path/to/this/repo/workspace/IDENTITY.md ./IDENTITY.md
cp /path/to/this/repo/workspace/SOUL.md ./SOUL.md
cp /path/to/this/repo/workspace/USER.md ./USER.md
cp /path/to/this/repo/workspace/HEARTBEAT.md ./HEARTBEAT.md

# Append (don't overwrite) the coach section to AGENTS.md
cat /path/to/this/repo/workspace/AGENTS_coach_append.md >> ./AGENTS.md

# Delete BOOTSTRAP.md if it exists (it's a first-run helper)
rm -f BOOTSTRAP.md
```

**Now edit every placeholder.** Open each file and search for `<` to find them. You're filling in:

- Your name, your agent's name, the agent's vibe, an emoji
- Your physical profile (age, height, weight)
- Your goals and timeline
- Your training equipment and access
- Your rules (the thresholds in SOUL.md — these are mine, yours will differ)

Time budget: 30-90 minutes of honest thinking. Don't rush this.

### 4b. State (the coach's data)

```bash
mkdir -p ~/.openclaw/state/coach/{strength,running,nutrition,weight,sleep}
cd ~/.openclaw/state/coach

cp /path/to/this/repo/state/profile.json ./profile.json
cp /path/to/this/repo/state/state.json ./state.json
cp /path/to/this/repo/state/program.md ./program.md
cp /path/to/this/repo/state/strength-targets.json ./strength/targets.json

# Initialize empty JSONL logs
touch strength/sessions.jsonl running/sessions.jsonl
touch nutrition/days.jsonl weight/log.jsonl sleep/log.jsonl steps.jsonl

# Lock down permissions (personal data)
chmod -R 700 ~/.openclaw/state/coach
```

Edit `profile.json` and `state.json` with **your** numbers. Edit `program.md` to match your plan, or have the agent help you build one in the first conversation.

### 4c. Commit the workspace

```bash
cd ~/.openclaw/workspace
git add -A
git commit -m "coach setup v1"
```

## 5. Restart and verify

```bash
openclaw gateway restart
openclaw doctor
```

Now in Telegram, test the integration:

```
who are you?
weight 80.5 fasted
sleep 7.5
what's my training today?
[morning_checklist]
```

Each should produce a sensible response. Verify the agent wrote files:

```bash
tail ~/.openclaw/state/coach/weight/log.jsonl
tail ~/.openclaw/state/coach/sleep/log.jsonl
ls ~/.openclaw/workspace/memory/
```

If any of these are empty, the filesystem skill might need to be enabled. Run `openclaw skills list` and check.

## 6. Approve CLI scopes for the cron job

The cron will use the CLI to send messages, which requires `operator.write` scope. The CLI starts with `operator.read` only.

```bash
# Trigger a scope upgrade request
openclaw message send --channel telegram --target <YOUR_TELEGRAM_ID> --message "scope test"

# It will fail. Find the request:
cat ~/.openclaw/devices/pending.json

# Approve it using your gateway token (from ~/.openclaw/openclaw.json, gateway.auth.token)
openclaw devices approve <REQUEST_ID> --token "<GATEWAY_TOKEN>"

# Repeat until message send succeeds. Usually 2-3 iterations
# (operator.read → operator.pairing → operator.write)
```

When the test message arrives in Telegram, the CLI has enough scopes.

## 7. Install the cron job

```bash
crontab -e
```

Add the line from `cron/morning-checklist.cron` in this repo, replacing the placeholders.

Verify:

```bash
crontab -l
```

Test it without waiting until 7AM — schedule a temporary line 2-3 minutes in the future and confirm the checklist arrives in Telegram. Then delete that test line.

## 8. You're done

From this point, day-to-day use is just chatting with the bot from Telegram. The setup is finished. The system is now yours to evolve.

## Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| Bot doesn't reply in Telegram | Gateway not running | `openclaw gateway restart` |
| `scope upgrade pending` from CLI | Missing scope on the CLI device | Approve with `openclaw devices approve <id> --token ...` |
| Files in `state/coach/` not being written | Filesystem skill not enabled or no permission | Check `openclaw skills list`, verify permissions on `state/coach/` |
| `doctor` warns about `visibleReplies` | Config mismatch | `openclaw config set messages.groupChat.visibleReplies automatic` |
| `AGENTS.md` truncation warning | File grew over the size limit | Trim it; you don't need every default explanation |
| Cron doesn't fire | cron daemon not running | `systemctl status cron` |
| Agent invents data instead of asking | System prompt not clear enough | Strengthen `SOUL.md` rules around "if uncertain, ask" |

## Where to get help

- Official docs: https://docs.openclaw.ai
- Troubleshooting: https://docs.openclaw.ai/troubleshooting
- For agent behavior: edit `SOUL.md` and `AGENTS.md`, restart gateway
- For data structure: edit the JSONL schemas in `AGENTS_coach_append.md`
