---
version: 1.0.0
last_updated: 2026-02-22
---

# MEMORY.md — Cross-Session Decisions & Preferences

> This file preserves institutional knowledge across sessions.
> Update after every session. Commit with: `chore(memory): [what was learned]`

---

## Naming Conventions

| Item | Format | Example |
|---|---|---|
| Session branch | `claude-session-YYYYMMDD-[topic]` | `claude-session-20260222-auth-refactor` |
| Config branch | `claude-config-YYYYMMDD-[change]` | `claude-config-20260222-rules-v2` |
| Hotfix branch | `claude-fix-YYYYMMDD-[bug-id]` | `claude-fix-20260223-rate-limiter` |
| Session log | `YYYY-MM-DD_[topic-slug].md` | `2026-02-22_initial-setup.md` |
| Git tag (config) | `config/vX.Y.Z` | `config/v1.2.0` |
| Git tag (code) | `vX.Y.Z` | `v2.1.0` |
| Commit format | Conventional Commits | `feat(auth): add JWT refresh` |

---

## Technology Stack

> Fill in as project evolves

- **Language**: [TBD]
- **Framework**: [TBD]
- **Database**: [TBD]
- **Infrastructure**: [TBD]
- **Testing framework**: [TBD]
- **CI/CD**: [TBD]
- **Hosting**: [TBD]

---

## User Preferences

> Record preferences discovered during sessions

- **Response length**: [TBD — verbose / concise / adaptive]
- **Code style**: [TBD — language + linter rules]
- **Output format**: [TBD — prefers tables / prose / lists]
- **Time zone**: [TBD]
- **Budget sensitivity**: [TBD]
- **Risk tolerance**: [TBD — conservative / moderate / aggressive]

---

## Key Decisions Made

| Date | Decision | Rationale | Alternatives Rejected |
|---|---|---|---|
| 2026-02-22 | Created Claude Expert System | Need persistent version-controlled AI workspace | Manual notes (too fragile); cloud-only (vendor lock-in) |
| 2026-02-22 | Modular `.claude/rules/` loading | Token efficiency; load only what task needs | Monolithic CLAUDE.md (bloats context) |
| 2026-02-22 | Git-based session continuity | Universal, offline, free, version-controlled | External DB (adds dependency); proprietary session API (lock-in) |

---

## Architectural Decision Records (ADR)

| # | Decision | Status | Date |
|---|---|---|---|
| ADR-001 | Modular `.claude/rules/` structure for context loading | Accepted | 2026-02-22 |
| ADR-002 | Git session branches for continuity and recovery | Accepted | 2026-02-22 |
| ADR-003 | Three-checkpoint SESSION.md protocol | Accepted | 2026-02-22 |
| ADR-004 | Conventional Commits format for all git messages | Accepted | 2026-02-22 |

---

## Known Project-Specific Gotchas

> Add quirks, failure patterns, and surprises discovered in sessions

- [ ] None recorded yet — update after first real session

---

## Experiments & Results

| Date | Experiment | Result | Incorporated? |
|---|---|---|---|
| — | — | — | — |

---

## Stakeholder Context

> Fill in as known

- **Primary user**: [Name / role]
- **Technical level**: [Beginner / Intermediate / Expert]
- **Primary use case**: [Description]
- **Domain**: [Industry / field]
- **Key constraints**: [Time / budget / compliance]
