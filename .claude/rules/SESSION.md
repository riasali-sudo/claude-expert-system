---
version: 1.0.0
last_updated: 2026-02-22
session_status: READY_FOR_FIRST_SESSION
---

# SESSION.md — Active Session State

> Single source of truth for current session state.
> Update at the THREE mandatory checkpoints. Do not skip any.
> Git tracks what happened. This file tracks where you are RIGHT NOW.

---

## Current Session

**Date**: [Fill on session start]  
**Branch**: [Fill on session start — run `git branch --show-current`]  
**Model**: [Fill — e.g., claude-sonnet-3.7]  
**Goal**: [One sentence — what this session accomplishes]  

---

## Checkpoint 1: Plan Approved

**[CONTEXT]**: [Problem being solved — 1-2 sentences]  
**[APPROACH]**: [Chosen method + core rationale]  
**[REJECTED_APPROACHES]**:
- Option A: [Description] — rejected because [reason]
- Option B: [Description] — rejected because [reason]

**[GIT_COMMIT]**: `git commit -m "session(plan): [topic] approach approved"`  

---

## Checkpoint 2: In Progress

**[CURRENT_PHASE]**: Phase [ ] of [ ]  

**[DECIDED]**:
- [Decision 1 + rationale]
- [Decision 2 + rationale]

**[EXCLUDED]**:
- [What was tried but rejected + why]

**[FILES_MODIFIED]**:
- [ ] [filename]

**[BLOCKERS]**: [None / description of current blockers]  
**[NEXT_ACTION]**: [Exact next step — include file:line if applicable]  

**[GIT_COMMIT]**: `git commit -m "session(checkpoint): Phase N — [summary]"`  

---

## Checkpoint 3: Session End

**[FINAL_STATE]**: [Exactly where things stand]  
**[OUTSTANDING]**:
- [ ] [Incomplete item 1]
- [ ] [Incomplete item 2]

**[NEXT_ACTION]**: [Precise starting point for next session — be specific enough to resume cold]  
**[RESUME_COMMAND]**: `/continue`  
**[GIT_COMMIT]**: Run `bash .claude/hooks/session-end.sh` — auto-commits all  

---

## Model Escalations This Session

| Time | From | To | Reason | Resolved? |
|---|---|---|---|---|
| — | — | — | — | — |

---

## Compact Events

> Automatically appended by pre-compact.sh hook

---

## Session Notes

[Freeform notes — anything that doesn't fit structured fields]
