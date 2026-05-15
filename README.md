# agent-skills

A collection of reusable agent skills and plugins for OpenClaw, Hermes, pi, and other [agentskills.io](https://agentskills.io)-compatible agents. Each entry is a Git submodule pointing to its own repo.

## Setup

```bash
git clone --recurse-submodules https://github.com/jal-co/agent-skills.git
```

If already cloned:

```bash
git submodule update --init --recursive
```

## Skills

<!-- Keep this table in sync when adding or removing skills. -->

| Skill | Description | Source |
|---|---|---|
| `openclaw-webclaw` | OpenClaw plugin replacing Firecrawl with native WebClaw v1 API — scrape, crawl, extract, diff, search, summarize, map, batch, brand | [repo](https://github.com/jal-co/openclaw-webclaw) |

## Adding a Skill

See [AGENTS.md](./AGENTS.md) for the full workflow — create a skill repo with a valid `SKILL.md`, add it as a submodule, and update the table above.
