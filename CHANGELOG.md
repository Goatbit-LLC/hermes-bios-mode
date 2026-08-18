# Changelog

## 1.0.0 — 2026-08-16

First public release. Everything prior was beta.

- Profile-agnostic self-location (default vs named profiles).
- POST status block on entry, gathered in a single batched call.
- Hardware map: 14-subsystem inventory with real read/change commands.
- Menu navigation: `menu`, `menu <n|name>`, `back`, `status`, `?`.
- Draft → approve → apply discipline; nothing written without explicit go-ahead.
- Snapshot / revert with manifest and audit log.
- Owner lock (optional per-profile policy).
- Self-test that verifies the mode's own machinery.
- Terse, flat response format; multi-line outputs fenced for correct rendering.
- MIT license.
