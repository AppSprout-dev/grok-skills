# Changelog

## Unreleased

### Policy

- Paired τ gate for skill, MCP, and plugin exposure PRs (`docs/PAIRED_TAU_GATE.md`). PR fields: `level`, `hypothesis`, `dna_touch`, `paired_eval_Δ`.
- `paired-tau-gate` skill: fill those fields before opening an exposure PR.
- Reject log path: `docs/gate-rejects/paired-tau.jsonl` (empty until the first refuse).

### Skills

- Template DNA audit (`docs/TEMPLATE_DNA_AUDIT.md`): process skills did not paste DNA or PRD bodies. Added a semantic-manifest pointer so copied harness files stay paths.

## 1.0.0 — 2026-08-26

First public release of the AppSprout Grok Build marketplace.

### Plugins

- `cem-design-process` — foundational-first CEM design process
- `greenfield-research-platform` — Grok Build campaign host + arm workers, two-process split, Challenge 0 loop

### Notes

Process skills only. Project-context skills stay private.
Pin installs to tag `v1.0.0` when you need a frozen SHA.
