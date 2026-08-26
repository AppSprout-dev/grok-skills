# Worked example (file roles)

A production chemistry research platform used this layout. Copy structure and process, not domain policy.

## File roles

| Path | Role |
|---|---|
| `docs/PRD.md` | Founding PRD |
| `docs/ARCHITECTURE.md` | Layer diagram and ownership table |
| `docs/V0.1.md` | Platform snapshot tag (harness works, not a measured result) |
| `AGENTS.md` | Two-process split |
| `.grok/config.toml` | Project MCP |
| `.grok/agents/{name}-campaign.md` | Chief-of-staff host |
| `.grok/agents/{name}-research.md` | Arm worker |
| `.grok/skills/{challenge}-cycle/SKILL.md` | Kickoff + orchestrator + burst + policy |
| `.grok/skills/campaign-docs/SKILL.md` | wrap-docs only |
| `config/challenges/{id}.toml` | Challenge ranges and weights |
| tools surface module | Tool contract MCP wraps 1:1 |
| thin MCP adapter | No loop inside the server |
| wrap CLI | Store snapshot into Kickoff / shortlist / learnings |
| schema | Candidates, scores, embeddings, cluster state, lab flags |
| experimental package | Human protocol + ingest stub |

## What “the harness works” means

A live campaign host spawned research arms (`background=true`, shared-store isolation), stored a bounded burst through MCP, received ARM REPORTs, merged structured state, and `resume_from` the next burst.

## What not to copy blindly

- Domain estimators, class cautions, and target numbers — policy, not platform law
- Encoder and validator libraries — stack, not process
- Empirical-flag name — rename to the new domain
- Burst cap of 8 and max 3 children — starting numbers; keep them bounded and documented
- Commercial, certification, or lab-spend detail — out of scope for this skill

## Two-process reminder

Coding sessions build the repo. They do not generate the Challenge 0 shortlist.

Research is a separate process:

```bash
grok --agent {name}-campaign
```

Arm workers must not edit the repo. The host may run wrap-docs after each ARM REPORT. It must not edit product code, schema, or tests and must not `git commit`.
