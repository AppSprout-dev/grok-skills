# Changelog

## Unreleased

### Policy

- Paired τ gate for skill, MCP, and plugin exposure PRs (`docs/PAIRED_TAU_GATE.md`). Required PR fields: `level`, `hypothesis`, `dna_touch`, `paired_eval_Δ`. Optional: `expected_effect`.
- Petra C0 harness defaults (harness-admit only, not chemistry τ): primary `turns_to_correct_mcp_use`, Δ = parent − candidate, τ_harness 1.0 turn; honesty pass-fraction Δ ≥ 0.10 on a primary tie. V is frozen at Petra `evals/c0_harness_v/` fixture SemVer 1.0.0, merge `9a149ba3260c7c05c937f13884870f8ef9573d2f`.
- `paired-tau-gate` skill: fill those fields before opening an exposure PR.
- Reject log path: `docs/gate-rejects/paired-tau.jsonl` (empty until the first refuse).

### Skills

- Add `yield-framework` process skill (check|audit|new) — Grok packaging of AppSprout-dev/yield-framework / jkbennitt/yield-framework. No Claude slash commands or PostToolUse hooks.
- Template DNA audit (`docs/TEMPLATE_DNA_AUDIT.md`): process skills did not paste DNA or PRD bodies. Added a semantic-manifest pointer so copied harness files stay paths.

## 1.0.0 — 2026-08-26

First public release of the AppSprout Grok Build marketplace.

### Plugins

- `cem-design-process` — foundational-first CEM design process
- `greenfield-research-platform` — Grok Build campaign host + arm workers, two-process split, Challenge 0 loop

### Notes

Process skills only. Project-context skills stay private.
Pin installs to tag `v1.0.0` when you need a frozen SHA.
