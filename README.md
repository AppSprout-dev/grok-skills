# AppSprout Grok Skills

**v1.0.0** · MIT · [Releases](https://github.com/AppSprout-dev/grok-skills/releases)

Process skills for [Grok Build](https://x.ai/build) and Grok chat. This repository is a small marketplace: two design methods plus an exposure-gate skill, no project-private context.

Topics: `grok` · `grok-build` · `skills` · `plugins` · `marketplace` · `computational-engineering` · `multi-agent` · `mcp`

| Plugin | Use when |
|---|---|
| `cem-design-process` | Starting or restructuring a Computational Engineering Model |
| `greenfield-research-platform` | Starting or restructuring a greenfield research platform on Grok Build |
| `paired-tau-gate` | Opening a PR that changes a skill, MCP schema/manifest, or plugin surface |

These are **process** skills. They do not ship product code, locked repo decisions, commercial paths, or lab protocols.

Companion public work:

- Claude Code plugin: [AppSprout-dev/yield-framework](https://github.com/AppSprout-dev/yield-framework) (fork of [jkbennitt/yield-framework](https://github.com/jkbennitt/yield-framework))

## Install in Grok Build

Latest:

```bash
grok plugin marketplace add AppSprout-dev/grok-skills
grok plugin install cem-design-process --trust
grok plugin install greenfield-research-platform --trust
grok plugin install paired-tau-gate --trust
```

Pinned to v1.0.0:

```bash
grok plugin marketplace add AppSprout-dev/grok-skills
# after add, pin the marketplace source to tag v1.0.0 / SHA of that release
grok plugin install cem-design-process --trust
grok plugin install greenfield-research-platform --trust
grok plugin install paired-tau-gate --trust
```

In the TUI: `/marketplace` after the source is added, then install by name.

## Install as plain skills

Copy a skill folder into `~/.grok/skills/` or a project's `./.grok/skills/`:

```
plugins/cem-design-process/skills/cem-design-process/
plugins/greenfield-research-platform/skills/greenfield-research-platform/
plugins/paired-tau-gate/skills/paired-tau-gate/
```

Grok chat can also install from GitHub with the skill installer by pointing at those paths.

Start a new session after install so the agent picks the skills up.

## Fleet policy

Skill, MCP, and plugin exposure PRs use the paired τ gate: `docs/PAIRED_TAU_GATE.md`. τ, the Petra Challenge 0 replay V, and the primary metric are TODO(Petra). This repo does not ship that replay or any CEM DNA.

Harness templates point at project files via `assets/semantic-manifest.md.template`. They do not paste DNA or PRD bodies. Audit: `docs/TEMPLATE_DNA_AUDIT.md`.

## What is not in this repo

Project-context skills (locked decisions for one private codebase) stay private. Write those separately after a founding PRD is accepted. CEM DNA, scorer weights, golden recipe ids, Bend LAWS/PROOF, and Jev schemas stay in their product repos.

## License

MIT. See `LICENSE`.
