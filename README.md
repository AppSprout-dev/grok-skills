# AppSprout Grok Skills

**v1.0.0** · MIT · [Releases](https://github.com/AppSprout-dev/grok-skills/releases)

Process skills for [Grok Build](https://x.ai/build) and Grok chat. This repository is a small marketplace: two reusable methods, no project-private context.

Topics: `grok` · `grok-build` · `skills` · `plugins` · `marketplace` · `computational-engineering` · `multi-agent` · `mcp`

| Plugin | Use when |
|---|---|
| `cem-design-process` | Starting or restructuring a Computational Engineering Model |
| `greenfield-research-platform` | Starting or restructuring a greenfield research platform on Grok Build |

These are **process** skills. They do not ship product code, locked repo decisions, commercial paths, or lab protocols.

Companion public work:

- Claude Code plugin: [AppSprout-dev/yield-framework](https://github.com/AppSprout-dev/yield-framework) (fork of [jkbennitt/yield-framework](https://github.com/jkbennitt/yield-framework))

## Install in Grok Build

Latest:

```bash
grok plugin marketplace add AppSprout-dev/grok-skills
grok plugin install cem-design-process --trust
grok plugin install greenfield-research-platform --trust
```

Pinned to v1.0.0:

```bash
grok plugin marketplace add AppSprout-dev/grok-skills
# after add, pin the marketplace source to tag v1.0.0 / SHA of that release
grok plugin install cem-design-process --trust
grok plugin install greenfield-research-platform --trust
```

In the TUI: `/marketplace` after the source is added, then install by name.

## Install as plain skills

Copy a skill folder into `~/.grok/skills/` or a project's `./.grok/skills/`:

```
plugins/cem-design-process/skills/cem-design-process/
plugins/greenfield-research-platform/skills/greenfield-research-platform/
```

Grok chat can also install from GitHub with the skill installer by pointing at those paths.

Start a new session after install so the agent picks the skills up.

## What is not in this repo

Project-context skills (locked decisions for one private codebase) stay private. Write those separately after a founding PRD is accepted.

## License

MIT. See `LICENSE`.
