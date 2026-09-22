---
name: yield-framework
description: Use for architecture, cleverness-debt review, or greenfield compounding. Modes are check, audit, and new. Trigger on yield review, cleverness debt, compounding asset, build versus configure, or a new project architecture.
metadata:
  type: workflow
  version: "1.0.0"
  author: AppSprout
---

# Yield Framework

Process skill with three modes: `check`, `audit`, and `new`. No Claude slash commands. No PostToolUse hooks. The sections below are the AppSprout-dev/yield-framework texts (principles, then each mode).

# Yield Framework Skill

## Description
The Yield framework helps build software that compounds. It synthesizes the Bitter Lesson (general methods leveraging computation beat hand-crafted cleverness) with first principles thinking (identify what scales, then get out of its way).

## When to Activate
Activate Yield thinking when:
- Designing new systems or architecture
- Making build vs. configure decisions
- Reviewing code for quality
- Discussing technical debt or refactoring
- Choosing between "clever" and "simple" solutions
- Planning iteration/feedback loops
- Evaluating what to build vs. skip

## The Five Principles

1. **Identify the compounding asset**: What gets better with more compute, data, or iteration? Invest there. Everything else is technical debt in disguise.

2. **Minimize embedded cleverness**: Hand-crafted heuristics, domain-specific optimizations, and "smart" shortcuts encode assumptions that rot. Build systems that learn or that are trivially replaceable.

3. **Build what builds**: Tooling, pipelines, and iteration loops produce value. Artifacts just consume it. Invest in leverage, not solutions.

4. **Optimize for iteration speed, not plan quality**: Search beats planning. The fastest path to a good system is rapid feedback, not upfront design. Reduce cycle time ruthlessly.

5. **Defer decisions to systems that scale**: When choosing between encoding human judgment vs. letting a general system figure it out, bias heavily toward the latter. Your judgment is a snapshot. The system keeps learning.

## Cleverness Debt Patterns
Flag when you see:
- Magic numbers and hardcoded thresholds
- Domain-specific heuristics in business logic
- Deeply nested conditionals (high cyclomatic complexity)
- Configuration buried in code
- Repeated patterns that should be abstracted
- Clever algorithms where simple + scalable would work
- Hardcoded assumptions about environment, scale, or domain

## How to Apply
When giving advice, bias toward:
- Configuration over code
- General over specific
- Learned over hand-crafted
- Simple over clever
- Fast iteration over perfect planning
- Leverage over artifact

## Output Style
When applying Yield thinking:
- Be direct about what compounds and what doesn't
- Name specific anti-patterns when you see them
- Suggest the simpler, more general alternative
- Don't be preachy—one observation, one suggestion
- Use "⚡ Yield:" prefix for inline observations

# Yield Check

Quick cleverness debt scan of the current file or recent changes.

## Instructions

Perform a fast scan for Yield anti-patterns in the current context. Focus on the file currently being edited or recent changes.

### Scan For These Patterns

**Magic Numbers & Hardcoded Values**
- Numeric literals without explanation
- Hardcoded strings that should be config
- Timeout/retry values without rationale
- Threshold values buried in logic

**Embedded Heuristics**
- If/else chains encoding domain knowledge
- Switch statements with business logic
- Scoring/weighting calculations
- Custom sorting/ranking logic

**Complexity Signals**
- Functions longer than 50 lines
- Nesting deeper than 3 levels
- More than 5 parameters
- Multiple return paths with different logic

**Configuration in Code**
- URLs, endpoints, hostnames
- Feature flags as booleans
- Environment-specific logic
- Credentials or API keys (critical!)

**Repeated Patterns**
- Similar code blocks that should be abstracted
- Copy-paste with minor variations
- Parallel structures that could be unified

### Output Format

For each issue found:
```
[PATTERN TYPE] filename:line
Brief description of the issue
→ Yield recommendation: [what to do instead]
```

Prioritize by impact. If the code is clean, say so and move on—don't invent problems.

Keep output concise. This is a quick check, not a full audit.

# Yield Audit

Perform a comprehensive Yield framework analysis on the current project or specified files.

## Instructions

Analyze this codebase through the Yield framework lens:

### 1. Compounding Assets Audit
Identify what in this system actually gets better with more compute, data, or iteration. Also note what was *intended* to compound but doesn't, and what accidentally compounds that should be leaned into.

### 2. Cleverness Debt Inventory
Scan for and list:
- Magic numbers and hardcoded thresholds
- Domain-specific heuristics and "smart" shortcuts
- Deeply nested conditionals (cyclomatic complexity > 10)
- Configuration buried in code instead of externalized
- Hand-crafted optimizations that encode assumptions
- Clever algorithms where simple + general would work

For each item found, note: what assumption does it encode, and how might that assumption age?

### 3. Leverage Inventory
Categorize the codebase into three buckets:
- **LEVERAGE**: Tooling, pipelines, abstractions that multiply effort
- **ARTIFACT**: End products that deliver value but don't compound
- **SCAFFOLDING**: Neither leverage nor artifact—should probably be deleted or replaced

### 4. Iteration Friction Analysis
Identify:
- Current feedback cycle time (estimate)
- Top 3 friction points slowing iteration
- Quick wins that would dramatically speed up the loop

### 5. Hardcoded Judgment Scan
Find where human decisions are baked into code that could be:
- Moved to configuration
- Made learned/adaptive
- Replaced with more general solutions

### 6. Scaling Ceiling
If this system got 100x more usage/data/compute, what breaks first? What would we wish we'd built differently?

## Output Format

Provide a prioritized action plan:
- **AMPLIFY**: What's working and should be invested in further
- **DELETE**: What's pure liability and should be removed
- **REPLACE**: What should be swapped for something more general
- **DEFER**: What's fine for now but flagged for future leverage

Be specific. Name files, functions, and line numbers where possible.

# Yield New Project

Help architect a new project using Yield framework principles to maximize compounding from day one.

## Instructions

Guide the user through setting up a new project with the Yield framework. Ask them to describe their project, then help them think through:

### 1. What Compounds Here?
Given this domain, what aspects of the system will get better with more compute, data, or iteration? These are the core investments.

Examples to probe:
- Data that improves predictions over time
- User behavior that trains recommendations
- Content that builds SEO/discovery
- Tooling that accelerates future development

### 2. What's the Substrate?
What tooling, pipelines, or infrastructure will produce the most leverage? What should be built that builds other things?

Help them identify:
- CI/CD and deployment automation
- Testing infrastructure
- Data pipelines
- Developer experience tooling
- Abstraction layers that will be used repeatedly

### 3. Cleverness Traps
Identify the likely traps—places they'll be tempted to hand-craft heuristics or encode domain knowledge that will rot.

Common traps:
- Custom scoring/ranking algorithms
- Hardcoded business rules
- Domain-specific optimizations
- "Smart" caching strategies
- Hand-tuned thresholds

### 4. Minimum Viable Iteration Loop
What's the fastest possible feedback cycle they can establish on day one? What would make iteration 10x faster than the obvious approach?

Consider:
- Hot reload / instant preview
- Automated testing on save
- Feature flags for instant rollback
- Synthetic data for development
- Local-first architecture

### 5. Deferred Decisions
What choices can be pushed to runtime, configuration, or learned systems instead of hardcoding now?

Examples:
- Thresholds and weights → config files
- Feature toggles → feature flag service
- Business rules → rules engine
- Recommendations → ML model
- Copy/content → CMS

### 6. What NOT to Build
What's the smallest possible artifact that still validates whether the compounding asset works?

Help them ruthlessly cut:
- Admin dashboards (use existing tools)
- Custom auth (use a service)
- Analytics (use off-the-shelf)
- Features that don't test the core hypothesis

## Output Format

Provide a concrete starting architecture that:
1. Names the compounding asset explicitly
2. Lists the substrate to build first
3. Flags cleverness traps to avoid
4. Defines the iteration loop
5. Lists decisions to defer
6. Defines the MVP scope

Be opinionated. Push back on complexity.

