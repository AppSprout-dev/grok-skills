# Template DNA paste audit (2026-09-22)

Scope: `plugins/**/skills/**` and the greenfield harness assets/templates, plus `cem-design-process`. Question: does any skill or template paste a full CEM DNA blob or founding PRD body?

Result: no file pasted CEM DNA, a PRD body, scorer weights, golden recipe ids, Bend LAWS/PROOF, or Jev schemas. Process how-to stayed. No static semantic-layer dump was present to delete.

Replacement applied anyway: copied harness files now carry a manifest-pointer pattern so a later edit cannot turn them into a dump. The pointer file is `plugins/greenfield-research-platform/skills/greenfield-research-platform/assets/semantic-manifest.md.template` (installed as `.grok/semantic-manifest.md`). It lists paths. It does not contain DNA.

| Path | Pasted DNA or PRD body? | Change |
|---|---|---|
| `plugins/cem-design-process/skills/cem-design-process/SKILL.md` | No. Process steps. "DNA" is a design-order rule, not a payload. | Pointer rule added. |
| `plugins/greenfield-research-platform/skills/greenfield-research-platform/SKILL.md` | No. Process steps and file roles. | Manifest added to the copy list and file tree. |
| `plugins/greenfield-research-platform/skills/greenfield-research-platform/assets/AGENTS.md.template` | No. Points at `docs/PRD.md` and `docs/ARCHITECTURE.md`. | Points at `.grok/semantic-manifest.md`. |
| `plugins/greenfield-research-platform/skills/greenfield-research-platform/assets/campaign-agent.md.template` | No. Host procedure. | On-demand manifest read. |
| `plugins/greenfield-research-platform/skills/greenfield-research-platform/assets/research-agent.md.template` | No. Worker procedure. | On-demand manifest read. |
| `plugins/greenfield-research-platform/skills/greenfield-research-platform/assets/cycle-skill.md.template` | No. Cycle procedure and ARM REPORT shape. | On-demand manifest read. |
| `plugins/greenfield-research-platform/skills/greenfield-research-platform/assets/campaign-docs-skill.md.template` | No. Wrap-docs procedure. | On-demand manifest read. |
| `plugins/greenfield-research-platform/skills/greenfield-research-platform/assets/config.toml.template` | No. MCP launch command only. | Comment: endpoints only. |
| `plugins/greenfield-research-platform/skills/greenfield-research-platform/references/harness-files.md` | No. File contracts. | Manifest contract added. |
| `plugins/greenfield-research-platform/skills/greenfield-research-platform/references/worked-example.md` | No. Roles. Says to copy structure, not domain policy. | Manifest row added. |
| `plugins/greenfield-research-platform/skills/greenfield-research-platform/references/standup-checklist.md` | No. Checklist. | Pointer checkbox added. |

Out of scope for this audit, and not added: Petra or any other CEM DNA, fintech ontology, CannaSage ontology, a vendor EvoOntology plugin.
