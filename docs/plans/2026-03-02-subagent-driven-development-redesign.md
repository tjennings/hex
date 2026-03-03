# Subagent-Driven Development Redesign — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use hex:executing-plans to implement this plan task-by-task.

**Goal:** Rewrite subagent-driven-development to exclusively use agent teams, absorb dispatching-parallel-agents, default up to 3 parallel streams.

**Architecture:** Skill markdown rewrite + deletion of absorbed skill. No code — pure documentation changes to skill files.

**Tech Stack:** Markdown, YAML frontmatter

---

## Design Reference

See sections below for the approved design that each task implements.

---

### Task 1: Rewrite subagent-driven-development SKILL.md

**Files:**
- Modify: `skills/subagent-driven-development/SKILL.md` (full rewrite)

**Step 1: Read the current skill**

Read `skills/subagent-driven-development/SKILL.md` to understand current structure.

**Step 2: Write the new skill**

Replace entire contents of `skills/subagent-driven-development/SKILL.md` with:

```markdown
---
name: subagent-driven-development
description: Use when executing implementation plans or facing 2+ independent tasks that benefit from parallel work
---

# Subagent-Driven Development

Execute tasks by creating an agent team with parallel implementer-reviewer streams. Up to 3 streams run concurrently. Each stream is autonomous: implementer builds, reviewer gates.

**Core principle:** Agent teams + parallel streams + three-stage review (spec → quality → simplify) = high quality, fast throughput

## When to Use

Ask yourself:
1. **Have tasks to execute?** (plan or ad-hoc list) No → brainstorm first
2. **Are tasks mostly independent?** No (tightly coupled, shared state) → manual execution
3. **All yes → subagent-driven-development**

Works for both structured plan execution AND ad-hoc parallel tasks.

**vs. Executing Plans (parallel session):**
- Same session (no context switch)
- Agent teams with parallel streams (not sequential standalone agents)
- Three-stage review per task: spec compliance → code quality → /simplify
- Faster throughput (up to 3 tasks in parallel)

## Team Structure

One team per execution. Streams scale to task count:

| Tasks | Streams | Team size |
|-|-|-|
| 1 | 1 | 1 lead + 1 implementer + 1 reviewer = 3 |
| 2 | 2 | 1 lead + 2 implementers + 2 reviewers = 5 |
| 3+ | 3 (max) | 1 lead + 3 implementers + 3 reviewers = 7 |

### Agent Type Selection

Before creating the team, detect language-specialized agents:

1. Scan project context: file extensions, CLAUDE.md, plan content
2. Check if specialized agents exist (e.g. `rust-developer`, `rust-perf-reviewer`)
3. If found → use as `subagent_type` for implementers/reviewers
4. If not found → use `general-purpose` for implementers, `code-reviewer` for reviewers

### Roles

- **Lead (you):** Creates team, detects agent types, creates tasks, assigns work to streams, assigns next task when streams complete, runs final verification
- **Implementer (impl-1, impl-2, impl-3):** Receives task from lead, implements with TDD (`hex:test-driven-development`), commits, messages paired reviewer
- **Reviewer (reviewer-1, reviewer-2, reviewer-3):** Spec compliance → code quality → `/simplify`. Coordinates fixes directly with paired implementer. Messages lead when approved.

Implementer N pairs with reviewer N. Pairs communicate directly without lead involvement.

## The Process

### Step 1 — Gather tasks
- If plan exists: read plan, extract tasks with full text
- If ad-hoc: collect task descriptions from user
- Create TaskCreate items for all tasks

### Step 2 — Detect agent types
- Scan project for language indicators
- Check if specialized agents exist
- Fall back to `general-purpose` / `code-reviewer` defaults

### Step 3 — Create team & spawn streams
- `TeamCreate` with descriptive name
- Spawn implementer + reviewer pairs (up to 3 streams)
- Each pair uses detected agent types

### Step 4 — Assign initial tasks
- Lead assigns one task per stream via `SendMessage` to implementers
- Include: full task text, plan context, paired reviewer name

### Step 5 — Stream loop (autonomous)
Within each stream, without lead involvement:
1. Implementer implements with TDD, runs `hex:verification-before-completion`, commits, messages their reviewer
2. Reviewer checks spec compliance → if issues, messages implementer → fix loop → repeat until pass
3. Reviewer checks code quality → same fix loop until pass
4. Reviewer runs `/simplify` → same fix loop until pass
5. Reviewer messages lead: "task approved"

### Step 6 — Reassignment
- When lead receives approval, assigns next unfinished task to that stream's implementer
- Repeat until all tasks complete

### Step 7 — Final
- Lead runs final verification across all work
- Use `hex:finishing-a-development-branch`
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

**If implementer gets stuck:**
- Reviewer can help via direct message
- If both stuck, they message the lead
- Lead provides context/decisions, doesn't implement

**If streams conflict (edited same files):**
- Lead pauses remaining streams
- Resolves conflicts
- Resumes streams

## Integration

**Required workflow skills:**
- **hex:using-git-worktrees** — set up isolated workspace before starting
- **hex:writing-plans** — creates plans this skill executes (for plan-based work)
- **hex:finishing-a-development-branch** — complete development after all tasks

**Teammates should use:**
- **hex:test-driven-development** — implementers follow TDD
- **hex:verification-before-completion** — implementers verify before notifying reviewer
- **simplify** — reviewers run as final review stage

**Alternative workflow:**
- **hex:executing-plans** — use for parallel session instead of same-session execution
```

**Step 3: Verify the file**

Read `skills/subagent-driven-development/SKILL.md` and confirm it matches the design doc.

**Step 4: Commit**

```bash
git add skills/subagent-driven-development/SKILL.md
git commit -m "feat: rewrite subagent-driven-development to use agent teams"
```

---

### Task 2: Delete dispatching-parallel-agents skill

**Files:**
- Delete: `skills/dispatching-parallel-agents/SKILL.md`
- Delete: `skills/dispatching-parallel-agents/` (directory)

**Step 1: Delete the skill directory**

```bash
rm -rf skills/dispatching-parallel-agents
```

**Step 2: Verify deletion**

```bash
ls skills/dispatching-parallel-agents 2>&1
```
Expected: "No such file or directory"

**Step 3: Commit**

```bash
git add -A skills/dispatching-parallel-agents
git commit -m "feat: remove dispatching-parallel-agents (absorbed into subagent-driven-development)"
```

---

### Task 3: Update writing-plans skill reference

**Files:**
- Modify: `skills/writing-plans/SKILL.md:107-127`

**Step 1: Read the execution handoff section**

Read `skills/writing-plans/SKILL.md` lines 105-127 to see current wording.

**Step 2: Update the execution handoff**

The section currently says "Subagent-Driven (this session)" with "Fresh subagent per task + code review". Update the description to reflect teams:

Change line 121-122 from:
```
- Stay in this session
- Fresh subagent per task + code review
```
To:
```
- Stay in this session
- Agent teams with parallel implementer-reviewer streams
```

**Step 3: Verify the edit**

Read the modified section and confirm it's accurate.

**Step 4: Commit**

```bash
git add skills/writing-plans/SKILL.md
git commit -m "docs: update writing-plans to reflect team-based subagent-driven-development"
```
