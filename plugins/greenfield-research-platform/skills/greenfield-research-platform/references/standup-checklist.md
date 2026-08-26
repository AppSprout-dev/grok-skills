# Greenfield research platform standup checklist

Use this list when creating a new platform or auditing an existing one. Check items in order. Do not skip to campaign polish before the two-process split exists.

## Frame

- [ ] Platform named
- [ ] Founding PRD accepted (vision, metrics, non-goals, Challenge 0, architecture principles)
- [ ] Prior repo declared a negative reference (or explicitly re-validated, piece by piece)
- [ ] Challenge 0 is one closed loop; extra tracks are written as deferred
- [ ] Success metrics include computational gates and later empirical gates
- [ ] ZDR / payload-logging posture written down

## Repo

- [ ] New greenfield repository (not a rename of the tainted tree)
- [ ] `AGENTS.md` states the two-process split in the first screen
- [ ] `docs/PRD.md` and `docs/ARCHITECTURE.md` committed
- [ ] Challenge config is data (`config/challenges/{id}.toml`), not scattered constants
- [ ] CI runs unit tests for domain compute and the tool surface

## Store of record

- [ ] Durable store up from `docker compose` or equivalent
- [ ] Schema for candidates, properties, scores, runs, embeddings, lineage, literature, structured campaign state, lab-handoff flags
- [ ] Vector column for novelty retrieval
- [ ] Graph or explicit lineage edges
- [ ] Wrap snapshot can be produced from the store (`wrap` / `wrap-docs` analog)

## Domain compute

- [ ] Validator is pure code, no LLM
- [ ] First-order estimators exist and are labeled as estimates
- [ ] Scorer is properties-in / scores-out with a fallback path
- [ ] Calibration table hooks exist even if empty
- [ ] Tests refuse to train or advertise heads on quantities that are unmeasured

## Foundation models

- [ ] Self-hosted encoder path with an offline fingerprint fallback
- [ ] First download is explicit and gated (env flag)
- [ ] Allowed trained properties are a closed set
- [ ] Predicted values carry `*_source` fields

## Harness

- [ ] `.grok/config.toml` points at the project MCP server
- [ ] `{name}-campaign.md` host agent (write + shell only for wrap-docs)
- [ ] `{name}-research.md` worker agent (write and shell disallowed)
- [ ] `{challenge}-cycle` skill with Kickoff, Orchestrator, Child burst, Stop
- [ ] `campaign-docs` skill for the wrap-docs path
- [ ] Host starts Kickoff on the first turn
- [ ] Workers return a structured ARM REPORT and stop
- [ ] Max three live children, no nesting, no coding/explore/plan from the host

## Honesty and hand-off

- [ ] Computational promotion (`selected_for_lab` analog) is distinct from the empirical flag
- [ ] Empirical flag stays false until the documented measured gate
- [ ] Experimental module is a logged human protocol + ingest stub
- [ ] Literature ingest is inspectable primaries only

## Challenge 0 done means

A live `grok --agent {name}-campaign` session has

1. read memory sequentially,
2. spawned at least one arm worker,
3. stored a burst through MCP,
4. received an ARM REPORT,
5. merged structured state,
6. run wrap-docs,
7. issued `resume_from` or the next spawn.

A unit test of `propose_batch` is not Challenge 0 done.
