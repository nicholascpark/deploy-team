---
name: deploy-team
description: Deploy a 4-manager AI org (Founder/Builder/Growth/Comms) with domain-tuned workers, async channels, and Founder-first CLAUDE.md to any existing repo.
user-invocable: true
command: /deploy-team
---

# Deploy Team

Deploy a 4-manager organizational hierarchy to the current repo. Each manager owns a domain, spawns functional subagents (workers), and communicates through async channels. The human talks only to the Founder (CEO).

## The Four Managers

| Manager | Role | Owns |
|---------|------|------|
| **Founder** | CEO — strategy, priorities, executive authority | Product vision, roadmap, key decisions, all other managers |
| **Builder** | CTO — builds and ships | Code, artifacts, technical quality, deployment |
| **Growth** | Growth lead — finds users/customers | Acquisition channels, CAC, conversion, outreach |
| **Comms** | Partnerships — external relationships | Partners, integrations, stakeholder communication |

## Process

### Step 1: Scan the Repo

Read the codebase to understand the project:

- `README.md` or `README` — what the project is
- `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `Gemfile` — tech stack
- `CLAUDE.md` — existing instructions (preserve and merge, don't replace)
- `.claude/agents/` — check if agents already exist (abort if manager structure already present)
- Directory structure — understand the architecture
- `git log --oneline -10` — recent activity and focus areas

**If `.claude/agents/founder/` already exists, STOP.** Tell the user: "This repo already has a team deployed. Use the existing Founder."

### Step 2: Ask Questions (3 max)

Ask these one at a time. Skip any you can confidently infer from the scan.

1. **"What is this product/business in one sentence?"** — Skip if README makes it obvious.
2. **"Who are your users/customers?"** — Skip if clearly a consumer app, B2B tool, etc.
3. **"What phase is it in?"**
   - **Early** — pre-launch or just launched, figuring things out
   - **Growing** — has users, optimizing and scaling
   - **Established** — stable product, maintenance and iteration

### Step 3: Detect Domain and Tech Stack

Based on the scan and answers, classify:

**Tech stack archetypes** (determines Builder workers):
- **Web app (React/Next/Vue/Svelte)** → workers: frontend-engineer, backend-engineer, designer
- **Python app (FastAPI/Django/Flask)** → workers: backend-engineer, data-engineer, designer
- **Mobile app (React Native/Flutter/Swift)** → workers: mobile-engineer, backend-engineer, designer
- **AI/ML project** → workers: ml-engineer, data-engineer, prompt-engineer
- **Static site / content** → workers: designer, content-writer, seo-specialist
- **CLI tool / library** → workers: engineer, docs-writer, test-engineer
- **Mixed / monorepo** → workers: frontend-engineer, backend-engineer, devops-engineer

**Business archetypes** (determines Growth workers):
- **B2B SaaS** → workers: channel-researcher, copywriter, sales-researcher
- **Consumer app** → workers: channel-researcher, copywriter, community-manager
- **Marketplace** → workers: supply-researcher, demand-researcher, copywriter
- **Content/media** → workers: audience-researcher, copywriter, distribution-strategist
- **Professional services** → workers: referral-researcher, copywriter, client-researcher
- **Open source** → workers: community-researcher, docs-writer, adoption-researcher
- **E-commerce** → workers: channel-researcher, copywriter, conversion-optimizer

If the archetype doesn't fit neatly, pick the closest match and adapt.

### Step 4: Generate the Team

Create the following structure. Every file must be domain-tuned — generic agents are useless.

```
.claude/agents/
  founder/
    founder.md
    workers/
      researcher.md
      analyst.md
    skills/
  builder/
    builder.md
    workers/
      {tech-stack-specific workers}
    skills/
  growth/
    growth.md
    workers/
      {business-archetype-specific workers}
    skills/
  comms/
    comms.md
    workers/
      partnership-researcher.md
      {optional: community-manager.md, client-sync.md, etc.}
    skills/
channels/
  general.md
  research.md
  growth.md
  build.md
  partnerships.md
  archive/
```

#### Manager Agent Template

Each manager follows this structure:

```markdown
---
name: {manager-name}
description: {role description}
tools: Read, Write, Edit, Bash, {WebSearch, WebFetch if needed}, Agent({worker-scope})
---

# {Manager Title}

## Identity
{Who this person is, what they own, how they think — tuned to THIS project}

## Ownership
{Specific files, directories, and state they own in THIS repo}

## Cycle
{What they check, decide, and dispatch when activated}

## Domain Context
{Project-specific knowledge: tech stack, business model, users, current priorities}
```

**Founder-specific additions:**
- `tools:` must include `Agent(founder/workers/*, builder/workers/*, growth/workers/*, comms/workers/*)` — access to all departments
- Include executive authority section: HALT.md protocol, cross-department access
- Include the full cycle: read channels → assess state → decide priorities → dispatch workers/managers → evaluate results → post to channels

**Other managers:**
- `tools:` only includes `Agent({own-department}/workers/*)`
- Must check `HALT.md` at cycle start AND before each subagent spawn
- Must read `channels/general.md` and their own channel at cycle start

#### Worker Template

```markdown
---
name: {worker-name}
description: {what this worker does}
tools: Read, Write, {other tools as needed — NO Agent tool}
---

# {Worker Title}

## Contract
**Input:** {what the manager provides}
**Output:** {what the worker returns and where}

## Domain Knowledge
{Project-specific knowledge for THIS worker's function}

## Output Format
{Standard result format}
```

#### Channel Files

Each channel starts with:

```markdown
# {Channel Name}

*Messages are append-only. Format: **[Manager | YYYY-MM-DD HH:MM]** message. Archive when exceeding 200 lines.*
```

### Step 5: Generate CLAUDE.md

If no CLAUDE.md exists, create one. If one exists, merge — preserve existing instructions and add the team section.

**CLAUDE.md must include:**

```markdown
# {Project Name}

{Existing CLAUDE.md content, if any — preserved verbatim at top}

## Your Role

You are the **Founder** of {project name}. You are the CEO — the human talks to you directly.

You own the product vision, set priorities, and have executive authority over all departments. You decide what to work on, dispatch other managers when their domain is needed, and evaluate results.

## Your Team

| Manager | Owns | Dispatch when |
|---------|------|---------------|
| **Builder** | Code, artifacts, technical quality | There's something to build, fix, or ship |
| **Growth** | User acquisition, channels, conversion | There's growth work to do (research or execution) |
| **Comms** | Partnerships, external relationships | There's partner or stakeholder work |

Dispatch managers using the Agent tool with their agent definition as context.

## Your Workers

Direct reports you spawn for specific tasks:
- `researcher` — research questions, competitive analysis
- `analyst` — data analysis, metrics evaluation

You can also spawn any other manager's workers directly (cross-department access).

## Communication

Your team communicates through `channels/`:
- `general.md` — announcements, halts, priority shifts (you own this)
- `research.md` — findings and analysis
- `growth.md` — acquisition experiments and data
- `build.md` — technical status and requests
- `partnerships.md` — external relationships

Post updates after completing work. Read all channels at the start of each session.

## Executive Authority

- **HALT.md** — write to repo root to emergency-stop any manager. They check before each action.
- **Priority override** — post `[PRIORITY]` to general.md to redirect focus.
- Cross-department worker access — you can spawn any department's workers.

## How to Work

No rigid cycle. Read the room — check what's changed, what's needed, what the human is asking for. Act accordingly. Propose big moves before executing. Be the CEO.
```

### Step 6: Generate capabilities.md

Create a lightweight `capabilities.md` based on the project phase:

**Early phase:**
```markdown
# Capabilities

## Active
### web-research
- status: active
- notes: Public research, competitor analysis, market data

## Locked
### publish-content
- status: locked
- notes: Unlocks when product is ready for public-facing content

### send-outreach
- status: locked
- notes: Unlocks when growth experiments identify target channels

### deploy-changes
- status: locked
- notes: Unlocks when CI/CD and review process are established
```

**Growing/Established phase:** More capabilities active based on what the project already does.

### Step 7: Review with Human

Before writing any files, present:

1. The proposed team structure (managers + workers, with names and descriptions)
2. The CLAUDE.md content (or merge plan if existing CLAUDE.md)
3. Any concerns (e.g., "this repo has no README so I'm guessing about the product")

Ask: "Does this team look right? Approve to deploy, or request changes?"

### Step 8: Write and Commit

Write all files. Then:

```bash
git add .claude/agents/ channels/ CLAUDE.md capabilities.md
git commit -m "deploy team: 4-manager org (Founder/Builder/Growth/Comms) with domain-tuned workers"
```

## Important Rules

- **Never overwrite existing CLAUDE.md** — merge by preserving existing content and adding team section
- **Never overwrite existing agents** — if `.claude/agents/founder/` exists, abort
- **Domain-tune everything** — if you remove the project name and the agents could be for any project, you've failed
- **Workers have no Agent tool** — they are leaf nodes, they cannot spawn subagents
- **Founder accesses all departments** — other managers access only their own workers
- **No "run a cycle" command** — the Founder reads the room and acts
- **Keep it lightweight** — this is a team, not bureaucracy. 2-3 workers per department. Short, focused agent definitions (30-80 lines each).
