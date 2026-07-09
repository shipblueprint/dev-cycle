---
description: Start architect mode — plan and route work
argument-hint: "<request>"
---
# /architect — Switch to Architect Agent

Switch to the `@architect` agent for dev cycle orchestration.

Usage: `/architect` or `/architect add login feature`

The architect will:
1. Understand your request
2. Route it to the right pipeline stage
3. Dispatch subagents (@executor, @surgeon) as needed
4. Track progress in `out/`
