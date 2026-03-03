---
name: writing-skills
description: Use when creating new skills, editing existing skills, or verifying skills work before deployment
---

# Writing Skills

## Overview

**Writing skills IS Test-Driven Development applied to process documentation.**

You write test cases (pressure scenarios with subagents), watch them fail (baseline behavior), write the skill (documentation), watch tests pass (agents comply), and refactor (close loopholes).

**Core principle:** If you didn't watch an agent fail without the skill, you don't know if the skill teaches the right thing.

**REQUIRED BACKGROUND:** You MUST understand hex:test-driven-development before using this skill.

## TDD Mapping for Skills

| TDD Concept | Skill Creation |
|-|-|
| **Test case** | Pressure scenario with subagent |
| **Production code** | Skill document (SKILL.md) |
| **Test fails (RED)** | Agent violates rule without skill (baseline) |
| **Test passes (GREEN)** | Agent complies with skill present |
| **Refactor** | Close loopholes while maintaining compliance |
| **Write test first** | Run baseline scenario BEFORE writing skill |
| **Watch it fail** | Document exact rationalizations agent uses |
| **Minimal code** | Write skill addressing those specific violations |
| **Watch it pass** | Verify agent now complies |
| **Refactor cycle** | Find new rationalizations -> plug -> re-verify |

## When to Create a Skill

**Create when:**
- Technique wasn't intuitively obvious
- You'd reference this again across projects
- Pattern applies broadly (not project-specific)
- Others would benefit

**Don't create for:**
- One-off solutions
- Standard practices well-documented elsewhere
- Project-specific conventions (put in CLAUDE.md)
- Mechanical constraints (if enforceable with regex/validation, automate it)

## SKILL.md Structure

**Frontmatter (YAML):** Only `name` and `description` supported. Max 1024 characters total.
- `name`: Letters, numbers, hyphens only
- `description`: Start with "Use when..." — describes ONLY triggering conditions, NEVER workflow

```markdown
---
name: skill-name
description: Use when [specific triggering conditions and symptoms]
---

# Skill Name

## Overview
Core principle in 1-2 sentences.

## When to Use
Bullet list with SYMPTOMS and use cases. When NOT to use.

## Core Pattern
Before/after comparison (for techniques/patterns).

## Quick Reference
Table or bullets for scanning.

## Common Mistakes
What goes wrong + fixes.
```

**File organization:** SKILL.md required. Supporting files only for heavy reference (100+ lines) or reusable tools.

## Claude Search Optimization (CSO)

**CRITICAL: Description = When to Use, NOT What the Skill Does**

The description must ONLY describe triggering conditions. Never summarize the skill's process or workflow.

**Why:** When a description summarizes workflow, Claude follows the description instead of reading the full skill. The skill body becomes documentation Claude skips.

```yaml
# BAD: Summarizes workflow - Claude may follow this instead of reading skill
description: Use when executing plans - dispatches subagent per task with code review between tasks

# GOOD: Just triggering conditions, no workflow summary
description: Use when executing implementation plans with independent tasks in the current session
```

**Content rules:**
- Use concrete triggers, symptoms, situations
- Describe the *problem* not language-specific symptoms
- Write in third person
- Never summarize the skill's process or workflow

**Keywords:** Use words Claude would search for — error messages, symptoms, synonyms, tool names.

**Naming:** Active voice, verb-first. Use gerunds for processes (`creating-skills`, not `skill-creation`).

**Token efficiency:** Keep skills concise. Reference `--help` instead of documenting all flags. Cross-reference other skills instead of repeating content.

## The Iron Law

```
NO SKILL WITHOUT A FAILING TEST FIRST
```

This applies to NEW skills AND EDITS to existing skills.

Write skill before testing? Delete it. Start over.
Edit skill without testing? Same violation.

**No exceptions:** Not for "simple additions", not for "just adding a section", not for "documentation updates". Don't keep untested changes as "reference". Delete means delete.

## RED-GREEN-REFACTOR for Skills

**RED — Write Failing Test (Baseline):** Run pressure scenario with subagent WITHOUT the skill. Document exact behavior: what choices they made, what rationalizations they used (verbatim), which pressures triggered violations.

**GREEN — Write Minimal Skill:** Write skill addressing those specific rationalizations. Don't add extra content for hypothetical cases. Run same scenarios WITH skill — agent should now comply.

**REFACTOR — Close Loopholes:** Agent found new rationalization? Add explicit counter. Re-test until bulletproof.

## Testing Skill Types

| Skill Type | Test With | Success Criteria |
|-|-|-|
| Discipline (rules) | Pressure scenarios, combined pressures, rationalization traps | Follows rule under maximum pressure |
| Technique (how-to) | Application scenarios, edge cases, missing info tests | Applies technique to new scenario |
| Pattern (mental model) | Recognition, application, counter-examples | Correctly identifies when/how to apply |
| Reference (docs/APIs) | Retrieval, application, gap testing | Finds and correctly applies information |

## Common Rationalizations for Skipping Testing

| Excuse | Reality |
|-|-|
| "Skill is obviously clear" | Clear to you != clear to other agents. Test it. |
| "Testing is overkill" | Untested skills have issues. Always. 15 min saves hours. |
| "I'll test if problems emerge" | Problems = agents can't use skill. Test BEFORE deploying. |
| "Academic review is enough" | Reading != using. Test application scenarios. |

**All of these mean: Test before deploying. No exceptions.**

## Skill Creation Checklist

**IMPORTANT: Use TaskCreate to create todos for EACH checklist item below.**

**RED Phase:**
- [ ] Create pressure scenarios (3+ combined pressures for discipline skills)
- [ ] Run scenarios WITHOUT skill — document baseline behavior verbatim
- [ ] Identify patterns in rationalizations/failures

**GREEN Phase:**
- [ ] Name: letters, numbers, hyphens only
- [ ] YAML frontmatter: only `name` and `description` (max 1024 chars)
- [ ] Description starts with "Use when..." — triggers/symptoms only, third person
- [ ] Keywords throughout for search (errors, symptoms, tools)
- [ ] Clear overview with core principle
- [ ] Address specific baseline failures from RED phase
- [ ] One excellent example (not multi-language)
- [ ] Run scenarios WITH skill — verify compliance

**REFACTOR Phase:**
- [ ] Identify new rationalizations from testing
- [ ] Add explicit counters, build rationalization table
- [ ] Re-test until bulletproof

**Quality Checks:**
- [ ] Quick reference table, common mistakes section
- [ ] No narrative storytelling
- [ ] Supporting files only for tools or heavy reference

**Deployment:**
- [ ] Commit and push
- [ ] STOP — do NOT batch-create skills without testing each one

## The Bottom Line

**Creating skills IS TDD for process documentation.**

Same Iron Law: No skill without failing test first.
Same cycle: RED (baseline) -> GREEN (write skill) -> REFACTOR (close loopholes).
