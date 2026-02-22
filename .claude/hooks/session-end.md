---
hook_type: Stop (SessionEnd)
version: 1.0.0
last_updated: 2026-02-22
---

# session-end.sh — Session End Hook

> **Save as**: `.claude/hooks/session-end.sh`  
> **Make executable**: `chmod +x .claude/hooks/session-end.sh`  
> **Hook type**: `Stop` — Runs on ALL session termination types  
> **Fires on**: Manual close, crash, timeout, API expiry, context limit

---

## Shell Script

```bash
#!/bin/bash
# SessionEnd Hook (Stop hook) — fires on ALL exit types
# Commits all state + writes handoff file for next session

TIMESTAMP=$(date +%Y-%m-%d_%H-%M)
BRANCH=$(git branch --show-current 2>/dev/null || echo "unknown")
LOG_DIR="quality_reports/session_logs"
LOG_FILE="${LOG_DIR}/${TIMESTAMP}_session-end.md"

# Ensure log directory exists
mkdir -p "$LOG_DIR"

# Extract fields from SESSION.md for handoff
CONTEXT=$(grep -A 2 "\[CONTEXT\]" .claude/rules/SESSION.md 2>/dev/null | grep -v "\[CONTEXT\]" | head -1)
NEXT_ACTION=$(grep -A 3 "\[NEXT_ACTION\]" .claude/rules/SESSION.md 2>/dev/null | grep -v "\[NEXT_ACTION\]" | head -1)

# Write handoff log
cat > "$LOG_FILE" << EOF
---
session_end: $(date)
branch: ${BRANCH}
model: ${CLAUDE_MODEL:-unknown}
---

# Session Handoff: ${TIMESTAMP}

[CONTEXT]:
${CONTEXT:-"See .claude/rules/SESSION.md for full context"}

[NEXT_ACTION]:
${NEXT_ACTION:-"UNDEFINED — check SESSION.md and docs/WORKLOG.md"}

[GIT_STATE]:
- Branch: ${BRANCH}
- Last commit: $(git log --oneline -1 2>/dev/null || echo "none")
- Uncommitted files: $(git status --short 2>/dev/null | wc -l | tr -d ' ')

[RESUME]:
  Command: /continue
  Branch: ${BRANCH}
  Log: ${LOG_FILE}
EOF

# Commit all state including the new handoff log
git add -A 2>/dev/null
git commit -m "session(end): ${TIMESTAMP} — ${BRANCH}" 2>/dev/null
git push origin HEAD 2>/dev/null

echo "Session saved: ${LOG_FILE}"
echo "Committed and pushed to: ${BRANCH}"
echo "Resume with: /continue"
```

---

## Registration in `.claude/settings.json`

```json
{
  "hooks": {
    "Stop": [
      {
        "type": "command",
        "command": "bash .claude/hooks/session-end.sh",
        "timeout": 30
      }
    ]
  }
}
```

---

## Exit Scenarios Covered

| Exit Type | Hook Fires? | Result |
|---|---|---|
| Manual `/exit` | ✅ Yes | Committed + handoff written |
| App closed (browser/desktop) | ✅ Yes | Committed + handoff written |
| API session timeout | ✅ Yes | Committed + handoff written |
| Crash / unexpected termination | ✅ Yes (best effort) | Committed if git available |
| Context limit reached | ✅ Yes | Committed + handoff written |
| `/clear` | ✅ Yes | Committed + handoff written |

---

## Notes

- The `Stop` hook type fires on all the above exit scenarios
- A 30-second timeout is set — gives enough time for commit + push
- If push fails (offline), commit is still local and push on next start
- Handoff log is written even if SESSION.md fields are empty
