# deploy-team

A Claude Code skill that deploys a 4-manager AI organizational hierarchy to any existing repo.

## What It Does

Run `/deploy-team` in any repo to get an AI-powered team:

- **Founder** (CEO) — owns strategy, priorities, dispatches other managers
- **Builder** (CTO) — builds and ships the product
- **Growth** — finds users/customers, optimizes acquisition
- **Comms** — handles partnerships and external relationships

Each manager spawns domain-tuned subagent workers. Managers communicate through async channels. The human talks only to the Founder.

## Install

Add to your Claude Code settings:

```json
{
  "skills": [
    "/path/to/deploy-team/skills"
  ]
}
```

Or clone and add the path:

```bash
git clone https://github.com/nicholascpark/deploy-team.git
```

## Usage

```bash
cd ~/your-project
# then in Claude Code:
/deploy-team
```

The skill will:
1. Scan your repo (README, tech stack, code structure)
2. Ask 2-3 questions about your product
3. Generate domain-tuned managers, workers, channels, and CLAUDE.md
4. Ask you to review before writing
5. Commit

## What Gets Created

```
.claude/agents/
  founder/          — CEO, strategy, executive authority
    workers/        — researcher, analyst
  builder/          — CTO, product, technical execution
    workers/        — {tech-stack-specific workers}
  growth/           — acquisition, channels, conversion
    workers/        — {business-archetype-specific workers}
  comms/            — partnerships, external relationships
    workers/        — partnership-researcher, etc.
channels/           — async inter-manager communication
CLAUDE.md           — "You are the Founder" (merged with existing)
capabilities.md     — what actions the team can take
```

## Architecture

Based on management science research (Ringelmann effect, Amazon two-pizza teams, YC founding team studies):

- **4 managers** — optimal team size, clear ownership, no diffused responsibility
- **Workers are subagents** — spawned per task, single-purpose, stateless
- **Channels for communication** — async, timestamped, append-only markdown files
- **Founder has executive authority** — can halt any manager, access any department's workers
- **No rigid cycles** — the Founder reads the room and acts

## License

MIT
