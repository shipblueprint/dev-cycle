# Dev Cycle — Spec-Driven Development Pipeline

**Multi-agent development cycle for OpenCode and Pi.** Enforces **Spec → Plan → Execute → Archive** discipline. No blind coding — every change starts with a spec and ends with an archive.

Built for [OpenCode](https://opencode.ai) and [Pi](https://pi.ai) — each stage is a dedicated agent you can call independently or chain into a full pipeline.

---

## Architecture

```
┌───────────────────────────────────────────────────────┐
│                  OpenCode / Pi Runtime                  │
│  (agent dispatcher, model resolver, file persistence)  │
└───────────────────────┬───────────────────────────────┘
                        │
              ┌─────────▼──────────┐
              │   @architect         │
              │   Orchestrator       │
              │   (routing, gating)  │
              └─────────┬──────────┘
                        │
          ┌─────────────┼──────────────┐
          ▼              ▼              ▼
    ┌──────────┐  ┌──────────┐  ┌──────────┐
    │@executor │  │@surgeon  │  │@reviewer │
    │Features  │  │Fixes     │  │QA Gate   │
    └──────────┘  └──────────┘  └──────────┘
```

| Layer | Role |
|-------|------|
| **@architect** (`.opencode/agent/architect.md`) | Default orchestrator — routes requests, enforces pipeline gating |
| **@executor** (`.opencode/agent/executor.md`) | Full feature implementation with TDD |
| **@surgeon** (`.opencode/agent/surgeon.md`) | Precise, isolated code edits |
| **Pipeline** (`.opencode/pipelines/dev-cycle/`) | Stage-by-stage pipeline definition |

---

## Quick Start

### With OpenCode

```bash
cd /path/to/your/project
# Link dev-cycle (or open it alongside)
opencode

# @architect loads automatically as default agent
# Just say what you want:
"add login with Google OAuth"
"fix the pagination bug"
"refactor the billing module"
```

### With Pi

```bash
cd /path/to/your/project
pi

# Agents load as slash commands:
/dev-cycle add login with Google OAuth
/architect plan the auth system
/surgeon fix the null reference in PaymentService
/executor
```

### Direct agent calls

```
@surgeon fix the null reference in PaymentService.process()
@executor implement the user model with email/password auth
```

---

## Pipeline Stages

```
PROPOSE → APPROVE → PLAN → BUILD → VERIFY → ARCHIVE
```

| Stage | What happens | Artifact |
|-------|-------------|----------|
| **PROPOSE** | Understand intent, create proposal | `out/proposal.md` |
| **APPROVE** | Present to user, get sign-off | — |
| **PLAN** | Architecture design, task breakdown | `out/plan.md`, `out/tasks.md` |
| **BUILD** | @executor (features) or @surgeon (fixes) | Code changes |
| **VERIFY** | Run tests, spec compliance | `out/verification.md` |
| **ARCHIVE** | Close out, save artifacts | `archive/<feature>/` |

---

## Agents

| Agent | Purpose | How to call |
|-------|---------|-------------|
| **@architect** | Orchestrator — routes requests, enforces the cycle | Default on enter, or `@architect` |
| **@executor** | Full feature implementation with TDD | `@executor` |
| **@surgeon** | Surgical code modifications | `@surgeon` |
| **@reviewer** | Code review / quality gate (optional) | `@reviewer` |

---

## Model Configuration

Each agent declares its model in YAML frontmatter (e.g., `model: opencode/big-pickle`). The IDE resolves this through its model-resolution pipeline:

- **OpenCode**: Uses `opencode.json` provider config + model-resolution-pipeline
- **Pi**: Uses its own model resolution from settings

To change a model, edit the `model:` field in `.opencode/agent/*.md`.

---

## Resume & Partial Runs

The system detects already-completed stages by checking output files:

| Check | Means |
|-------|-------|
| `out/proposal.md` exists | S1 done |
| `out/plan.md` exists | S3 done |
| `out/tasks.md` with `[x]` | S4 done |
| `out/verification.md` exists | S5 done |
| `archive/<feature>/` exists | S6 done |

---

## Project Structure

```
dev-cycle/
├── opencode.json              # OpenCode config: default_agent: architect
├── AGENTS.md                  # Multi-IDE agent definitions
├── OPENCODE.md                # OpenCode runtime instructions
├── README.md                  # This file
├── .opencode/
│   ├── agent/
│   │   ├── architect.md       # Default orchestrator agent
│   │   ├── executor.md        # Feature implementation agent
│   │   └── surgeon.md         # Surgical fix agent
│   ├── pipelines/
│   │   └── dev-cycle/
│   │       └── OPENCODE.md    # Pipeline stage definitions
│   └── skills/                # Reusable skills (add as needed)
├── .pi/
│   ├── settings.json          # Pi config → shared skills + prompts
│   └── prompts/
│       ├── pipeline.md        # /dev-cycle — full pipeline
│       ├── architect.md       # /architect — switch to architect
│       ├── executor.md        # /executor — run executor
│       └── surgeon.md         # /surgeon — run surgeon
└── out/                       # Stage artifacts (generated)
```

---

## Requirements

- [OpenCode](https://opencode.ai) CLI or [Pi](https://pi.ai) IDE
- Git (for project context)

---

## License

MIT
