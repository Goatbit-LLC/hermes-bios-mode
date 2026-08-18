# Contributing to Hermes BIOS-mode

Thanks for your interest. This is a small, focused skill — contributions are welcome and easy to review.

## What we're looking for

- **Bug fixes** — a real symptom, reproduced, with the fix and a note on how you verified it.
- **New subsystems** — if Hermes grows a tunable surface the hardware map doesn't cover, add it with the same shape: Path · Read · Change · Applies · Guards.
- **Better commands** — if a `hermes` CLI command changed or a better read/change path exists, update the map.
- **Docs** — clearer README, FAQ entries for real questions, website copy.

## What we're not looking for

- **Personality.** BIOS-mode is deliberately flat and terse. Don't add warmth, emoji, or conversational flourishes.
- **Speculative features.** No new commands or subsystems without a concrete use case.
- **Windows support hacks.** The POST shell template is POSIX by design. A proper Windows port is a separate, larger effort.

## Conventions

- **Versioning** — bump `version` in SKILL.md frontmatter and add a CHANGELOG entry under the current unreleased section.
- **Tone** — the skill speaks to the *agent*, not the end user. Imperative, literal, no filler.
- **Paths** — use `<root>` and `<profile>` placeholders, never hardcoded absolute paths.
- **No personal references** — no names, IPs, hostnames, or machine-specific plugin lists.

## How to contribute

1. Fork the repo.
2. Make your change in a branch.
3. Run `selftest` in a bios-mode session to confirm the machinery still works.
4. Open a PR with a clear description of the problem and the fix.

## Verification checklist

- [ ] `version` bumped and CHANGELOG updated
- [ ] No hardcoded paths or personal references
- [ ] Tone is flat and terse
- [ ] `selftest` passes
- [ ] README/FAQ updated if behavior changed

## License

By contributing, you agree your work is licensed under the MIT license (see LICENSE).
