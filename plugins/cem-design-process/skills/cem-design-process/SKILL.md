---
name: cem-design-process
description: Use when starting or restructuring a Computational Engineering Model. Covers foundational-first DNA, module negotiation, clean-room rules, PRD and epic structure, mandatory deep research before code, and anti-patterns that cause thin-slice lock-in. Trigger on new CEM, redesign a CEM, module negotiation, or DNA versus phenotype.
metadata:
  type: workflow
  version: "1.0.0"
  author: AppSprout
---

# CEM Design Process

## Purpose

Capture the disciplined process for designing and implementing a Computational Engineering Model (CEM). A CEM encodes how a *class* of objects is designed: requirements, physics, manufacturing constraints, and construction logic. Inputs are high-level specs. Outputs are recipes, manufacturable parameters, and performance predictions.

A CEM is not a generative LLM. The model may propose. The CEM validates and sizes.

Use this skill for *how to structure and build* the CEM. Keep a separate pattern library for concrete types (Spec, recipe id, golden-safe consumers, process gates) if the project needs one.

## Core process (ordered, non-negotiable)

### 1. Lock the strategic frame first

- Name the CEM.
- Write and accept a short founding PRD before any code.
- Explicitly state scope (foundational vs specialized phenotype).
- List architecture principles and non-goals in writing.
- Decide primary language for the design-intelligence layer and the geometry boundary (external consumers only).
- Treat any prior thin-slice or host-coupled work as a **negative reference** only. Prefer clean-room.

### 2. Prefer foundational DNA over vehicle-first slices

- Build a fully fleshed core that can support many phenotypes later.
- Do not start with a single vehicle and hope the architecture generalizes.
- Vehicle or product specializations become later phenotypes that share the DNA.
- If a project already has a thin slice, treat expansion of the core as a deliberate re-architecture, not incremental feature work.

### 3. Encode the construction logic as modules that negotiate

- Decompose designs into modules that own local parameters and rules.
- Modules interact under global constraints (functional, physical, process, performance).
- Manufacturing and process limits are first-class inputs that actively reshape design, not post-checks.
- Support generation, cross-section adaptation, and load-bearing transformation of modules are part of the negotiation pattern.

### 4. Keep pure design maths separate from geometry

- The CEM owns sizing, selection, constraint satisfaction, performance prediction, and recipe generation.
- Geometry kernels remain external consumers of emission contracts.
- Never embed a second geometry kernel inside the CEM.

### 5. Require determinism, identity, and honest fidelity

- Identical inputs plus model versions must produce identical recipes.
- Every design carries a deterministic recipe identity.
- Every model result carries explicit fidelity bands. Never over-claim.
- Leave calibration and ledger hooks from day one so physical or simulation feedback can refine coefficients without breaking historical recipes.

### 6. Structure implementation as a complete epic with no deferments

Recommended workstream order (adapt names to the domain, but do not skip layers):

1. Solution and project foundation
2. Core domain abstractions (Spec, Recipe, Identity, Fidelity, Versioning)
3. Material / feedstock (or domain equivalent) system
4. Process and manufacturing constraint system
5. Physics and sizing model framework
6. Module system and negotiation engine
7. Architecture / phenotype machinery
8. Recipe generation and emission contracts
9. Calibration, ledger, and feedback hooks
10. Self-test, golden paths, and quality infrastructure
11. First coherent demonstration family (proves the foundation without locking to one product)
12. Documentation, ADRs, and governance

Each issue should become its own commit for auditability.

### 7. Gate every implementation issue with deep research

Before writing any code for an issue:

- Re-read the PRD and relevant principles.
- Research the corresponding domain concept thoroughly (physics, process limits, prior art).
- Produce a short research brief mapping findings to acceptance criteria.
- Obtain explicit go-ahead before coding.
- Reject placeholder or “TODO later” scaffolding that leaves acceptance criteria unmet.

### 8. Consumers come after the core is stable

- Keep the CEM consumer-agnostic.
- Emission contracts must be readable by multiple independent consumers.
- Decide priority among authoring surfaces, geometry hosts, analysis pipelines, or product clients only after the foundational contracts exist.

## Anti-patterns (reject these)

- Thin single-product or single-vehicle first slice that becomes the de-facto core
- Premature coupling to a specific geometry host or product surface
- Treating an LLM as the source of design truth
- Consumers that mutate the authoritative recipe
- Over-claiming fidelity or class-of-system validation that has not been earned
- Heavy migration of prior thin-slice code that re-introduces the same architectural compromises
- Deferring essential foundational machinery “for later”
- Boiling the ocean on many vehicles before the DNA exists

## When applying to an existing CEM

- Inventory what already exists against the workstream list above.
- Identify which layers are missing, thin, or host-coupled.
- Prefer expanding or re-founding the core over extending a locked-in slice.
- Use the negative-reference discipline: keep useful pure functions only after re-validation under the new architecture.
- Create or update a PRD and epic so the remaining work is fully specified and non-deferred.

## Relationship to other skills

- A companion *pattern library* skill may document Spec objects, recipe ids, golden-safe consumers, and process gates. This skill stays the process.
- Write a separate project-context skill once a CEM’s PRD is accepted. Do not put locked repo state in this file.
- `greenfield-research-platform` is a different product (search/campaign systems, not recipe emitters).

## Working rule

Any time a new CEM is proposed or an existing one is to be significantly advanced, load this skill first and confirm the process steps before writing code or opening implementation issues.
