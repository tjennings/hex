# hex

Distilled development discipline for Claude Code: TDD, systematic debugging, verification, and collaboration patterns.

## Install

From the Claude Code marketplace:

```
claude plugin install hex
```

Or install directly from GitHub:

```
claude plugin install github:tjennings/hex
```

## What's included

### Skills

| Skill | Purpose |
|-|-|
| `using-hex` | Bootstraps skill discovery at conversation start |
| `brainstorming` | Structured ideation before creative work |
| `writing-plans` | Turn specs into step-by-step implementation plans |
| `executing-plans` | Execute written plans across sessions |
| `test-driven-development` | Red-green-refactor workflow |
| `systematic-debugging` | Root-cause analysis before fixes |
| `verification-before-completion` | Verify claims before marking done |
| `subagent-driven-development` | Parallel execution of independent tasks |
| `using-git-worktrees` | Isolated feature branches |
| `finishing-a-development-branch` | Merge/PR workflow for completed work |
| `requesting-code-review` | Ask for review on completed work |
| `receiving-code-review` | Process and apply review feedback |
| `writing-skills` | Create and edit hex skills |

### Commands

- `/brainstorm` — Start a brainstorming session
- `/write-plan` — Generate an implementation plan
- `/execute-plan` — Execute an existing plan

### Agents

- `code-reviewer` — Reviews completed work against plans and coding standards

### Hooks

- `SessionStart` — Initializes context at the start of each conversation

## Usage

Once installed, hex skills activate automatically when relevant. For example, starting a debugging task triggers `systematic-debugging`, and implementing a feature triggers `test-driven-development`.

You can also invoke commands directly:

```
/brainstorm a caching layer for the API
/write-plan from docs/spec.md
/execute-plan docs/plans/my-plan.md
```

## License

MIT
