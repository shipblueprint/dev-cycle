# Dev Cycle Pipeline

**CRITICAL RULE:** After every stage, the orchestrator MUST save the output to `out/` before proceeding. Do not wait for the user to remind you.

## Stages (run in order)

### S1: PROPOSE

1. Understand user intent. Ask clarifying questions if the request is vague.
2. Check if `openspec` CLI is available. If yes: `openspec propose "<description>"`.
3. If no OpenSpec: create a structured proposal in Markdown.
4. Include: (a) What will be built, (b) Key design decisions, (c) Scope boundaries, (d) Files likely affected.
5. **PERSIST:** `out/proposal.md`

### S2: APPROVE

1. Present the full proposal to the user.
2. Flag any concerns: ambiguous scope, risky changes, dependencies.
3. Ask: *"Shall I proceed with this plan?"*
4. Wait for explicit approval before continuing.

### S3: PLAN

1. Read `out/proposal.md`.
2. Read `out/PROJECT_MAP.md` if it exists for system context.
3. If project has existing architecture docs, read those too.
4. Design: tech stack, module layout, data flow, API surface.
5. Break into actionable tasks — each task should be completable in 1-5 tool calls.
6. **PERSIST:**
   - `out/plan.md` — architecture design + approach
   - `out/tasks.md` — task list with `[ ]` markers
   - `out/PROJECT_MAP.md` — system map (create/update)

### S4: BUILD

**Route based on scope:**
- **Multi-file / new feature** → dispatch `@executor` with `out/plan.md` + `out/tasks.md`
- **Single fix / isolated refactor** → dispatch `@surgeon` with change description + file list

**After subagent completes:**
- Verify all edited files exist and have content.
- Run `lsp_diagnostics` on changed files.
- Check `out/tasks.md` — are all tackled tasks marked `[x]`?
- If tests exist, run them to verify no regression.

### S5: VERIFY

1. Run test suite (if project has one): `npm test`, `pytest`, `cargo test`, `go test`, etc.
2. Verify all tasks in `out/tasks.md` are `[x]`.
3. Do a quick spec compliance check — does the implementation match `out/proposal.md`?
4. **PERSIST:** `out/verification.md` with results.

### S6: ARCHIVE

1. Move all files from `out/` to `archive/<feature-name>-<date>/`.
2. Clear `out/` directory.
3. If OpenSpec CLI is available: `openspec archive`.
4. Present summary to user: what was built, what files changed, test results.

---

## PERSIST Summary

| Stage | File | What |
|-------|------|------|
| S1 | `out/proposal.md` | Feature proposal with scope + approach |
| S3 | `out/plan.md` | Architecture design + approach |
| S3 | `out/tasks.md` | Task list with progress markers |
| S3 | `out/PROJECT_MAP.md` | System architecture map |
| S5 | `out/verification.md` | Test results + compliance check |
| S6 | `archive/<feature>/` | Finalized artifacts |
