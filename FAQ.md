# Hermes BIOS-mode — FAQ

## What is BIOS-mode?

A Hermes Agent skill that turns the agent into a BIOS-style configuration panel for tuning itself. When you say "bios mode", the agent drops its personality, prints a status screen, and enters a flat, terse, draft-approve-apply discipline for editing its own files — SOUL.md, memories, config, skills, and more.

The name is the metaphor: a BIOS is the setup screen that runs *before* the OS loads, where you tune the machine itself rather than use it. BIOS-mode is that, for an agent.

## What is it *not*?

- **Not a separate app or dashboard.** It runs inside your normal chat with the agent. You say "bios mode" and the same conversation switches into config-panel mode.
- **Not a memory system.** It deliberately does *not* write to MEMORY.md, USER.md, or fact_store unless you explicitly ask. It's for tuning, not for remembering.
- **Not a backup system.** Snapshots cover a single session's edits. They are not a strategy for backing up your whole profile — that's what cron jobs are for.
- **Not a security boundary.** The owner lock is a soft contract plus prompt-injection defense. It stops the agent from being talked into editing itself; it does not stop someone who already owns the account.

## How do I start it?

Just say **"bios mode"** in chat. The agent prints a POST status block and waits for commands.

## How do I leave it?

Say **"exit bios mode"** (or "exit BIOS"). The agent confirms nothing is pending, logs the session, and resumes its normal persona from the next message.

## What can I tune?

Fourteen subsystems, from SOUL.md (identity) to config.yaml (model, providers, theme) to cron jobs and plugins. The full inventory with real commands is in `references/hardware-map.md`. In-session, `menu` lists them all and `menu <n>` opens one.

## Why does it refuse to edit something?

Three likely reasons:

1. **You haven't said "apply".** BIOS-mode drafts changes and shows you a diff, but writes nothing until you explicitly approve. That's the whole point.
2. **The profile is locked.** If an owner lock is set, only the named owner can approve edits. Anyone else gets a denial line.
3. **It's a protected subsystem.** Cron and gateway changes are ask-first by design, and the mode never touches another profile's files.

## What's the owner lock?

An optional per-profile policy (`bios/policy.md`) that names an owner and sets `locked: true`. When locked, only the owner can enter BIOS-mode and approve edits. It's meant for shared or family deployments — e.g. an agent at your mom's house that only *you* should be able to reconfigure.

It's a soft contract, not authentication. Real identity is enforced at the platform/gateway layer.

## I made a bad edit. How do I undo it?

Say **"revert"**. BIOS-mode snapshots every file before it writes, so the last snapshot can be restored. Revert is itself an edit — it shows you what will change and needs your go-ahead, and it's logged like any other change.

## Where do snapshots and logs live?

In `<profile-root>/bios/`:

- `snapshots/` — timestamped pre-edit copies, each with a MANIFEST
- `bios.log` — one line per apply/revert/session-end
- `policy.md` — the optional owner lock

The directory is mode 0700 and files 0600, because snapshots can contain secret-bearing files like `.env`.

## Does it work on Windows?

No. The POST shell template is POSIX (bash). Linux and macOS are supported.

## Does it work on every Hermes version?

The commands in the hardware map were verified against the `hermes` CLI. If you're on a very different version, a path or command may have drifted — run `selftest` to check the mode's own machinery, and `hermes doctor` for config health.

## Why is the agent so terse in BIOS-mode?

Deliberately. BIOS-mode suspends the profile's normal communication customs (warmth, acknowledgements, check-ins) and responds like a config panel: one line, no filler. It's a tuning screen, not a conversation.

## Why does entering BIOS-mode sometimes feel slow?

That's usually the session model burning reasoning tokens on trivial steps. BIOS-mode never needs deliberation — if entry lags, switch or pin the session model (`hermes model`). The skill itself is designed to gather its status in a single batched call.

## Can I contribute?

Yes. The skill is MIT-licensed. See the repo for the layout and open an issue or PR.

## Where's the full command reference?

In-session, type `?`. In the repo, see the "Commands" table in README.md and the full skill text in SKILL.md.
