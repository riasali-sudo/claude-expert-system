---
version: 1.0.0
last_updated: 2026-02-22
project: [PROJECT_NAME]
---

# CLAUDE.md — Master Routing File

> ≤150 lines. This file ROUTES. It does not teach. Load specific files as needed.

## Quick Reference
- **Session state**: `.claude/rules/SESSION.md`
- **Resume last session**: run `/continue`
- **Reasoning style**: `.claude/rules/SOUL.md`
- **Reusable patterns**: `.claude/rules/SKILLS.md`
- **Project decisions**: `.claude/rules/MEMORY.md`
- **System design**: `docs/ARCHITECTURE.md`
- **Decision log**: `docs/WORKLOG.md`
- **Session history**: `quality_reports/session_logs/`

## Model Routing

| Task Type | Model |
|---|---|
| Simple lookups, formatting, file ops | claude-haiku-* |
| Standard development (default 80%) | claude-sonnet-3.7 |
| Complex architecture, stuck after 3 retries | claude-opus-* |

**Escalation rule**: Retry Sonnet once with original prompt → retry with refined prompt → escalate to Opus. Never skip steps. Log all escalations in SESSION.md.

## Git Protocol

- **New session**: `git checkout -b claude-session-YYYYMMDD-[topic]`
- **Every change**: auto-commit via `post-tool-use.sh` hook
- **Phase done**: `git commit -m "session(checkpoint): Phase N — [summary]"`
- **Session end**: `session-end.sh` auto-commits + writes handoff
- **Walk back**: `/rollback` → see `.claude/commands/rollback.md`
- **Pick up**: `/continue` → see `.claude/commands/continue.md`

## Token Rules

- Load `ARCHITECTURE.md` only when task requires it
- Use `/clear` between unrelated tasks
- Compact at 60k tokens (pre-compact hook saves state first)
- CLAUDE.md stays ≤150 lines always
- Load `.claude/rules/*.md` files modularly — not all at once

## Error Patterns (Project-Specific)

> Add errors specific to THIS project here as discovered. Do NOT add generic rules.
- [ ] No patterns recorded yet — update after first session

## Session Continuity — Three Checkpoints Required

1. **After plan approval** → commit + update SESSION.md Checkpoint 1
2. **During implementation** → incremental SESSION.md Checkpoint 2 updates
3. **Session end** → run `session-end.sh` (Checkpoint 3)

## Slash Commands

| Command | Description |
|---|---|
| `/continue` | Resume from last session |
| `/checkpoint` | Manual phase save + git commit |
| `/rollback` | Walk-back options menu |
| `/status` | Current session state dashboard |

## Config Version Control

Rules files versioned via git tags: `config/vX.Y.Z`
Roll back: `git checkout config/vX.Y.Z -- .claude/rules/`
View tags: `git tag -l "config/*"`
