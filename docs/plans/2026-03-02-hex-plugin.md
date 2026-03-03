# Hex Plugin Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use hex:executing-plans to implement this plan task-by-task.

**Goal:** Create a Claude Code plugin called "hex" that distills the superpowers plugin (14 skills, 1 agent, 3 commands, 1 hook) into a leaner version with every skill capped at 200 lines.

**Architecture:** 1:1 mapping with superpowers. Same philosophy (TDD, root-cause debugging, verification iron laws). Same workflow pipeline. Every skill preserves its hard gates and iron laws but strips verbose examples, redundant explanations, and oversized rationalization tables.

**Tech Stack:** Markdown skills, JSON manifest, bash hook

---

### Task 1: Scaffold Directory Structure and Plugin Manifest

**Files:**
- Create: `.claude-plugin/plugin.json`
- Create: `LICENSE`

**Step 1: Create all directories**

```bash
mkdir -p .claude-plugin skills/using-hex skills/brainstorming skills/writing-plans skills/writing-skills skills/executing-plans skills/subagent-driven-development skills/dispatching-parallel-agents skills/test-driven-development skills/systematic-debugging skills/verification-before-completion skills/using-git-worktrees skills/finishing-a-development-branch skills/requesting-code-review skills/receiving-code-review agents commands hooks
```

**Step 2: Create plugin.json**

```json
{
  "name": "hex",
  "description": "Distilled development discipline: TDD, debugging, verification, and collaboration patterns",
  "version": "1.0.0",
  "author": {
    "name": "ltj"
  },
  "license": "MIT",
  "keywords": ["skills", "tdd", "debugging", "collaboration", "best-practices", "workflows"]
}
```

**Step 3: Create LICENSE**

MIT license with current year and author.

**Step 4: Commit**

```bash
git init
git add .claude-plugin/plugin.json LICENSE
git commit -m "feat: scaffold hex plugin with manifest and license"
```

---

### Task 2: Create using-hex Skill

**Files:**
- Create: `skills/using-hex/SKILL.md`

**Distill from:** superpowers `using-superpowers` (96 lines → ≤200 lines)

**What to preserve:**
- EXTREMELY-IMPORTANT gate: skills MUST be invoked, not optional
- The Rule: invoke skills BEFORE any response
- Skill priority order (process first, implementation second)
- Skill types (rigid vs flexible)
- Red flags rationalization table (trim to 6-8 most important)

**What to cut:**
- Dot graph (replace with bullet flow)
- "How to Access Skills" section (obvious)
- TodoWrite references (use TaskCreate instead)

**Key constraint:** All internal references say "hex:" not "superpowers:"

**Commit:**
```bash
git add skills/using-hex/SKILL.md
git commit -m "feat: add using-hex skill"
```

---

### Task 3: Create brainstorming Skill

**Files:**
- Create: `skills/brainstorming/SKILL.md`

**Distill from:** superpowers `brainstorming` (97 lines → ≤200 lines)

**What to preserve:**
- HARD-GATE: no implementation until design approved
- Anti-pattern: "too simple to need a design"
- Checklist (6 steps)
- Process sections: understanding, exploring approaches, presenting design
- Terminal state: invoke writing-plans only
- Key principles (one question at a time, YAGNI, etc.)

**What to cut:**
- Dot graph (checklist is sufficient)
- "elements-of-style" reference (not in hex)

**Key constraint:** References say "hex:writing-plans"

**Commit:**
```bash
git add skills/brainstorming/SKILL.md
git commit -m "feat: add brainstorming skill"
```

---

### Task 4: Create writing-plans Skill

**Files:**
- Create: `skills/writing-plans/SKILL.md`

**Distill from:** superpowers `writing-plans` (117 lines → ≤200 lines)

**What to preserve:**
- Overview philosophy (zero context engineer, bite-sized tasks)
- Plan document header template (update to reference hex:executing-plans)
- Task structure template (files, steps, test-first)
- Bite-sized granularity examples
- Execution handoff (subagent-driven vs parallel session)
- Remember list

**What to cut:**
- Nothing major — already 117 lines

**Key constraint:** All skill references use "hex:" prefix

**Commit:**
```bash
git add skills/writing-plans/SKILL.md
git commit -m "feat: add writing-plans skill"
```

---

### Task 5: Create writing-skills Skill

**Files:**
- Create: `skills/writing-skills/SKILL.md`

**Distill from:** superpowers `writing-skills` (656 lines → ≤200 lines)

**What to preserve:**
- Core concept: writing skills IS TDD for documentation
- TDD mapping table
- When to create / don't create
- SKILL.md structure (frontmatter rules, section template)
- CSO: description = triggering conditions only, never workflow summary
- Iron law: no skill without failing test first
- RED-GREEN-REFACTOR cycle for skills (3 sentences each)
- Skill creation checklist (compressed)

**What to cut:**
- Extended CSO examples (keep 1 good/bad pair)
- Token efficiency section (summarize in 2 lines)
- Flowchart usage section
- Code examples section
- File organization section (keep 1-line rule)
- Testing all skill types section (keep as compact table)
- Bulletproofing/persuasion section
- Anti-patterns section
- Discovery workflow section
- Common rationalizations table (trim to 4 rows)

**Key constraint:** This is the biggest cut (656 → 200). Focus on what Claude needs to CREATE a skill, not all the meta-guidance.

**Commit:**
```bash
git add skills/writing-skills/SKILL.md
git commit -m "feat: add writing-skills skill"
```

---

### Task 6: Create executing-plans Skill

**Files:**
- Create: `skills/executing-plans/SKILL.md`

**Distill from:** superpowers `executing-plans` (85 lines → ≤200 lines)

**What to preserve:**
- All 5 steps (load/review, execute batch, report, continue, complete)
- Default batch size: 3 tasks
- When to stop and ask for help
- When to revisit earlier steps
- Remember list
- Integration references (hex:using-git-worktrees, hex:writing-plans, hex:finishing-a-development-branch)

**What to cut:**
- Nothing major — already 85 lines, well within budget

**Key constraint:** All skill references use "hex:" prefix

**Commit:**
```bash
git add skills/executing-plans/SKILL.md
git commit -m "feat: add executing-plans skill"
```

---

### Task 7: Create subagent-driven-development Skill

**Files:**
- Create: `skills/subagent-driven-development/SKILL.md`

**Distill from:** superpowers `subagent-driven-development` (242 lines → ≤200 lines)

**What to preserve:**
- Core principle: fresh subagent per task + two-stage review
- When to use decision (vs executing-plans)
- The process flow (dispatch implementer → spec review → code quality review → next task)
- Red flags list (trim to essential items)
- Integration references

**What to cut:**
- Dot graphs (replace with numbered process list)
- Full example workflow (trim to 10-line summary)
- Advantages section (unnecessary — the skill speaks for itself)
- Efficiency/cost analysis sections
- Prompt template file references (inline the key instructions)

**Key constraint:** The two-stage review (spec then quality) must be crystal clear without the dot graph.

**Commit:**
```bash
git add skills/subagent-driven-development/SKILL.md
git commit -m "feat: add subagent-driven-development skill"
```

---

### Task 8: Create dispatching-parallel-agents Skill

**Files:**
- Create: `skills/dispatching-parallel-agents/SKILL.md`

**Distill from:** superpowers `dispatching-parallel-agents` (181 lines → ≤200 lines)

**What to preserve:**
- Core principle: one agent per independent problem domain
- When to use / don't use
- The pattern (identify domains, create tasks, dispatch, review/integrate)
- Agent prompt structure (focused, self-contained, specific output)
- Common mistakes (4 pairs)
- Verification steps after agents return

**What to cut:**
- Dot graph (replace with bullet checklist)
- Real example section (trim to 5 lines or remove)
- Key benefits section (obvious)
- Real-world impact section

**Key constraint:** Already near 200 lines, minor trim needed.

**Commit:**
```bash
git add skills/dispatching-parallel-agents/SKILL.md
git commit -m "feat: add dispatching-parallel-agents skill"
```

---

### Task 9: Create test-driven-development Skill

**Files:**
- Create: `skills/test-driven-development/SKILL.md`

**Distill from:** superpowers `test-driven-development` (371 lines → ≤200 lines)

**What to preserve:**
- Iron law: NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
- "Violating the letter is violating the spirit"
- Red-Green-Refactor cycle with good/bad examples (trim to 1 each)
- Verify RED / Verify GREEN as mandatory steps
- When to use (always, exceptions only with human permission)
- Good tests table
- Common rationalizations table (trim to 6 most important)
- Red flags list (trim to 8 items)
- Verification checklist
- Bug fix example (compact)

**What to cut:**
- Dot graph
- "Why Order Matters" extended arguments (keep 2-line summary per excuse)
- Bad code example in GREEN (keep only good)
- When Stuck table (keep 2 rows)
- Debugging integration section (1 line)
- Testing anti-patterns reference
- "Final Rule" section (redundant with Iron Law)

**Key constraint:** Hardest distillation after writing-skills. Every iron law and gate stays, but the persuasion essays get compressed to their conclusions.

**Commit:**
```bash
git add skills/test-driven-development/SKILL.md
git commit -m "feat: add test-driven-development skill"
```

---

### Task 10: Create systematic-debugging Skill

**Files:**
- Create: `skills/systematic-debugging/SKILL.md`

**Distill from:** superpowers `systematic-debugging` (297 lines → ≤200 lines)

**What to preserve:**
- Iron law: NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
- Four phases (root cause, pattern analysis, hypothesis, implementation)
- Phase 1 sub-steps (read errors, reproduce, check changes, gather evidence, trace data flow)
- Phase 4 "3+ fixes failed → question architecture" rule
- Red flags list (trim to 8 items)
- Quick reference table
- Common rationalizations (trim to 5 rows)

**What to cut:**
- Multi-component diagnostic example (trim to 3-line concept)
- "Your human partner's signals" section
- Supporting techniques references (keep 1 line)
- "When process reveals no root cause" section
- Real-world impact section

**Commit:**
```bash
git add skills/systematic-debugging/SKILL.md
git commit -m "feat: add systematic-debugging skill"
```

---

### Task 11: Create verification-before-completion Skill

**Files:**
- Create: `skills/verification-before-completion/SKILL.md`

**Distill from:** superpowers `verification-before-completion` (140 lines → ≤200 lines)

**What to preserve:**
- Iron law: NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
- The gate function (5-step process)
- Common failures table
- Red flags list
- Key patterns (tests, regression, build, requirements, agent delegation)
- Rationalization prevention table
- When to apply list

**What to cut:**
- "Why this matters" section (motivational, not operational)
- Already within budget, minor consolidation only

**Commit:**
```bash
git add skills/verification-before-completion/SKILL.md
git commit -m "feat: add verification-before-completion skill"
```

---

### Task 12: Create using-git-worktrees Skill

**Files:**
- Create: `skills/using-git-worktrees/SKILL.md`

**Distill from:** superpowers `using-git-worktrees` (218 lines → ≤200 lines)

**What to preserve:**
- Directory selection priority (existing → CLAUDE.md → ask user)
- Safety verification (must verify directory is ignored)
- Creation steps (detect project, create worktree, run setup, verify baseline, report)
- Quick reference table
- Auto-detect setup (npm/cargo/pip/go)
- Red flags and common mistakes

**What to cut:**
- Example workflow (trim to 3 lines)
- Common mistakes section (merge into red flags)
- Verbose bash code blocks (trim to essential commands)

**Key constraint:** Global directory references change from `~/.config/superpowers/` to `~/.config/hex/`

**Commit:**
```bash
git add skills/using-git-worktrees/SKILL.md
git commit -m "feat: add using-git-worktrees skill"
```

---

### Task 13: Create finishing-a-development-branch Skill

**Files:**
- Create: `skills/finishing-a-development-branch/SKILL.md`

**Distill from:** superpowers `finishing-a-development-branch` (201 lines → ≤200 lines)

**What to preserve:**
- Process: verify tests → present 4 options → execute → cleanup
- All 4 options with their bash commands
- Quick reference table
- Worktree cleanup rules (only Options 1 & 4)
- Typed "discard" confirmation for Option 4
- Red flags

**What to cut:**
- Common mistakes section (merge key points into red flags)
- Trim bash examples slightly

**Key constraint:** Already at 201 lines — needs only minor trim.

**Commit:**
```bash
git add skills/finishing-a-development-branch/SKILL.md
git commit -m "feat: add finishing-a-development-branch skill"
```

---

### Task 14: Create requesting-code-review Skill

**Files:**
- Create: `skills/requesting-code-review/SKILL.md`

**Distill from:** superpowers `requesting-code-review` (106 lines → ≤200 lines)

**What to preserve:**
- When to request (mandatory vs optional)
- How to request (get SHAs, dispatch reviewer, act on feedback)
- Example workflow
- Integration with workflows
- Red flags

**What to cut:**
- Already well within budget — only rename references

**Key constraint:** References say "hex:code-reviewer"

**Commit:**
```bash
git add skills/requesting-code-review/SKILL.md
git commit -m "feat: add requesting-code-review skill"
```

---

### Task 15: Create receiving-code-review Skill

**Files:**
- Create: `skills/receiving-code-review/SKILL.md`

**Distill from:** superpowers `receiving-code-review` (213 lines → ≤200 lines)

**What to preserve:**
- Response pattern (read, understand, verify, evaluate, respond, implement)
- Forbidden responses (no performative agreement)
- Handling unclear feedback (stop and ask)
- Source-specific handling (human partner vs external)
- YAGNI check
- Implementation order
- When to push back
- Acknowledging correct feedback (fix it, don't thank)

**What to cut:**
- Real examples section (trim to 1)
- Gracefully correcting pushback section (2 lines)
- GitHub thread replies section (1 line)
- Common mistakes table (merge into response pattern)

**Commit:**
```bash
git add skills/receiving-code-review/SKILL.md
git commit -m "feat: add receiving-code-review skill"
```

---

### Task 16: Create code-reviewer Agent

**Files:**
- Create: `agents/code-reviewer.md`

**Distill from:** superpowers `code-reviewer` agent (~60 lines → ≤60 lines)

**What to preserve:**
- Agent frontmatter (name, description with examples, model: inherit)
- System prompt: 6 responsibilities (plan alignment, code quality, architecture, documentation, issue identification, communication)
- Issue categorization: Critical/Important/Suggestions

**What to cut:**
- Verbose descriptions of each responsibility (compress to 1-2 lines each)

**Commit:**
```bash
git add agents/code-reviewer.md
git commit -m "feat: add code-reviewer agent"
```

---

### Task 17: Create Commands

**Files:**
- Create: `commands/brainstorm.md`
- Create: `commands/write-plan.md`
- Create: `commands/execute-plan.md`

**Each command follows same pattern:**

```markdown
---
description: "<brief description>"
disable-model-invocation: true
---

Invoke the hex:<skill-name> skill and follow it exactly as presented to you
```

Commands:
- `brainstorm.md` → hex:brainstorming
- `write-plan.md` → hex:writing-plans
- `execute-plan.md` → hex:executing-plans

**Commit:**
```bash
git add commands/
git commit -m "feat: add slash commands"
```

---

### Task 18: Create SessionStart Hook

**Files:**
- Create: `hooks/hooks.json`
- Create: `hooks/session-start`
- Create: `hooks/run-hook.cmd`

**hooks.json:**
```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "startup|resume|clear|compact",
        "hooks": [
          {
            "type": "command",
            "command": "'${CLAUDE_PLUGIN_ROOT}/hooks/run-hook.cmd' session-start",
            "async": false
          }
        ]
      }
    ]
  }
}
```

**session-start:** Adapted from superpowers version:
- Remove legacy skills directory warning (not applicable to hex)
- Read using-hex skill content
- Inject as session context with "You have hex." framing
- Keep JSON escaping function
- Keep dual-format output (additional_context + hookSpecificOutput)

**run-hook.cmd:** Copy from superpowers (cross-platform polyglot wrapper, unchanged).

**Step: Make session-start executable**
```bash
chmod +x hooks/session-start
```

**Commit:**
```bash
git add hooks/
git commit -m "feat: add SessionStart hook"
```

---

### Task 19: Final Verification

**Step 1: Verify all files exist**
```bash
find . -type f | sort
```

Expected: 22 files (plugin.json, LICENSE, 14 skills, 1 agent, 3 commands, 3 hook files)

**Step 2: Verify no skill exceeds 200 lines**
```bash
wc -l skills/*/SKILL.md
```

Expected: Every file ≤ 200 lines

**Step 3: Verify all internal references use "hex:" prefix**
```bash
grep -r "superpowers:" skills/ agents/ commands/ hooks/
```

Expected: 0 matches

**Step 4: Verify plugin.json is valid JSON**
```bash
python3 -c "import json; json.load(open('.claude-plugin/plugin.json'))"
```

**Step 5: Final commit if any fixes needed**
