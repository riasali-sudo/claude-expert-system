---
version: 1.0.0
last_updated: 2026-02-22
---

# SKILLS.md — Reusable Patterns & Task Templates

> When a new effective pattern is discovered in a session, add it here with a version bump in the header.

---

## 1. Technology Assessment Template

Use for any technology evaluation task:

```markdown
## [Technology Name] Assessment

### Current (Production-Ready)
- **Provider**: 
- **Cost**: $X/month or $Y/seat/year
- **Regional availability**: 🇺🇸 US ✅ / 🇪🇺 EU ✅ / 🌏 APAC ✅ / Restricted: [list]
- **Maturity**: GA since [date], [N] documented case studies
- **Limitations**: [key technical or operational constraints]

### Emerging (Beta / Experimental)
- **Technology**: 
- **Stage**: Beta / Private Preview / RC / Early Access
- **ETA to GA**: [timeline]
- **Access**: [how to join beta / waitlist URL]
- **Risk**: [stability, API changes, vendor lock-in concerns]

### Experimental (Research Stage)
- **Research source**: [institution / lab / team / paper]
- **Published**: [date] | **URL**: [link]
- **Commercialization ETA**: [if known, else N/A]
- **Key barrier**: [what must happen before productization]

### Theoretical / Speculative
- **Concept**: 
- **Blockers**: [technical or economic prerequisites]
- **Speculative timeline**: [SPECULATIVE: X years, if ever]
```

---

## 2. Cost-Benefit Analysis Template

```markdown
## Cost-Benefit: [Solution Name]

### Costs
| Item | One-Time | Monthly | Annual |
|---|---|---|---|
| Licensing | $X | — | — |
| Infrastructure | — | $X | — |
| Integration engineering | $X | — | — |
| Staff training | $X | — | — |
| Support contract | — | $X | — |
| **Total** | $X | $X/mo | $X/yr |

### Benefits
| Benefit | Type | Estimated Value/Year | Confidence |
|---|---|---|---|
| [Eliminated manual process] | Direct | $X | [ESTIMATED] |
| [Improved dev velocity] | Indirect | $X | [ESTIMATED] |
| [Risk reduction] | Strategic | $X | [ESTIMATED] |

### ROI Summary
- Payback period: X months
- 3-year TCO: $X
- Risk-adjusted value: $X (assumes Y% implementation success probability)
- Scenarios: Best $X / Base $X / Worst $X
```

---

## 3. Prioritized Action Plan Template

```markdown
## Action Plan

### Priority 1 — [Action Name] | Efficacy: XX% | Value: High
- **What**: [One sentence]
- **Why**: [Core benefit]
- **Cost**: $X setup + $Y/month recurring
- **Timeline**: X weeks to first value
- **Risk**: [Main failure mode]
- **Rollback**: [How to undo if it fails]
- **Next Steps**:
  1. [Specific step 1]
  2. [Specific step 2]
  3. [Specific step 3]

### Priority 2 — [Action Name] | Efficacy: XX% | Value: Medium
...
```

---

## 4. Git Session Pattern

Standard git workflow for every Claude session:

```bash
# SESSION START
git checkout -b claude-session-$(date +%Y%m%d)-[topic]
git push -u origin HEAD
gh pr create --draft --title "WIP: [topic]" --body "Session started: $(date)"

# DURING SESSION — after each meaningful change (auto via hook)
git add -A && git commit -m "<type>(<scope>): <description>"
# Types: feat | fix | chore | docs | refactor | session | revert

# PHASE COMPLETE
git commit -m "session(checkpoint): Phase N — [summary]"
git push origin HEAD

# SESSION END (auto via hook)
bash .claude/hooks/session-end.sh
```

---

## 5. Model Escalation Decision Tree

```
Task received
    │
    ├─ Simple (lookup, format, rename, file op)? → Haiku
    │
    ├─ Standard (code, analysis, debugging)?     → Sonnet (default)
    │       │
    │       ├─ Stuck attempt 1? → Refine prompt, retry Sonnet
    │       ├─ Stuck attempt 2? → Add extended thinking, retry Sonnet
    │       └─ Stuck attempt 3? → Escalate to Opus
    │
    └─ Complex (architecture, multi-var, cross-domain)?  → Opus directly
```

**Log every escalation in SESSION.md:**
```markdown
[MODEL_ESCALATION]: Sonnet → Opus at [HH:MM] — [reason]
[RESULT]: [What Opus resolved / still stuck]
[PATTERN]: [If useful, add to SKILLS.md]
```

---

## 6. Regional Context Checklist

For every technology recommendation, verify:
- [ ] 🇺🇸 **US**: regulatory compliance (FCC / FTC / CCPA / HIPAA / SOX as applicable)
- [ ] 🇪🇺 **EU**: GDPR data residency, SCCs, DPA requirements
- [ ] 🌏 **APAC**: local alternatives for China (Great Firewall), India data localization
- [ ] 💰 **Pricing**: confirm US pricing vs. global; note currency and regional tiers
- [ ] 🕐 **Support**: timezone coverage, SLA availability, language support
- [ ] 🔒 **Export controls**: ITAR/EAR if hardware/encryption involved

---

## 7. Hallucination Check Pattern

Before finalizing any response with factual claims, verify:
- [ ] All cost figures cited with source + date
- [ ] All timelines marked `[ESTIMATED]` with confidence %
- [ ] All technical specs linked to official docs or flagged
- [ ] Unverified claims marked `[UNVERIFIED]`
- [ ] Code has corresponding tests (TDD: failing test → implementation → passing)
- [ ] Rejected alternatives logged in SESSION.md

---

## 8. Information Gap Protocol

When critical data is missing:

```markdown
## Information Gaps

**Missing**: [Specific data point]
**Why it matters**: [How it changes the recommendation]
**How to obtain**: [Source, method, estimated time]
**Interim assumption**: [ASSUMPTION]: If X, then recommendation becomes Y

**Recommended discovery steps**:
1. [Action]
2. [Validation method]
3. [Decision point once data is available]
```

---

*Add new patterns below this line — bump version in header*
