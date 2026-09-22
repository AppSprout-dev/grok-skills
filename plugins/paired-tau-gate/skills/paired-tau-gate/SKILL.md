---
name: paired-tau-gate
description: Use before opening a pull request that changes a Grok Bot skill, an MCP schema or manifest, or plugin surface. Requires level, hypothesis, dna_touch, and paired_eval_Δ with an evidence pointer. Trigger on skill PR, MCP schema change, plugin surface edit, or agent tool exposure.
metadata:
  type: workflow
  version: "1.0.0"
  author: AppSprout
---

# Paired τ gate

## Purpose

Stop ungated edits to agent–tool exposure. Load this skill before opening a PR that changes a Grok Bot skill, an MCP schema or manifest, or a plugin surface. The policy text is `docs/PAIRED_TAU_GATE.md` in `AppSprout-dev/grok-skills`. Copy its four fields into the PR body. This skill does not score the replay and does not write CEM DNA.

## Fields (exact names)

```yaml
level: # content | tool | schema
hypothesis: # short string
dna_touch: # false | true
paired_eval_Δ: # <number> OR N/A (exempt: docs typo / no exposure change) OR BLOCKED
evidence: # path or URL
```

- One `level` only: `content`, `tool`, or `schema`.
- `hypothesis` is one short sentence.
- `dna_touch` is `false` on any Tool PR a bot might merge. `true` means human + physics only, and bot merge is blocked.
- `paired_eval_Δ` is a number only after parent and candidate run on the same frozen replay V, same budget, and Δ ≥ τ. The exempt string is exactly `N/A (exempt: docs typo / no exposure change)` and only when exposure does not change. Otherwise write `BLOCKED`.
- `evidence` points at the artifact, or at the note that explains `BLOCKED` or the exemption.

## Admit rule

Bot merge needs `level: tool` (or MCP/plugin `schema` only), `dna_touch: false`, a numeric `paired_eval_Δ` with Δ ≥ τ, and a resolving `evidence` pointer. τ, V, and the primary metric are TODO(Petra): turns-to-correct-tool-use and/or wrap-docs honesty, on a frozen Petra C0 replay. No number in this repo is τ. The path `petra-campaign/replay/c0/V` is a placeholder. Do not invent the slice or a score.

`BLOCKED` stays open for a human. A missing number, a broken pointer, or Δ < τ is an ungated Tool edit: do not merge; append a line to `docs/gate-rejects/paired-tau.jsonl` using the schema in the policy doc.

## Refused work

Do not open, and do not "gate", a PR that would:

- Write CEM DNA, scorer weights, golden recipe ids, Bend LAWS/PROOF, or Jev schemas
- Add a fintech (Sovereign / AlphaForge) or CannaSage ontology
- Vendor or port an EvoOntology plugin
- Paste CEM DNA or a PRD body into a skill, agent prompt, or PR (path pointers only; see the semantic manifest template)

A `level: content` PR that writes CEM DNA is a refuse, not an evolution candidate. Set `dna_touch: true` only to hand real DNA work to a human and physics tests. Leave the evolution loop off.
