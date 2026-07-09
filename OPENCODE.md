# Dev Cycle Pipeline Project

This project enforces a **Spec → Plan → Execute → Archive** development cycle for any software project.

## How it works

The `@architect` agent is the default orchestrator. When you enter this folder with OpenCode, it loads `@architect` automatically. From there:

1. **Describe what you want** — "add login with Google OAuth", "fix the pagination bug", "refactor the billing module"
2. **@architect routes it** — full cycle for features, direct to @surgeon for fixes
3. **Subagents handle the work** — `@executor` for building, `@surgeon` for precise edits
4. **Every stage is persisted** to `out/` for resumability

## Available agents

| Agent | How to call | When |
|---|---|---|
| **@architect** | Default on enter (or `@architect`) | Orchestration, planning, routing |
| **@executor** | `@executor` | Full feature implementation |
| **@surgeon** | `@surgeon` | Surgical fixes, isolated refactors |
| **@reviewer** | `@reviewer` | Code review / quality gate |

## Pipeline stages

Pipeline definition: `.opencode/pipelines/dev-cycle/OPENCODE.md`

```
PROPOSE → APPROVE → PLAN → BUILD → VERIFY → ARCHIVE
```

Call `@architect` and describe what you want. The orchestrator handles the rest.

## Subagents

Each pipeline stage is a subagent under `.opencode/agent/`. They can be called directly (`@agent-name`) for standalone tasks:

```
@executor implement the user model with email/password auth
@surgeon fix the null reference in PaymentService.process()
```

## Model configuration

Each agent declares its required model in its YAML frontmatter. OpenCode resolves the actual provider/model through its model-resolution pipeline. Update the `model` field in any `.opencode/agent/*.md` to switch models.
