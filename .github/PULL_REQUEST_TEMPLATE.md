## Skill / MCP exposure

Policy: `docs/PAIRED_TAU_GATE.md`. Use this block on every PR. Exposure changes (Grok Bot skill, MCP schema or manifest, plugin surface) need a paired parent/candidate result on frozen replay V at `evals/c0_harness_v/` (AppSprout-dev/Petra). Ungated Tool edits are rejected and logged.

```yaml
level: # content | tool | schema
hypothesis: # short string
dna_touch: # false | true
paired_eval_Δ: # <number> OR N/A (exempt: docs typo / no exposure change) OR BLOCKED
evidence: # path or URL
expected_effect: # optional short string; Petra JUMP cards require it
```

- `level`, `hypothesis`, `dna_touch`, and `paired_eval_Δ` are required. `expected_effect` is optional fleet parity with Petra JUMP cards. It does not admit the PR.
- `level` is exactly one of `content`, `tool`, `schema`.
- `hypothesis` is one short sentence.
- `dna_touch: true` blocks bot merge (human + physics only). Bot-mergeable Tool PRs set `false`.
- `paired_eval_Δ` is the primary Δ on `turns_to_correct_mcp_use`: parent − candidate (lower is better). A number admits only when Δ ≥ τ_harness. Default τ_harness is harness-admit only: **1.0** turn. If the primary ties (Δ = 0), the secondary wrap-docs honesty pass-fraction must improve by ≥ **0.10** (candidate − parent). These are not chemistry τ. The exact rule lives in Petra `evals/c0_harness_v/README.md`.
- The exempt value is exactly `N/A (exempt: docs typo / no exposure change)` and only when exposure does not change.
- `BLOCKED` means the paired run was not done. The fixture at `evals/c0_harness_v/` is merged and frozen (SemVer **1.0.0**, Petra merge `9a149ba3260c7c05c937f13884870f8ef9573d2f`). `BLOCKED` is not an admit and is not bot-mergeable.
- `evidence` for a scored run points at the parent/candidate artifact for fixture SemVer **1.0.0** under `evals/c0_harness_v/` (Petra merge `9a149ba3…`). An exempt PR points at the diff note. For `BLOCKED`, point at a note that the paired run was not done.

Same fields: `.github/PULL_REQUEST_TEMPLATE/skill-mcp-exposure.md`.
