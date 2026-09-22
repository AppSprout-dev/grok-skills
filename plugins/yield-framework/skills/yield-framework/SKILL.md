---
name: Yield Framework
description: >-
  use this when designing architecture, reviewing code for cleverness debt,
  planning a greenfield for compounding, or choosing build-vs-configure /
  clever-vs-simple
---
# Yield Framework

Source (Claude Code plugin): [jkbennitt/yield-framework](https://github.com/jkbennitt/yield-framework) / canonical org copy [AppSprout-dev/yield-framework](https://github.com/AppSprout-dev/yield-framework). Companion Grok packaging lives in [AppSprout-dev/grok-skills](https://github.com/AppSprout-dev/grok-skills).

Philosophy: use first principles to find what compounds, then get out of its way. Synthesizes the Bitter Lesson (general methods that ride compute/data/iteration beat hand-crafted cleverness) with ruthless scope control.

## Modes (pick one)

| Mode | When | Depth |
|------|------|-------|
| **check** | Current file, PR diff, or recent edits | Fast scan; if clean, say so and stop |
| **audit** | Whole repo / architecture review | Full six-section audit + prioritized plan |
| **new** | Greenfield or re-architecture | Compounding-first starting architecture |

Do not run all three unless asked. Default to **check** for a narrow ask; **audit** when the user names a project or wants a full pass; **new** when nothing exists yet or they say “architect from scratch.”

## Five principles

1. **Identify the compounding asset** — What gets better with more compute, data, or iteration? Invest there.
2. **Minimize embedded cleverness** — Hand-crafted heuristics and “smart” shortcuts encode assumptions that rot.
3. **Build what builds** — Prefer tooling, pipelines, fixtures, and loops over one-off artifacts.
4. **Optimize for iteration speed** — Search beats planning; shrink cycle time.
5. **Defer decisions to systems that scale** — Configure or learn when the decision is judgment-under-uncertainty; do not hardcode snapshots of human taste.

## Hard walls (do not mis-label as cleverness debt)

These are intentional leverage in this stack. Flag *rot* or *missing evidence*, not the existence of the pattern:

- **Deterministic domain DNA** — CEM rules-as-code, physics modules, manufacturing constraints, emission contracts. Compounding happens through fixtures, golden paths, and fidelity honesty — not by “learning” the laws of physics in chat.
- **Proof / law surfaces** — Bend LAWS/PROOF (and paired benches). Comment or fixture gaps are debt; the law body is not.
- **Typed judgment schemas** — Choice/Score-style gates are triage walls. Schema changes need an explicit schema level plus a paired bench; do not “simplify” them into free-form LLM judgment.
- **Refuse-overclaim / structural-only public language** — Settlement, FTD, swaps, short interest. Never trade Yield “simplicity” for accusatory or overclaiming wording.
- **Secrets and credentials in code** — Always critical. Externalize; never “defer” secrets into the repo.

When Yield and a sibling skill conflict, the sibling wins for its domain: `cem-design-process`, `bend-laws-and-proof-spike`, `typesafe-ai`, `refuse-overclaim`, `paired-tau-gate` (in grok-skills), `greenfield-research-platform`.

## Cleverness debt patterns (still flag)

- Magic numbers and undocumented thresholds in product/business logic
- Domain heuristics that should be config, data, or a general method
- Deep nesting / high cyclomatic complexity where a table, rules file, or simpler control flow would do
- Configuration, URLs, environment forks, or feature flags buried in code
- Repeated near-copy blocks that should be one abstraction
- Clever algorithms where simple + scalable is enough
- Hardcoded scale/environment assumptions

**Not** automatic debt: named constants with units and provenance; physics coefficients with citations and fixtures; intentional `@unsafe` / proof exceptions that are documented and tested.

## Mode recipes

### check

Scan the named file(s) or diff only.

For each real issue:

```
[PATTERN] path:line
What assumption it encodes
→ Replace with: …
```

Prioritize by impact. Invent nothing. If clean: one sentence, then stop.

### audit

1. **Compounding assets** — What actually compounds; what was meant to and does not; accidental compounding to lean into.
2. **Cleverness debt inventory** — Patterns above; for each, the aging assumption.
3. **Leverage vs artifact vs scaffolding** — Tooling/pipelines vs end products vs delete candidates.
4. **Iteration friction** — Estimated feedback cycle; top 3 blockers; quick wins.
5. **Hardcoded judgment** — What should move to config, data, or a general system (respect hard walls).
6. **Scaling ceiling** — What breaks first at ~100× usage/data/compute.

End with the action plan below.

### new

Ask only what you cannot infer. Then output a starting architecture that:

1. Names the compounding asset explicitly
2. Lists substrate to build first (CI, fixtures, pipelines, DX)
3. Flags cleverness traps for this domain
4. Defines the minimum viable iteration loop
5. Lists decisions to defer (config / flags / learned where appropriate)
6. Defines MVP scope and an explicit **what not to build** list

Be opinionated. Push back on admin dashboards, custom auth, and features that do not test the compounding hypothesis — unless a hard wall or sibling skill requires them.

## Action plan format (audit / new)

```
## AMPLIFY
…

## DELETE
…

## REPLACE
…

## DEFER
…
```

Be specific (paths, symbols). Every observation needs a recommendation. Most impactful first. Concise.

## Output tone

Direct, not preachy. One observation paired with one suggestion for inline notes. Prefer plain prose over slogans. Do not anthropomorphize models. Do not claim a system “learns” when the real compounding asset is deterministic fixtures and proof.

## Anti-patterns for the auditor

- Treating every constant as debt
- Recommending LLM judgment in place of CEM/Bend/Jev evidence walls
- Expanding scope into DNA/scorer/recipe mutation or silent fallbacks
- Inventing findings on a clean check
- Packaging advice as Claude Code hooks/slash commands — this skill is the Grok Bot recipe; Claude plugin install stays in the yield-framework repos
