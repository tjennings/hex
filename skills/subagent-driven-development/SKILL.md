---
name: subagent-driven-development
description: Use when executing implementation plans with independent tasks in the current session
---

# Subagent-Driven Development

Execute plan by dispatching fresh subagent per task, with two-stage review after each: spec compliance review first, then code quality review.

**Core principle:** Fresh subagent per task + two-stage review (spec then quality) = high quality, fast iteration

## When to Use

Ask yourself three questions:
1. **Have implementation plan?** No -> Manual execution or brainstorm first
2. **Tasks mostly independent?** No (tightly coupled) -> Manual execution or brainstorm first
3. **Stay in this session?** No (parallel session) -> hex:executing-plans

All yes -> **subagent-driven-development**

**vs. Executing Plans (parallel session):**
- Same session (no context switch)
- Fresh subagent per task (no context pollution)
- Two-stage review after each task: spec compliance first, then code quality
- Faster iteration (no human-in-loop between tasks)

## The Process

1. **Read plan** - Extract all tasks with full text, note context, create TaskCreate items
2. **Per task** (sequential, one at a time):
   a. Dispatch implementer subagent (`./implementer-prompt.md`) with full task text + context
   b. If implementer asks questions -> answer clearly, provide context, re-dispatch
   c. Implementer implements, tests, commits, self-reviews
   d. Dispatch spec reviewer (`./spec-reviewer-prompt.md`)
   e. If spec issues found -> implementer fixes -> re-dispatch spec reviewer -> repeat until pass
   f. Dispatch code quality reviewer (`./code-quality-reviewer-prompt.md`)
   g. If quality issues found -> implementer fixes -> re-dispatch quality reviewer -> repeat until pass
   h. Mark task complete in TaskCreate -> next task
3. **After all tasks**: Dispatch final code reviewer for entire implementation
4. **Finish**: Use `hex:finishing-a-development-branch`

## Prompt Templates

- `./implementer-prompt.md` - Dispatch implementer subagent
- `./spec-reviewer-prompt.md` - Dispatch spec compliance reviewer subagent
- `./code-quality-reviewer-prompt.md` - Dispatch code quality reviewer subagent

## Example Workflow (Summary)

1. Read plan, extract 5 tasks, create TaskCreate
2. Per task: dispatch implementer -> answer any questions -> implementer implements/tests/commits
3. Spec reviewer checks compliance; implementer fixes any gaps; re-review until approved
4. Code quality reviewer checks; implementer fixes any issues; re-review until approved
5. After all tasks: final code reviewer confirms everything, then `hex:finishing-a-development-branch`

## Red Flags

**Never:**
- Start implementation on main/master without explicit user consent
- Skip reviews (spec compliance OR code quality)
- Proceed with unfixed issues
- Dispatch multiple implementation subagents in parallel (conflicts)
- Make subagent read plan file (provide full text instead)
- Skip scene-setting context (subagent needs to understand where task fits)
- Ignore subagent questions (answer before letting them proceed)
- Accept "close enough" on spec compliance (issues found = not done)
- Skip review loops (issues found = implementer fixes = review again)
- Let implementer self-review replace actual review (both are needed)
- **Start code quality review before spec compliance is approved** (wrong order)
- Move to next task while either review has open issues

**If subagent asks questions:**
- Answer clearly and completely
- Provide additional context if needed
- Don't rush them into implementation

**If reviewer finds issues:**
- Implementer (same subagent) fixes them
- Reviewer reviews again; repeat until approved
- Don't skip the re-review

**If subagent fails task:**
- Dispatch fix subagent with specific instructions
- Don't try to fix manually (context pollution)

## Integration

**Required workflow skills:**
- **hex:using-git-worktrees** - REQUIRED: Set up isolated workspace before starting
- **hex:writing-plans** - Creates the plan this skill executes
- **hex:requesting-code-review** - Code review template for reviewer subagents
- **hex:finishing-a-development-branch** - Complete development after all tasks

**Subagents should use:**
- **hex:test-driven-development** - Subagents follow TDD for each task

**Alternative workflow:**
- **hex:executing-plans** - Use for parallel session instead of same-session execution
