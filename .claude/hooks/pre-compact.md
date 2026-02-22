---
hook_type: PreCompact
version: 1.0.0
last_updated: 2026-02-22
---

# pre-compact.sh — Backup Before Context Compaction

> **Save as**: `.claude/hooks/pre-compact.sh`  
> **Make executable**: `chmod +x .claude/hooks/pre-compact.sh`  
> **Hook type**: `PreCompact` — Fires when Claude is about to compact context  
> **Purpose**: Ensures no uncommitted work is lost when context window is compacted

---

## Shell Script

```bash
#!/bin/bash
# PreCompact Hook — backs up state before context window compaction
# Context compaction discards conversation history — this preserves git state

TIMESTAMP=$(date +%Y-%m-%d_%H-%M)
BRANCH=$(git branch --show-current 2>/dev/null || echo "unknown")

echo "--- PreCompact: Saving state before context compaction ---"

# Commit any uncommitted work
if [[ -n $(git status --porcelain 2>/dev/null) ]]; then
  git add -A 2>/dev/null
  git commit -m "auto(pre-compact): state saved before context compaction at ${TIMESTAMP}" \
    2>/dev/null
  echo "Uncommitted changes committed."
else
  echo "No uncommitted changes — state already clean."
fi

# Append compact event marker to SESSION.md
SESSION_FILE=".claude/rules/SESSION.md"
if [ -f "$SESSION_FILE" ]; then
  echo "" >> "$SESSION_FILE"
  echo "### Compact Event: ${TIMESTAMP}" >> "$SESSION_FILE"
  echo "Context compacted. Branch: ${BRANCH}. Resume with /continue." >> "$SESSION_FILE"
fi

echo "Pre-compact backup complete. Branch: ${BRANCH}"
echo "After compaction: type /continue to reload context"
```

---

## Registration in `.claude/settings.json`

```json
{
  "hooks": {
    "PreCompact": [
      {
        "type": "command",
        "command": "bash .claude/hooks/pre-compact.sh",
        "timeout": 15
      }
    ]
  }
}
```

---

## When Context Compaction Fires

| Trigger | Threshold | Action |
|---|---|---|
| Token count | ~60k tokens (default) | Automatic compaction |
| Manual command | `/compact` | User-triggered compaction |
| Near context limit | 80% of model limit | Automatic compaction |
| Enterprise limit | ~400k tokens | Automatic compaction |

---

## After Compaction — Recovery Steps

1. Type `/continue` — loads SESSION.md + git log + last NEXT_ACTION
2. If `/continue` fails: `cat .claude/rules/SESSION.md` manually
3. Check compact event timestamp in SESSION.md for last known state
4. Use `git log --oneline -10` to see what was committed before compact

---

## Notes

- Context compaction discards conversation history but NOT file changes
- This hook ensures git state is committed before history is lost
- The compact event marker in SESSION.md creates a named recovery point
- Works with both automatic and manual compaction
