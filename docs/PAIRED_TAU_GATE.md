# Paired τ gate (harness / MCP exposure)

Fleet policy for `AppSprout-dev/grok-skills`. Greenlit 2026-09-22.

A candidate that changes agent–tool exposure is admitted only when the same frozen replay \(\mathcal{V}\) scores the parent and the candidate at the same decode and tool budget, and \(\Delta = \phi(\mathcal{L}',\mathcal{V}) - \phi(\mathcal{L},\mathcal{V}) \geq \tau\). Anything short of that is rejected and logged. Ungated Tool edits are banned.

This gate covers harness and MCP exposure. It is not a path for writing CEM DNA.

## When it applies

Fill the four PR fields and attach evidence when a pull request changes what an agent can call or how that call is exposed:

- A Grok Bot skill (`plugins/**/skills/**`, or a copied agent/skill template that defines tool use)
- An MCP schema or session manifest (tool names, arguments, required fields)
- Plugin surface (`plugin.json`, or a `.grok-plugin/marketplace.json` entry that adds, removes, or redefines a plugin)

A docs typo, or any other diff that does not change exposure, uses the exempt `paired_eval_Δ` value below and still points at evidence that the diff leaves exposure unchanged.

One level per PR. A Tool PR does not also rewrite DNA.

## Required PR fields

Use these names exactly. Templates:

- `.github/PULL_REQUEST_TEMPLATE.md` (default)
- `.github/PULL_REQUEST_TEMPLATE/skill-mcp-exposure.md`

| Field | Legal value |
|---|---|
| `level` | `content` \| `tool` \| `schema` |
| `hypothesis` | short string |
| `dna_touch` | `true` or `false` |
| `paired_eval_Δ` | a JSON number, exactly `N/A (exempt: docs typo / no exposure change)`, or `BLOCKED` |

`evidence` is the pointer that accompanies `paired_eval_Δ` (artifact path or URL). It is required for every value. A number is an admit only when \(\Delta \geq \tau\). The exempt string is an admit only when exposure is unchanged. `BLOCKED` is not an admit (TODO(Petra) still open, or the paired run was not done).

### How a PR is routed

- `dna_touch: false` is required for a bot-mergeable Tool PR.
- `dna_touch: true` is human + physics tests only. Bot merge is auto-blocked. The evolution loop stays off. Log the attempt and hand it to a human. Do not retry it as a bot Tool PR.
- `level: tool` and `dna_touch: false` goes through this gate. Bot merge is allowed only when evidence shows \(\Delta \geq \tau\) on frozen \(\mathcal{V}\).
- `level: schema` is in scope only for an MCP or plugin tool schema, with `dna_touch: false`, and the same gate. Bend LAWS/PROOF, Jev schemas, and CEM DNA schemas are a hard refuse.
- `level: content` that writes CEM DNA, scorer weights, golden recipe ids, or physics constants is a hard refuse. Content evolution does not write CEM DNA.
- `level: content` with no exposure change uses the exempt string.
- `level: content` whose skill text changes tool exposure is an exposure change: the gate applies, and the DNA hard wall still applies.
- A Tool or exposure Schema PR with no numeric \(\Delta\), a failed evidence pointer, or \(\Delta < \tau\) is an ungated Tool edit. Do not bot-merge it. Log a reject when a bot tries to merge it, or when a scored candidate fails τ. A human PR marked `BLOCKED` while TODO(Petra) is open stays unmerged and does not get a reject line until a merge is attempted.

## Frozen \(\mathcal{V}\), τ, and the primary metric

TODO(Petra): the primary metric is not locked. Petra owns the Challenge 0 replay. This repository does not invent that slice, a numeric τ, or any CEM DNA.

Metrics Petra may lock (name only, not a scored definition):

- turns-to-correct-tool-use
- wrap-docs honesty checklist score

Parent and candidate share \(\mathcal{V}\), the decode, and the tool-call budget. Admit only if \(\Delta \geq \tau\).

| Placeholder | Status |
|---|---|
| τ | TODO(Petra). No number in this repository is τ. |
| \(\mathcal{V}\) path | TODO(Petra). `petra-campaign/replay/c0/V` is a path placeholder. The frozen held-out C0 trajectory slice is not stored here and must not be reconstructed here. |
| Primary metric | TODO(Petra). turns-to-correct-tool-use and/or wrap-docs honesty. |

Until those TODOs close, an exposure-changing PR has no legal numeric `paired_eval_Δ`. Write `BLOCKED` in that field, point `evidence` at the reason, and leave the PR open. `BLOCKED` is not an admit and is not bot-mergeable. Do not use the exempt string unless exposure is unchanged. Do not invent a score so the field parses as a pass.

The PR that introduces this document is `BLOCKED` for the same reason. Review may accept that documentation after reading it, without a numeric Δ. That acceptance is not a precedent. Every later exposure PR stays `BLOCKED` and unmerged until TODO(Petra) locks τ, \(\mathcal{V}\), and the metric, and the PR shows \(\Delta \geq \tau\).

## Evidence artifact

A numeric admit points at a parent-vs-candidate record: \(\mathcal{V}\) id (once Petra names it), metric name, parent score, candidate score, \(\Delta\), shared budget, and a digest of the skill or MCP diff. Keep that artifact outside CEM DNA (PR attachment or campaign artifact).

An exempt PR points at the diff, or a short note, showing the change is a docs typo or otherwise leaves tool exposure unchanged.

## Reject log

Append-only log: `docs/gate-rejects/paired-tau.jsonl`. One JSON object per line. Do not delete or rewrite prior lines. The file may be empty until the first reject. A `BLOCKED` human policy PR waiting on TODO(Petra) is not itself a reject row. A bot attempt to merge that PR, or any refused candidate, is a row.

Keys:

| Key | Value |
|---|---|
| `ts` | ISO-8601 timestamp |
| `pr` | pull request URL, or `n/a` |
| `level` | `content` \| `tool` \| `schema` |
| `hypothesis` | the PR hypothesis string |
| `dna_touch` | bool |
| `delta` | number, or `null` |
| `tau` | `null` until TODO(Petra) locks τ |
| `v_id` | `TODO(Petra)` until the frozen slice is named |
| `metric` | `TODO(Petra)`, or the locked metric name |
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

Also refused, with no τ exception:

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
3. TODO(Petra) has locked τ, \(\mathcal{V}\), and the primary metric.
4. `paired_eval_Δ` is a number, \(\Delta \geq \tau\), and `evidence` resolves to the parent/candidate artifact.
5. The hard wall is clear.
6. The reject log has no refuse row for this diff.
