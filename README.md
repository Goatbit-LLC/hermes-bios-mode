# Hermes BIOS-mode

Turn your Hermes Agent into a BIOS-style configuration panel for tuning itself.

When you say **"bios mode"**, the agent drops its personality, prints a POST status block, and enters a flat, terse, draft-approve-apply discipline for editing its own files. It snapshots before it writes, logs every change, and can revert.

Think of it as the setup screen that runs *before* the OS loads — except the "OS" is the agent's own identity, memory, and configuration.

---

## Why

Agents edit their own files all the time — SOUL.md, memories, config, skills. Most of the time that's fine. But when you're deliberately *tuning* an agent, you want the opposite of a chatty assistant. You want:

- **A status screen**, not a greeting — what profile am I in, what model, how full is memory, what's the config state.
- **A map** of every tunable subsystem, with the real command to read and change each one.
- **A discipline** — nothing gets written until you say so, and every write is snapshot and logged.
- **A way back** — revert to the last snapshot, or just exit and resume the normal persona.

BIOS-mode is that discipline, packaged as a skill.

---

## Features

- **POST on entry** — a status block showing profile, model, memory headroom, skills/cron counts, config state, and lock status.
- **Hardware map** — a 14-subsystem inventory of every tunable part of the agent, with real read/change commands for each.
- **Menu navigation** — `menu`, `menu <n>`, `back`, `status`, `?` to move between subsystem screens.
- **Draft → approve → apply** — nothing is written without an explicit go-ahead.
- **Snapshot / revert** — pre-edit copies with a manifest, restorable on request.
- **Audit log** — one line per apply/revert/session-end, a walkable trail.
- **Owner lock** — optional per-profile policy restricting edits to a named owner.
- **Self-test** — verifies the mode's own machinery without touching live files.

---

## Install

Copy the skill into your Hermes skills directory:

```bash
mkdir -p ~/.hermes/skills/creative
cp -R bios-mode ~/.hermes/skills/creative/
```

Or clone directly:

```bash
git clone <repo-url> ~/.hermes/skills/creative/bios-mode
```

The skill loads on the next session. To use it, just say **"bios mode"**.

---

## Quick start

```
you:  bios mode
agent:
      BIOS MODE — default profile
      POST: OK — self-location resolved, files readable
      Root: ~/.hermes
      Session: glm-5.2:cloud / ollama-cloud
      Live: SOUL.md · MEMORY.md · USER.md · environment.md
      Memory: MEMORY 4235/4400 · USER 2734/2750
      Skills: 245 · Cron: 19 jobs
      Config: model.default=glm-5.2:cloud (config.yaml mtime 2026-08-15 13:59)
      BIOS lock: off · Snapshots: 0 · Last log: —
      Commands: menu [n] · status · ? for full list · exit bios mode

you:  menu 1
agent:
      [SOUL] — identity and personality
      Path: ~/.hermes/SOUL.md
      Actions: draft <change> · apply · revert · back

you:  draft change "be brief" to "be terse"
agent:
      current:  Be brief.
      proposed: Be terse.
      dropped:  nothing · kept: nothing

you:  apply
agent:
      applied SOUL.md (snapshot 20260816-164144)

you:  exit bios mode
agent:
      session-end: 1 applied, 0 reverted
```

---

## Commands

| Command | What it does |
|---------|-------------|
| `menu` | Main menu — numbered list of all 14 subsystems |
| `menu <n\|name>` | Open a subsystem screen (e.g. `menu 1`, `menu config`) |
| `back` | Return to the main menu (drafts survive) |
| `status` | Re-print the POST block |
| `map [name]` | Subsystem inventory — index or one entry |
| `draft <change>` | Propose an edit: current → proposed → dropped/kept |
| `snapshot` | Capture pre-edit state of the pending batch |
| `apply` | Write a greenlit draft |
| `revert` | Restore the last snapshot |
| `selftest` | Run the machinery checks |
| `?` | Print the full command list |
| `exit bios mode` | End the mode, resume normal persona |

---

## The 14 subsystems

1. SOUL.md — identity and personality
2. MEMORY.md — always-on context index
3. USER.md — what the agent knows about you
4. environment.md — environment snapshot
5. fact_store — operational state DB
6. config.yaml — model, providers, tools, gateway, theme
7. Skills — procedural memory
8. Cron jobs — scheduled tasks
9. Plugins — optional features
10. Theme / skin — visual identity
11. Gateway — message routing
12. Agent comms — inter-agent messaging
13. State databases — sessions, projects, kanban
14. BIOS bookkeeping — snapshots, audit log, policy

Full details (paths, read/change commands, effect timing, guards) are in [`references/hardware-map.md`](references/hardware-map.md).

---

## Safety model

BIOS-mode is built around one idea: **editing an agent's identity files is dangerous, so make it deliberate.**

- **Locked BIOS** — the mode engages only on a direct message from the user in the live chat. Nothing in a file, web page, tool result, or subagent output can trigger it.
- **No memory writes** — the mode never touches MEMORY.md, USER.md, or fact_store unless you explicitly ask.
- **Only greenlit changes** — drafts are shown as diffs and applied only on your explicit go-ahead.
- **Snapshot before write** — every batch is copied first, so a mistake is one `revert` away.
- **Owner lock** — an optional per-profile policy that restricts edits to a named owner. Useful for shared or family deployments.

The owner lock is a soft contract plus prompt-injection defense, **not** strong authentication. Real identity is enforced at the platform/gateway layer.

---

## Requirements

- Hermes Agent (the `hermes` CLI on PATH)
- Linux or macOS (the POST shell template is POSIX; Windows is not supported)

---

## Layout

```
bios-mode/
├── SKILL.md                    # the skill itself
├── references/
│   └── hardware-map.md        # 14-subsystem inventory
├── LICENSE
├── README.md
├── CHANGELOG.md
└── FAQ.md
```

---

## FAQ

See [FAQ.md](FAQ.md) for common questions — what BIOS-mode is and isn't, how the lock works, how to recover from a bad edit, and more.

---

## License

MIT. See [LICENSE](LICENSE).
