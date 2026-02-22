---
hook_type: PostToolUse
version: 1.0.0
last_updated: 2026-02-22
---

# post-tool-use.sh — Auto-Commit After Every File Change

> **Save as**: `.claude/hooks/post-tool-use.sh`  
> **Make executable**: `chmod +x .claude/hooks/post-tool-use.sh`  
> **Hook type**: `PostToolUse` — Fires after every Claude tool call  
> **Purpose**: Creates granular recovery point for every change Claude makes

---

## Shell Script

```bash
#!/bin/bash
# PostToolUse Hook — auto-commits file modifications after every tool call
# Foundation of walk-back recovery — every change has its own commit

TOOL_NAME="${CLAUDE_TOOL_NAME:-unknown-tool}"
TIMESTAMP=$(date +%H:%M:%S)

# Only act if there are actual uncommitted changes
if [[ -n $(git status --porcelain 2>/dev/null) ]]; then
  git add -A 2>/dev/null
  git commit -m "auto(${TOOL_NAME}): file change at ${TIMESTAMP}" \
    --quiet 2>/dev/null
fi
```

---

## Registration in `.claude/settings.json`

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "type": "command",
        "command": "bash .claude/hooks/post-tool-use.sh",
        "timeout": 10
      }
    ]
  }
}
```

---

## Walk-Back Usage

Because every tool use creates a commit, granular recovery is always available:

```bash
# See all auto-commits
git log --oneline --grep="auto("

# Undo last auto-commit, keep changes staged
git reset --soft HEAD~1

# Undo last 3 auto-commits
git reset --soft HEAD~3

# See what changed in last auto-commit
git diff HEAD~1 HEAD

# See ALL history including auto-commits (even after resets)
git reflog
```

---

## Notes

- Creates one micro-commit per Claude tool call that modifies files
- These micro-commits are the foundation of fine-grained rollback (Option A in `/rollback`)
- `git reflog` preserves these even after `git reset --hard`
- Push frequency handled by `session-end.sh` or manual `/checkpoint`
- Silent failure (`--quiet`) — does not interrupt Claude's output
- If git is not initialized, hook exits silently without error
