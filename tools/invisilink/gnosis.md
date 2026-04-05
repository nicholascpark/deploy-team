---
name: gnosis
version: "1.0"
description: Establish umbilical connection to the parent (business-machine) for pre-existing repos. Run after the parent has blessed this repo. Not for incubator-spawned ventures.
user-invocable: true
command: /gnosis
---

# Gnosis

Establish an umbilical connection between this repo and the parent (business-machine). This creates the communication directories, writes the configuration, registers the venture in the parent's `ventures.md`, and installs the `/flash` upgrade stub.

**Scope:** This skill is exclusively for pre-existing repos that were NOT spawned by the parent's incubator. Incubator-spawned ventures are born linked — they already have umbilical directories, a `ventures.md` entry, `umbilical/config.md`, and a `/flash` stub. If this venture is already linked or parent-spawned, run `/flash` instead.

**Prerequisite:** The parent must have run `/bless` on this repo first. That delivers this skill. If you're reading this, the blessing already happened.

## Process

### Step 1: Guard — Reject if Already Linked

Check for `umbilical/config.md`. If it exists, read it.

- If it contains `linked: true` → STOP. Tell the user: "This venture is already linked to the parent. Run `/flash` to upgrade." Do not proceed.

### Step 2: Guard — Reject if Incubator-Spawned

Check for ALL THREE of:
- `.claude/agents/founder/founder.md`
- `umbilical/inbox/`
- `umbilical/outbox/`

If all three exist, this is likely an incubator-spawned venture. STOP. Tell the user: "This venture appears to be parent-spawned. Run `/flash` to upgrade. `/gnosis` is for pre-existing repos only."

### Step 3: Create Umbilical Directories

```bash
mkdir -p umbilical/inbox umbilical/outbox
```

### Step 4: Confirm Parent Path

Ask the user: "Parent repo path? (default: ~/Documents/business-machine)"

Allow override. Store the confirmed path for subsequent steps.

### Step 5: Validate Parent

Check that the confirmed path:
1. Exists and is a directory
2. Contains `ventures.md`
3. Contains `tools/invisilink/flash.md`

If any check fails, report the specific error and stop.

### Step 6: Check for Existing Registration

Read the parent's `ventures.md`. Search for this venture's name or path.

If the venture already has an entry, STOP. Tell the user: "This venture is already registered in the parent's ventures.md. Run `/flash` to upgrade."

### Step 7: Register in Parent's ventures.md

Append an entry to `{parent_path}/ventures.md`:

```markdown
## {Venture Name}
- repo: {absolute path to this venture repo}
- spawned: pre-existing
- linked: {today's date in YYYY-MM-DD}
- source: gnosis (blessed by parent)
- current_phase: unknown
- status: linked
- last_sync: {today's date in YYYY-MM-DD}
```

### Step 8: Write Umbilical Config

Write `umbilical/config.md` in this repo:

```markdown
# Umbilical Configuration
- parent_path: {confirmed parent path}
- linked: true
- linked_date: {today's date in YYYY-MM-DD}
```

### Step 9: Write Local `/flash` Stub

Create the directory if needed:

```bash
mkdir -p .claude/skills/invisilink
```

Write `.claude/skills/invisilink/flash.md`:

```markdown
---
name: flash
command: /flash
---
# Flash

Read and follow the instructions at `{confirmed parent path}/tools/invisilink/flash.md`.
```

### Step 10: Report

Tell the user:

> "Gnosis complete. Umbilical established. This venture is now linked to the parent. Run `/flash` to deploy or upgrade infrastructure."

## What Gnosis Does NOT Do

- Deliver catch-up learnings — the parent's Learning Curator handles that on its next run
- Modify existing agents or skills beyond writing the `/flash` stub
- Run `/flash` automatically — the human decides when
- Work on incubator-spawned or already-linked ventures — those use `/flash` directly
