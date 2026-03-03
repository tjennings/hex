# Subagent-Driven Development Redesign

## Summary

Rewrite `subagent-driven-development` to exclusively use agent teams (TeamCreate/SendMessage/TaskCreate). Absorb `dispatching-parallel-agents` into this skill. Default up to 3 parallel streams.

## Skill Identity

- **Name:** `subagent-driven-development` (unchanged)
- **Description:** "Use when executing implementation plans or facing 2+ independent tasks that benefit from parallel work"
- **Replaces:** `dispatching-parallel-agents` (deleted)

## Team Structure

One team created per execution. Composition scales to task count:

| Tasks | Streams | Team size |
|-|-|-|
| 1 | 1 | 1 lead + 1 implementer + 1 reviewer = 3 |
| 2 | 2 | 1 lead + 2 implementers + 2 reviewers = 5 |
| 3+ | 3 | 1 lead + 3 implementers + 3 reviewers = 7 |

### Agent Type Selection

Before creating the team, the lead checks for language-specialized agents by scanning project context (file extensions, CLAUDE.md, plan content):

1. Check if specialized agents exist (e.g. `rust-developer`, `rust-perf-reviewer`)
2. If found → use as `subagent_type` for implementers/reviewers
3. If not found → use `general-purpose` for implementers, `code-reviewer` for reviewers

### Roles

- **Lead (you):** Creates team, detects language/agent types, creates tasks, assigns work, assigns next task when streams complete, runs final verification
- **Implementer:** Receives task from lead, implements with TDD, commits, notifies paired reviewer
- **Reviewer:** Spec compliance → code quality → `/simplify`. Coordinates fixes directly with implementer. Notifies lead when approved.

### Naming Convention

- `impl-1`, `impl-2`, `impl-3`
- `reviewer-1`, `reviewer-2`, `reviewer-3`
- Implementer N pairs with reviewer N

## The Process

### Step 1 — Gather tasks
- If plan exists: read plan, extract tasks with full text
- If ad-hoc: collect task descriptions from user
- Create TaskCreate items for all tasks

### Step 2 — Detect agent types
- Scan project for language indicators (file extensions, CLAUDE.md, plan content)
- Check if specialized agents exist
- Fall back to `general-purpose` / `code-reviewer` defaults

### Step 3 — Create team & spawn streams
- `TeamCreate` with descriptive name
- Spawn implementer + reviewer pairs (up to 3 streams)
- Each pair uses detected agent types

### Step 4 — Assign initial tasks
- Lead assigns one task per stream via `SendMessage` to implementers
- Include full task text + plan context + which reviewer is their pair

### Step 5 — Stream loop (autonomous)
Within each stream, without lead involvement:
1. Implementer implements with TDD, commits, messages their reviewer
2. Reviewer checks spec compliance → if issues, messages implementer → implementer fixes → reviewer re-checks → repeat until pass
3. Reviewer checks code quality → same fix loop
4. Reviewer runs `/simplify` → same fix loop
5. Reviewer messages lead: "task approved"

### Step 6 — Reassignment
- When lead receives approval from a stream, assigns next unfinished task to that stream's implementer
- Repeat until all tasks complete

### Step 7 — Final
- Lead runs final verification across all work
- `hex:finishing-a-development-branch`
- Shut down team

## Red Flags

**Never:**
- Skip team creation (no standalone Agent calls — always teams)
- Assign more than one task per stream at a time
- Let lead intervene in implementer-reviewer loops (autonomous streams)
- Spawn more than 3 streams regardless of task count
- Start a stream without both implementer and reviewer spawned
- Skip any review stage (spec → quality → simplify, all three required)
- Move to next task while reviewer has open issues
- Let implementer self-review replace dedicated reviewer
- Start quality review before spec compliance passes
- Run `/simplify` before code quality passes

**If an implementer gets stuck:**
- Reviewer can help via direct message
- If both are stuck, they message the lead
- Lead provides context/decisions, doesn't implement

**If streams conflict (edited same files):**
- Lead pauses remaining streams
- Resolves conflicts
- Resumes streams

## Integration

**Required workflow skills:**
- `hex:using-git-worktrees` — set up isolated workspace before starting
- `hex:writing-plans` — creates plans this skill executes (for plan-based work)
- `hex:finishing-a-development-branch` — complete development after all tasks

**Teammates should use:**
- `hex:test-driven-development` — implementers follow TDD
- `hex:verification-before-completion` — implementers verify before notifying reviewer
- `simplify` — reviewers run as final review stage
