---
hook_type: SessionStart
version: 1.0.0
last_updated: 2026-02-22
---

# session-start.sh — Session Start Hook

> **Save as**: `.claude/hooks/session-start.sh`  
> **Make executable**: `chmod +x .claude/hooks/session-start.sh`  
> **Hook type**: `SessionStart` — Informational only (cannot block Claude)
> **Fires on**: Every session start, resume, and `/clear`

---

## Shell Script

```bash
#!/bin/bash
# SessionStart Hook — fires on every session start, resume, or /clear
# Loads git context and last session handoff automatically
# Type: Informational (cannot block)

echo "========================================"
echo "  CLAUDE EXPERT SYSTEM — SESSION START"
echo "  $(date)"
echo "========================================"
echo ""

# Git state
echo "--- Current Branch ---"
git branch --show-current 2>/dev/null || echo "Not a git repo"
echo ""

echo "--- Recent Commits (last 5) ---"
git log --oneline -5 2>/dev/null || echo "No commits yet"
echo ""

echo "--- Uncommitted Changes ---"
git status --short 2>/dev/null || echo "No changes"
echo ""

# Load only NEXT_ACTION from most recent session log (token efficient)
echo "--- Last Session Handoff ---"
LATEST_LOG=$(ls -t quality_reports/session_logs/*.md 2>/dev/null | head -1)
if [ -f "$LATEST_LOG" ]; then
  echo "Log: $LATEST_LOG"
  echo ""
  grep -A 5 "\[NEXT_ACTION\]" "$LATEST_LOG" 2>/dev/null | head -6
else
  echo "No previous session log found. Starting fresh."
fi

echo ""
echo "--- Load .claude/rules/SESSION.md for full context ---"
echo "========================================"
```

---

## Registration in `.claude/settings.json`

```json
{
  "hooks": {
    "SessionStart": [
      {
        "type": "command",
        "command": "bash .claude/hooks/session-start.sh",
        "timeout": 10
      }
    ]
  }
}
```

---

## What This Hook Does

| Action | Token Cost | Purpose |
|---|---|---|
| Show current branch | 0 | Confirms which session is active |
| Show last 5 commits | ~50 | Context on recent work |
| Show git status | ~30 | Uncommitted work warning |
| Load last `[NEXT_ACTION]` | ~100 | Resume without re-reading full log |

**Total overhead**: ~180 tokens — minimal and targeted.

---

## Notes

- This hook is **informational only** — it cannot block Claude from proceeding
- It loads only the `[NEXT_ACTION]` excerpt, not the full session log (token efficiency)
- Full context is always available via `/continue` which loads SESSION.md
- Works even when `quality_reports/session_logs/` is empty (first session)
