---
description: Run executor agent for feature implementation
argument-hint: "<feature>"
---
# /executor — Run Executor Agent

Dispatch the `@executor` agent to implement a feature from the plan.

Usage: `/executor` — reads `out/plan.md` and `out/tasks.md` for context

The executor:
1. Reads the architecture plan and task list
2. Implements each task with TDD
3. Updates task status in `out/tasks.md`
4. Commits after meaningful changes
