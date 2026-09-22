## Skill / MCP exposure

Policy: `docs/PAIRED_TAU_GATE.md`. Required when this PR changes a Grok Bot skill, an MCP schema or manifest, or plugin surface. Ungated Tool edits are rejected and logged.

```yaml
level: # content | tool | schema
hypothesis: # short string
dna_touch: # false | true
paired_eval_Δ: # <number> OR N/A (exempt: docs typo / no exposure change) OR BLOCKED
evidence: # path or URL
```

- `level` is exactly one of `content`, `tool`, `schema`.
- `hypothesis` is one short sentence.
- `dna_touch: true` blocks bot merge (human + physics only). Bot-mergeable Tool PRs set `false`.
- `paired_eval_Δ` is a JSON number only when parent and candidate were scored on frozen V and Δ ≥ τ. The exempt value is exactly `N/A (exempt: docs typo / no exposure change)` and only when exposure does not change. `BLOCKED` means V/τ are still TODO(Petra) or the run was not done: the PR is not an admit and is not bot-mergeable.
- `evidence` points at the parent/candidate artifact, or at the diff note for an exempt PR. For `BLOCKED`, point at why no score exists.

Default template with the same fields: `.github/PULL_REQUEST_TEMPLATE.md`.
