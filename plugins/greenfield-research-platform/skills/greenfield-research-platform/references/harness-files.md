# Grok Build harness files

Templates live in `assets/`.

## AGENTS.md

Repo-root convention file. Grok Build loads it for every agent with `agents_md: true`.

Must state, in the first screen:

- What Challenge 0 is
- The two-process split (coding TUI vs campaign host vs workers)
- How to start the campaign (`grok --agent {name}-campaign`, `/effort high`)
- That `grok -p` is not the campaign
- Branch hygiene
- What the host may write (wrap-docs only) and what workers may not write
- Relaunch rule after MCP/tool-surface pulls

Do not turn AGENTS.md into the cycle skill. Policy detail belongs in `.grok/skills/{challenge}-cycle/SKILL.md`.

## .grok/config.toml

Project MCP server. Coding Grok in the repo is not the generator.

```toml
[mcp_servers.{name}]
command = "uv"
args = ["run", "--directory", "python", "-m", "{name}.mcp"]
env = { PLATFORM_DSN = "${PLATFORM_DSN}" }
enabled = true
```

Adapt runtime and module path. The DSN must be the same one wrap-docs uses.

## Custom agents (`.grok/agents/*.md`)

Campaign host frontmatter:

```yaml
---
name: {name}-campaign
description: >
  Campaign host. Talks to the operator. Spawns research arm workers,
  waits for reports, merges structured state, runs wrap-docs, sends
  the next burst. Does not grind propose/score/store itself.
  Use: grok --agent {name}-campaign
tools:
  - search_tool
  - use_tool
  - read_file
  - spawn_subagent
  - write
  - search_replace
  - run_terminal_command
  - run_terminal_cmd
disallowedTools: []
prompt_mode: full
permission_mode: always-approve
agents_md: true
---
```

Worker differences:

- `disallowedTools` includes `write`, `search_replace`, `run_terminal_command`, `run_terminal_cmd`
- `permission_mode: default`
- Body forbids spawning and file edits
- Burst cap and ARM REPORT format are mandatory

Spawn call shape the host must use:

- `subagent_type="{name}-research"`
- `background=true`
- `isolation=none` when children share the store
- `resume_from=<child id>` only after that child has completed

Built-in types (`general-purpose`, `explore`, `plan`, coding) are not research arms.

## Cycle skill

`.grok/skills/{challenge}-cycle/SKILL.md` is the research-process skill. It must contain:

1. **Kickoff** — machine-editable marker block the wrap-docs CLI rewrites
2. **Orchestrator** — host steps (read, spawn, merge, wrap, resume)
3. **Child burst** — worker cap, early-stop rules, ARM REPORT schema
4. **Session loop** — host rounds are the long horizon
5. **Steps per cycle** — sequential tool order
6. **Policy** — domain-specific, short, not a forbid wall
7. **Stop** — computational promotion predicate plus operator interrupt

Kickoff markers must be unique and boring so wrap-docs can rewrite them:

```
<!-- campaign-kickoff:begin -->
...
<!-- campaign-kickoff:end -->
```

## campaign-docs skill

Coding-TUI / host skill that only runs the wrap CLI. It must not propose, score, or store.

## Wrap CLI

One command snapshots the store and retargets Kickoff:

```
{runner} python -m {name}.learn wrap        # JSON snapshot to stdout
{runner} python -m {name}.learn wrap-docs   # rewrite Kickoff, shortlist, learnings
```

wrap-docs may promote computationally ultra-verified rows. It must not flip the empirical flag.

## Relaunch

| Action | Reloads MCP child? |
|---|---|
| `grok mcp disable/enable {name}` | No, writes config only |
| `/new` in an existing TUI | No, keeps in-memory server |
| Quit process, `grok --agent {name}-campaign` | Yes |

## Isolation

Research arms that share a transactional store use `isolation=none`. Worktree-isolated coding subagents are a different pattern and do not belong on the research host.
