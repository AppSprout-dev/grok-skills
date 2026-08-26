---
name: greenfield-research-platform
description: Use when starting or restructuring a greenfield computational research platform on the open-source Grok Build harness. Covers founding PRD, clean-room repo, two-process coding versus campaign split, chief-of-staff campaign host plus specialist arm workers, MCP tool surface, ZDR storage, structured campaign memory, Challenge-0 closed loop, and experimental hand-off stubs. Trigger on new research platform, greenfield repo, Grok Build harness, campaign host, orchestrator plus sub-agents, or iterative recursive research loop.
metadata:
  type: workflow
  version: "1.0.0"
  author: AppSprout
---

# Greenfield Research Platform

## Purpose

Stand up a computational research platform on the open-source Grok Build harness. The product is not a chatbot and not a Python agent loop. It is a harness around on-box domain compute, a durable store of record, and a recursive campaign of specialist sub-agents.

This skill is the process. It does not replace a project-context skill. Write that sibling once the new platform’s PRD is accepted.

## What this method is

A greenfield research platform has four layers that must stay separate:

1. **Harness** — open-source Grok Build (`grok` TUI / ACP). Skills, custom agents, MCP, AGENTS.md.
2. **Chief of staff** — one operator-facing campaign host that talks to the user, spawns specialist children, synthesizes reports, and issues the next burst.
3. **Domain tools** — deterministic operators (propose, evaluate, score, store, retrieve, reflect) exposed 1:1 over MCP. No loop inside the server.
4. **Store of record** — usually Postgres plus domain extensions (vectors, graph, cartridges). Structured campaign state lives here, not in a prose memory wall.

The LLM proposes hypotheses and chooses strategy. Tools and the store decide what is true.

## Core process (ordered, non-negotiable)

### 1. Lock the strategic frame first

- Name the platform and the first challenge.
- Write and accept a short founding PRD before any product code.
- State scope, architecture principles, and explicit non-goals in writing.
- Define measurable success metrics that include both computational gates and later physical/empirical gates.
- Decide the primary harness (Grok Build or a controlled fork). Do not start on a retired agent SDK.
- Treat any prior thin-slice or host-coupled repo as a **negative reference**. Prefer a clean-room greenfield repository.

### 2. One Challenge 0 before extra tracks

- The first campaign is a single closed loop on one material, mechanism, or question.
- Parallel product tracks, extra domains, and commercial packaging wait until that loop produces literature-grounded, honesty-labeled shortlists.
- Stub the later experimental hand-off on day one so the architecture does not need surgery when the lab starts.

### 3. Split coding from research

Two processes. Never collapse them.

| Process | Owns | Must not |
|---|---|---|
| Coding TUI (`grok` in the repo, default agent) | Language stack / schema / tests / MCP surface / literature seed / skills / agent files | Run propose / score / store as the product path |
| Campaign host (`grok --agent {name}-campaign`) | Talk to the operator, spawn arm workers, merge structured state, run the wrap-docs CLI | Edit product code, schema, or tests; `git commit`; spawn coding / explore / plan |
| Arm workers (`{name}-research` subagents) | One bounded burst of Design–Score–Analyze through MCP | Write files, run a shell, spawn children, loop forever |

`grok -p "...one cycle"` is a debug one-shot. It is not the campaign.

After any pull that changes the MCP server or tool surface, **quit the research Grok process and start a new `grok --agent {name}-campaign`**. `grok mcp disable/enable` only writes config. `/new` keeps the old in-memory server.

### 4. Wire Grok Build as the harness, not as a wrapper

Minimum repo surface:

```
AGENTS.md
.grok/config.toml              # project MCP server
.grok/agents/{name}-campaign.md
.grok/agents/{name}-research.md
.grok/skills/{challenge}-cycle/SKILL.md
.grok/skills/campaign-docs/SKILL.md
docs/PRD.md
docs/ARCHITECTURE.md
config/challenges/{challenge}.toml
```

Copy templates from `assets/` and replace `{name}` / `{challenge}`:

- `assets/AGENTS.md.template`
- `assets/campaign-agent.md.template`
- `assets/research-agent.md.template`
- `assets/config.toml.template`
- `assets/cycle-skill.md.template`
- `assets/campaign-docs-skill.md.template`

Full file contracts live in `references/harness-files.md`.

### 5. Encode the chief-of-staff pattern

This is the recursive design loop. Same shape as Grok multi-agent bots: one operator conversation, specialist children, parent synthesizes.

**Host (chief of staff)**

1. On the first turn of a new campaign session, start. Do not wait for a kickoff paste.
2. Read memory tools **sequentially** (parallel schema tools can deadlock a transactional store).
3. Run the wrap-docs CLI once so Kickoff / shortlist / learnings match the store.
4. If the campaign stop predicate is true, print the selected set and do not spawn.
5. Otherwise spawn 2–3 arm workers in one turn (`background=true`, `isolation=none` when they must share the store). Prompt each with a bounded assignment plus current structured state.
6. Stay the conversation. When a worker finishes, its structured report **is** the next host turn.
7. Merge structured state, brief the operator in a few lines, run wrap-docs, then immediately `resume_from` that child or spawn the next arm.
8. Do not ask whether to continue. Stop only on the documented stop predicate, a missing MCP server, or an operator interrupt.

**Workers**

- Own one arm and one burst (cap stored cycles; start at ≤8).
- Do not spawn. Do not write files. Do not loop past the burst cap.
- End with a structured ARM REPORT the host can parse. No essay.

**Hard limits**

- Max three live children.
- Never nest (workers do not spawn).
- Do not spawn the built-in `coding` / `explore` / `plan` agents from a research host.
- If spawn is unavailable, the host runs bursts sequentially. It still does not grind the whole campaign in one worker.

### 6. Keep domain compute on-box and honest

- Validation, physics or property estimators, scoring, and embeddings are ordinary code. They are not LLM text.
- Prefer a fast systems-language scorer with a slower fallback.
- Self-host domain foundation models. Treat them as **encoders and optional overlay heads**, never as the source of design truth.
- Default embeddings should work with no network (fingerprint fallback).
- Every predicted quantity carries an explicit source and fidelity band (estimate vs measured, overlay vs baseline).
- Calibration tables and ledger hooks exist from day one even if empty.

### 7. Give the agent a tool surface, not a second brain

Minimum MCP tools (adapt names to the domain):

- `propose_batch` / `score_batch` / `store_candidates`
- `retrieve_similar` / `retrieve_lineage` / domain front queries
- `reflect_on_batch` / `update_memory`
- structured-state get/set (cluster status analog)
- `retrieve_literature` / graph retrieve
- `retrieve_campaign_wrap`

Rules:

- MCP wraps the tool contract 1:1. No Design–Score–Analyze loop inside the server.
- Do not add `run_cycle`, a Python agent, or a CLI that re-implements the outer loop.
- `propose_batch` is a deterministic operator (seed / analogue / de-novo or domain equivalent), not the orchestrator.
- Freeze operator lists that the science report must not invent. Novelty belongs in seeds and retrieval, not in silent string-edits to the generator.

### 8. Memory is structured state

- The store of record holds candidates, scores, embeddings, lineage, literature, run reflections, and cluster status.
- Campaign policy lives in structured rows (`family_key` → frozen / keep-open / dead-end / open), plus a short `next_hypothesis`.
- Do not encode policy as a growing prose forbid wall.
- Wrap-docs snapshots the store into Kickoff markers, shortlist docs, and `data/campaign/learnings.json` so the next host session starts from the database, not from chat residue.
- Session-specific lore belongs in `update_memory`, not in the cycle skill.

### 9. Separate computational promotion from physical truth

- Computational ultra-verified (selected for lab) is a campaign stop, not a measured result.
- The empirical flag (`take_to_lab` or domain equivalent) stays false until a documented measured gate passes.
- The experimental module is a logged human protocol plus ingest stub. It is not a robot chemist and not the synthesis shopping list.
- Long-horizon search is how candidates are earned. A handful of generations is not enough to spend on wet work or hardware.

### 10. Structure the first implementation as a complete epic

Adapt names to the domain. Do not skip layers.

1. Solution and project foundation (repo, CI, ZDR posture, AGENTS.md)
2. Store of record (schema, vectors, graph, challenge config)
3. Domain compute (validators, estimators, scorer)
4. Foundation-model encoders and honesty-gated heads
5. MCP tool surface
6. Grok Build agents, cycle skill, wrap-docs
7. Literature seed and structured campaign state
8. Challenge 0 demonstration (one live host → worker → report → resume loop)
9. Experimental hand-off stubs
10. Documentation, ADRs, platform snapshot tag

Each issue should become its own commit.

## Stack defaults (adapt, do not cargo-cult)

Keep the *roles*. Libraries change with the domain.

| Role | Example chemistry stack | Generalize as |
|---|---|---|
| Harness | Grok Build + custom agents + MCP | Same |
| Orchestration | `{name}-campaign` host + `{name}-research` arms | Same pattern |
| Domain validation | Domain-native validator (RDKit in chemistry) | Domain-native validator |
| Estimators | First-order physics with labeled priors | Same |
| Scorer | Fast systems-language scorer + slower fallback | Same |
| Store | Durable store + vectors + lineage | Same |
| Encoders | Self-hosted domain encoder + offline fallback | Same |
| Privacy | ZDR, local model cache, no third-party payload logs | Same posture unless the operator explicitly relaxes it |

## Anti-patterns (reject these)

- Incremental patch of a tainted prior repo instead of a greenfield clean-room
- Coding TUI that is also the generator
- Python `while` loop or `run_cycle` that re-implements the agent
- MCP server that hides an inner agent loop
- Workers that spawn, write the repo, or run until interrupt
- Host that waits for permission after every burst
- Built-in `coding` / `explore` / `plan` spawned from a research host
- LLM as the source of scientific truth
- Foundation-model heads trained on quantities the lab has not measured, then treated as measured
- Prose forbid-wall memory
- `grok -p` one-shots sold as a campaign
- `/new` after an MCP pull instead of a full process relaunch
- Promoting computational hits to ready-for-the-world without an empirical gate
- Expanding to a second domain before Challenge 0 closes
- Shadow-library literature. Inspectable primaries only

## Fresh-platform checklist

Use `references/standup-checklist.md` as the working list. Do not mark Challenge 0 done until a live host session has spawned workers, stored a burst, received an ARM REPORT, merged state, and `resume_from` the next burst.

## Relationship to other skills

- `cem-design-process` — deterministic design-intelligence CEMs. Different product. This skill is for search/campaign systems, not recipe emitters.
- Write a sibling project-context skill once a platform PRD is accepted.
- `skill-creator` — how to package the per-project cycle skill and campaign-docs skill that each new platform still needs.

## Working rule

When the user proposes a new research platform, a greenfield rebuild, or a Grok Build campaign of sub-agents, load this skill first. Confirm the process steps and the two-process split before writing product code or opening implementation issues. File-layout notes live in `references/worked-example.md`.
