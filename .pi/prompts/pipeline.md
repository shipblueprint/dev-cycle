---
description: Run full dev cycle — propose, plan, build, verify, archive
argument-hint: "<feature description>"
---
# /dev-cycle — Run Full Development Cycle

Run the entire Spec → Plan → Execute → Archive pipeline for a feature or fix.

Usage: `/dev-cycle add login with Google OAuth`

Stages:
- `s1` — PROPOSE: Create feature proposal with scope + approach
- `s2` — APPROVE: Present to user, get sign-off
- `s3` — PLAN: Architecture design + task breakdown
- `s4` — BUILD: Dispatch @executor (features) or @surgeon (fixes)
- `s5` — VERIFY: Run tests, check spec compliance
- `s6` — ARCHIVE: Close out, save artifacts

The orchestrator checks `out/` for existing artifacts to skip completed stages.
