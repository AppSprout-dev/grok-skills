# Paired τ gate (harness / MCP exposure)

Fleet policy for `AppSprout-dev/grok-skills`. Greenlit 2026-09-22. Petra confirmed the Challenge 0 harness replay defaults below. Those defaults are harness-admit only. They are not chemistry τ, not a scorer weight, and not CEM DNA.

A candidate that changes agent–tool exposure is admitted only when the parent and the candidate run on the same frozen replay \(\mathcal{V}\), at the same decode and tool budget, and the paired result meets \(\tau_{harness}\). Anything short of that is rejected and logged. Ungated Tool edits are banned.

This gate covers harness and MCP exposure. It is not a path for writing CEM DNA.

## When it applies

Fill the required PR fields and attach evidence when a pull request changes what an agent can call or how that call is exposed:

- A Grok Bot skill (`plugins/**/skills/**`, or a copied agent/skill template that defines tool use)
- An MCP schema or session manifest (tool names, arguments, required fields)
- Plugin surface (`plugin.json`, or a `.grok-plugin/marketplace.json` entry that adds, removes, or redefines a plugin)

A docs typo, or any other diff that does not change exposure, uses the exempt `paired_eval_Δ` value below and still points at evidence that the diff leaves exposure unchanged.

One level per PR. A Tool PR does not also rewrite DNA.

## PR fields

Use these names exactly. Templates:

- `.github/PULL_REQUEST_TEMPLATE.md` (default)
- `.github/PULL_REQUEST_TEMPLATE/skill-mcp-exposure.md`

Required:

| Field | Legal value |
|---|---|
| `level` | `content` \| `tool` \| `schema` |
| `hypothesis` | short string |
| `dna_touch` | `true` or `false` |
| `paired_eval_Δ` | a JSON number, exactly `N/A (exempt: docs typo / no exposure change)`, or `BLOCKED` |

Optional fleet parity:

| Field | Legal value |
|---|---|
| `expected_effect` | short string. Petra JUMP cards require it. This fleet records it so a harness PR and a JUMP card name the same expected effect. It is not an admit signal. |

`evidence` is the pointer that accompanies `paired_eval_Δ` (artifact path or URL). It is required for every value of `paired_eval_Δ`. A number is the primary \(\Delta\) (defined below) and admits only under \(\tau_{harness}\). The exempt string admits only when exposure is unchanged. `BLOCKED` is not an admit: the frozen fixture is not yet published, or the paired run was not done. Do not invent a score so the field parses as a pass.

### How a PR is routed

- `dna_touch: false` is required for a bot-mergeable Tool PR.
- `dna_touch: true` is human + physics tests only. Bot merge is auto-blocked. The evolution loop stays off. Log the attempt and hand it to a human. Do not retry it as a bot Tool PR.
- `level: tool` and `dna_touch: false` goes through this gate. Bot merge is allowed only when evidence shows the \(\tau_{harness}\) rule on frozen \(\mathcal{V}\).
- `level: schema` is in scope only for an MCP or plugin tool schema, with `dna_touch: false`, and the same gate. Bend LAWS/PROOF, Jev schemas, and CEM DNA schemas are a hard refuse.
- `level: content` that writes CEM DNA, scorer weights, golden recipe ids, or physics constants is a hard refuse. Content evolution does not write CEM DNA.
- `level: content` with no exposure change uses the exempt string.
- `level: content` whose skill text changes tool exposure is an exposure change: the gate applies, and the DNA hard wall still applies.
- A Tool or exposure Schema PR with no numeric \(\Delta\), a failed evidence pointer, or a result that misses \(\tau_{harness}\) is an ungated Tool edit. Do not bot-merge it. Log a reject when a bot tries to merge it, or when a scored candidate misses \(\tau_{harness}\). A human PR marked `BLOCKED` while the Petra fixture is still a TODO path stays unmerged and does not get a reject line until a merge is attempted.

## Frozen \(\mathcal{V}\)

Repo: `AppSprout-dev/Petra`. Exact location: `evals/c0_harness_v/`.

Contents, once that tree is merged: `V.jsonl` and/or `manifest.json`, plus `README.md`.

Status: TODO path. The location above is the confirmed path. The fixture is not in this repository. Until the Petra change that adds `evals/c0_harness_v/` is undrafted and merged, do not treat the directory as published, and do not reconstruct the tasks here.

After that merge, the fixture is frozen. A task change takes a SemVer bump of the fixture. Parents and candidates in one paired run use the same frozen version.

The exact scoring procedure lives in Petra `evals/c0_harness_v/README.md` once that file is published. This document records the defaults Petra confirmed for harness admit. It does not replace that README, and it does not define a chemistry τ.

## Metrics and \(\tau_{harness}\)

Primary metric: `turns_to_correct_mcp_use`. Lower is better.

Primary paired \(\Delta\) = parent − candidate, in turns. Ship on the primary when \(\Delta \geq \tau_{harness}\).

Default \(\tau_{harness}\) (harness-admit only): **1.0** turn. Admit on the primary when \(\Delta \geq 1.0\).

Secondary metric: wrap-docs honesty checklist pass-fraction. Higher is better. Honesty \(\Delta\) = candidate − parent.

If the primary ties (\(\Delta = 0\)), admit when honesty \(\Delta \geq 0.10\). A primary \(\Delta\) strictly between 0 and 1.0 is a miss, not a tie, and the honesty bar does not replace it.

Parent and candidate share \(\mathcal{V}\), the decode, and the tool-call budget.

| Name | Value |
|---|---|
| Primary metric | `turns_to_correct_mcp_use` (lower better) |
| Primary \(\Delta\) | parent − candidate |
| \(\tau_{harness}\) primary | 1.0 turn |
| Secondary metric | wrap-docs honesty checklist pass-fraction |
| Tie bar | honesty \(\Delta \geq 0.10\) only when primary \(\Delta = 0\) |
| \(\mathcal{V}\) | TODO path `evals/c0_harness_v/` on `AppSprout-dev/Petra` until the undrafted PR merges |

`1.0` and `0.10` are harness-admit thresholds for this gate. They are not chemistry τ and not scorer weights.

Until `evals/c0_harness_v/` is merged and frozen, an exposure-changing PR has no published V to score. Write `BLOCKED` in `paired_eval_Δ`, point `evidence` at this TODO path, and leave the PR open. `BLOCKED` is not bot-mergeable.

The PR that introduces this document is `BLOCKED` because that fixture is not merged. Review may accept this documentation without a numeric \(\Delta\). That acceptance is not a precedent. Every later exposure PR stays `BLOCKED` and unmerged until the fixture is frozen and the PR shows the \(\tau_{harness}\) rule.

## Evidence artifact

A numeric admit points at a parent-vs-candidate record: fixture version under `evals/c0_harness_v/`, primary metric `turns_to_correct_mcp_use`, parent turns, candidate turns, primary \(\Delta\), and, when the primary ties, honesty pass-fractions and honesty \(\Delta\). Include the shared budget and a digest of the skill or MCP diff. Keep that artifact outside CEM DNA (PR attachment or campaign artifact).

An exempt PR points at the diff, or a short note, showing the change is a docs typo or otherwise leaves tool exposure unchanged.

## Reject log

Append-only log: `docs/gate-rejects/paired-tau.jsonl`. One JSON object per line. Do not delete or rewrite prior lines. The file may be empty until the first reject. A `BLOCKED` human policy PR waiting on the Petra fixture is not itself a reject row. A bot attempt to merge that PR, or any refused candidate, is a row.

Keys:

| Key | Value |
|---|---|
| `ts` | ISO-8601 timestamp |
| `pr` | pull request URL, or `n/a` |
| `level` | `content` \| `tool` \| `schema` |
| `hypothesis` | the PR hypothesis string |
| `dna_touch` | bool |
| `delta` | primary \(\Delta\) (parent − candidate on `turns_to_correct_mcp_use`), or `null` |
| `tau` | `1.0` (\(\tau_{harness}\) primary). This is harness-admit only, not chemistry τ |
| `v_id` | `evals/c0_harness_v/` plus fixture SemVer once frozen; until the Petra merge, `TODO:evals/c0_harness_v/` |
| `metric` | `turns_to_correct_mcp_use` |
| `reason` | `missing_evidence` \| `delta_below_tau` \| `ungated_tool` \| `dna_touch` \| `hard_wall` \| `exempt_misuse` |
| `actor` | `bot` \| `human` |

Log lines carry signatures only. They do not carry DNA payloads, scorer weights, recipe ids, or PRD bodies.

## Hard wall

This gate does not authorize writes to:

- CEM DNA (Petra, Phenara, Hygra, Torquon, or any other CEM)
- Scorer weights
- Golden recipe ids
- Bend LAWS/PROOF
- Jev schemas

Harness and MCP-skill exposure only. Also refused, with no \(\tau_{harness}\) exception:

- Fintech ontology (Sovereign / AlphaForge) and CannaSage ontology
- A vendor EvoOntology plugin drop-in. This marketplace does not vendor or port an upstream EvoOntology plugin
- A free-form bot PR whose request is to rewrite an ontology or CEM DNA
- Online mutation of tool exposure during a campaign. The check is an offline PR comparison on frozen \(\mathcal{V}\)

Evals and probes check docs and tool-contract mappings. They leave locked DNA unchanged.

## Templates stay pointers

Process skills and harness templates in this repo describe how to build. They point at project files and at `.grok/semantic-manifest.md`. They do not embed a semantic layer. Audit record: `docs/TEMPLATE_DNA_AUDIT.md`.

## Bot merge

Bot merge requires every item below. Otherwise the PR stays open for a human.

1. `level` is `tool`, or `schema` limited to an MCP or plugin tool schema.
2. `dna_touch` is `false`.
3. Frozen \(\mathcal{V}\) is the merged Petra fixture at `evals/c0_harness_v/` (same SemVer for parent and candidate).
4. `paired_eval_Δ` is the primary \(\Delta\) (parent − candidate on `turns_to_correct_mcp_use`) and \(\Delta \geq 1.0\), or the primary ties (\(\Delta = 0\)) and honesty \(\Delta \geq 0.10\). `evidence` resolves to that parent/candidate artifact.
5. The hard wall is clear.
6. The reject log has no refuse row for this diff.
