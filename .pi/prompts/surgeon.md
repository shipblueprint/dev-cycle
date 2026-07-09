---
description: Run surgeon agent for precise code fixes
argument-hint: "<fix description>"
---
# /surgeon — Run Surgeon Agent

Dispatch the `@surgeon` agent for a precise, isolated code fix.

Usage: `/surgeon fix the null reference in PaymentService.process()` or `/surgeon`

The surgeon:
1. Performs impact analysis before touching code
2. Makes minimal, precise edits only
3. Uses TDD: failing test → implement → pass
4. Updates task tracking if applicable
