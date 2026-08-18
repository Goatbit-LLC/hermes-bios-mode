# BIOS Hardware Map

Legend per subsystem: Path · Read · Change · Applies · Guards.
`<root>` = your profile root from the skill's "Locating your own files" section (default `~/.hermes`, named `~/.hermes/profiles/<profile>`). `<profile>` = the active profile name.
Everything below is about YOUR active profile. Other profiles are off-limits unless the user says otherwise. All commands were verified against the `hermes` CLI (on PATH as `hermes`).

## 1. SOUL.md — identity and personality
- Path: `<root>/SOUL.md`
- Read: `read_file` (it is also injected into your system prompt every session)
- Change: search_files → read context → `patch` with exact strings, show diff, verify after
- Applies: live, next message or session. No restart.
- Guards: never edit `.bak.*`, `archive/`, or session-dump copies; only the resolved live file counts.

## 2. MEMORY.md — always-on context index
- Path: `<root>/memories/MEMORY.md`
- Read: `read_file`; the injected header already shows current/limit usage
- Change: `memory` tool (target=memory) preferred; `patch` if it's an exact rewrite
- Applies: next turn
- Guards: 4,400-char budget; the periodic audit script relocates bloat. In BIOS mode, only on explicit ask (Rules 2).

## 3. USER.md — what you know about the user
- Path: `<root>/memories/USER.md`
- Read: `read_file`; injected header shows usage
- Change: `memory` tool (target=user)
- Applies: next turn
- Guards: 2,750-char budget. In BIOS mode, only on explicit ask.

## 4. environment.md — environment snapshot
- Path: default profile `<root>/environment.md`; named profiles `<root>/memories/environment.md`
- Read: at session start; `read_file` any time
- Change: `patch` / `write_file`
- Applies: next session start
- Guards: holds hosts, IPs, services, SSH details. Keep it current; secrets belong in `.env`, never here.

## 5. fact_store — operational state DB
- Path: backed by `~/.hermes/memory_store.db` (shared across profiles)
- Read/Change: always via the `fact_store` tool (add, probe, search, reason, update, remove)
- Applies: immediately; facts are trust-scored and trained by `fact_feedback`
- Guards: never open the DB with sqlite while operating. In BIOS mode, only on explicit ask.

## 6. config.yaml — model, providers, tools, gateway, theme
- Path: `<root>/config.yaml`
- Read: `hermes config get <key>` or `read_file`
- Change: `hermes config set <key> <value>` preferred; hand-edits must pass YAML validation first
- Applies: most keys after the gateway/service restarts ("reboot required"); some are read per session (e.g. `model.default` picks the session model)
- Guards: profile configs are mode 600 with a `.bak` beside them. Plaintext secrets belong in `.env` (`hermes config env-path` shows where), never in config.yaml.

## 7. Skills — procedural memory
- Path: `<root>/skills/` (category subdirs, one dir per skill)
- Read: `skills_list`, `skill_view <name>`
- Change: `skill_manage` (create, patch, edit, delete; `write_file` for references)
- Applies: after the skill index refreshes — most next session, some same session
- Guards: a new skill's description must fit the 60-char index budget, trigger first. Only the current profile's skills.

## 8. Cron jobs — scheduled tasks
- Path: tool-managed, data under `<root>/cron/` (jobs.json, executions.db, output/)
- Read/Change: the `cronjob` tool, or `hermes cron list|create|edit|pause|resume|remove|run`
- Applies: on their schedule
- Guards: creating or modifying cron is an ask-first action per the SOUL boundaries. BIOS mode never schedules jobs without explicit user request.

## 9. Plugins — optional features
- Path: `<root>/plugins/` (list the dir; plugins vary by install)
- Read: list the dir; docs live under each plugin
- Change: `hermes plugins` subcommand; most need a restart to load
- Applies: after restart
- Guards: don't hand-edit plugin dirs while the service is live unless explicitly asked.

## 10. Theme / skin — visual identity
- Path: config keys `skin` and `theme` (both currently `default`); some profiles keep a `<root>/skins/` dir
- Read: `hermes skin list` or `hermes config get theme`
- Change: `hermes skin use <name>` / `hermes skin set <color> <value>` / `hermes config set`
- Applies: after restart ("reboot required")
- Guards: never swap a theme without the draft-approve discipline; respect the user's stated aesthetic preference.

## 11. Gateway — message routing
- Path: `<root>/gateway/` (state files), plus `<root>/gateway_state.json`
- Read: state files, or `hermes status`
- Change: restart via the gateway service (`hermes gateway`); do not hand-edit state files while it is live
- Applies: after restart
- Guards: infrastructure — in BIOS mode, inspect only unless the user explicitly requests a change.

## 12. Agent comms — inter-agent messaging
- Path: keys at `~/.hermes/agent-comms/keys/<agent>.key` (32-byte raw Ed25519 private, mode 0600) + `.pub`
- Read/Change: the agent-comms MCP tools (send_message, check_inbox, register_agent, heartbeat)
- Applies: immediately
- Guards: private keys are secret. Never paste, read, or log key contents — 0600 is load-bearing.

## 13. State databases — sessions, projects, kanban
- Path: `<root>/state.db` (sessions/context), `kanban.db`, `projects.db`, `verification_evidence.db`
- Read/Change: strictly through their tools (`session_search` for sessions, project/kanban tools for the rest)
- Applies: n/a
- Guards: never open these with sqlite during operation; they are live system data.

## 14. BIOS bookkeeping — snapshots, audit log, policy
- Path: `<root>/bios/` (snapshots/, bios.log, policy.md, selftest/); README there explains it all
- Read: the audit log and policy; `ls` the snapshots dir
- Change: maintained by bios-mode itself via `snapshot`, `revert`, and on-session-end logging
- Applies: immediately (own bookkeeping, needs no approval)
- Guards: dir mode 0700 / files 0600. It stores prior state, never new memory; the audit log is one line per event, not a diary.

## Never list
- No raw sqlite against live DBs; no edits to another profile's root; no secret material in anything BIOS mode writes; no cron or gateway changes without explicit user request.
- If POST shows a subsystem missing or unreadable, stop and report instead of working around it.
- `hermes doctor` exists and is the first tool for diagnosing config/health problems at any point in a BIOS session.
