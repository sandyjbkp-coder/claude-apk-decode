# Memory Integration — All Pillars
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# Ensures every pillar loads memory at session start, searches failure
# patterns before commands, uses known fixes, saves new learnings, and
# updates memory-index.json.

## Universal Memory Integration Standard

Every pillar MUST execute these 5 steps in order:

### Step 1: Load Relevant Memory at Session Start
Read `agent/memory/autostart/session-init.md` and load:
- `agent/skills/AGENT_RULES.md` — identity and safety policy
- `agent/memory/knowledge/failure-patterns.md` — 6 patterns (FP-001 to FP-006)
- `agent/memory/knowledge/retry-policy.md` — budgets and backoff
- `agent/memory/knowledge/command-repair-playbook.md` — 6 fixes (FIX-001 to FIX-006)
- Pillar-specific memory files (see below)

### Step 2: Search Failure Patterns Before Commands
Before executing any command, search:
- `agent/memory/knowledge/failure-patterns.md` for matching patterns
- `agent/memory/knowledge/command-repair-playbook.md` for matching fixes
- Log match to `agent/logs/retrieval-guard.log`

### Step 3: Use Known Fixes
If a known fix exists (FIX-001 to FIX-006):
- Apply the least-invasive fix first
- Log fix attempt to `agent/logs/fix-attempts.jsonl`
- Retry with corrected command

### Step 4: Save New Learnings
On successful fix or new pattern:
- Append fix to `command-repair-playbook.md`
- Append pattern to `failure-patterns.md`
- Log to `agent/logs/successful-fixes.jsonl`

### Step 5: Update memory-index.json
Add new entries to `agent/memory/indexes/memory-index.json`:
- New failure patterns under `failure_patterns_index`
- New fixes under `repair_fixes_index`
- New sessions under `sessions`
- Update `last_updated` timestamp

---

## Pillar-Specific Memory Integration

### P-01: Coding & Development
| Memory File | Purpose |
|-------------|---------|
| `memory/knowledge/command-repair-playbook.md` | FIX-002 (wrong path), FIX-003 (wrong syntax) |
| `memory/indexes/memory-index.json` | Code patterns index |
| `logs/successful-fixes.jsonl` | Coding fixes applied |
| `logs/code-changes.log` | File edit history |

### P-02: Terminal & System Commands
| Memory File | Purpose |
|-------------|---------|
| `memory/knowledge/retry-policy.md` | Command retry budgets |
| `memory/knowledge/failure-patterns.md` | FP-001, FP-002, FP-006 |
| `agent/bin/agent-run` | Root command executor with self-healing |
| `logs/command-executions.log` | Command history |

### P-03: Web Search & Live Research
| Memory File | Purpose |
|-------------|---------|
| `memory/knowledge/failure-patterns.md` | FP-004 (network), FP-005 (rate limit) |
| `memory/knowledge/retry-policy.md` | API endpoint budget (2 attempts) |
| `logs/api-calls.log` | Web request history |

### P-04: AI Agents & Automation
| Memory File | Purpose |
|-------------|---------|
| `memory/sessions/` | Subagent session data |
| `memory/indexes/memory-index.json` | Subagent task index |
| `logs/subagent-log.jsonl` | Subagent execution history |

### P-05: Visual & Interactive UI
| Memory File | Purpose |
|-------------|---------|
| `monitoring/dashboards.md` | 4 dashboard templates |
| `logs/ui-generation.log` | UI generation history |

### P-06: Android-Specific Powers
| Memory File | Purpose |
|-------------|---------|
| `memory/knowledge/failure-patterns.md` | FP-001 (permission/root) |
| `agent/bin/root` | Root shell wrapper |
| `agent/diagnosis/` | Android diagnostics |

### P-07: Knowledge & Problem Solving
| Memory File | Purpose |
|-------------|---------|
| `memory/knowledge/failure-patterns.md` | Problem taxonomy |
| `memory/knowledge/command-repair-playbook.md` | Fix patterns |
| `logs/knowledge-tasks.log` | Knowledge task history |

### P-08: Customization & Extensions
| Memory File | Purpose |
|-------------|---------|
| `memory/indexes/memory-index.json` | Skill index |
| `memory/sessions/` | Persistent session data |
| `skills/` | On-demand skill storage |

---

## Memory Update Protocol

When a pillar saves new learnings:

1. Append to the appropriate knowledge file
2. Update `memory-index.json` with new entry
3. Log the update to `agent/logs/memory-updates.jsonl`:
```json
{"timestamp":"ISO","pillar":"P-XX","action":"add_pattern|add_fix|update_index","entry_id":"FP-XXX|FIX-XXX","session_id":"<id>"}
```

4. Verify `memory-index.json` remains valid JSON

---

## Verification Checklist

- [ ] All 8 pillars reference memory files
- [ ] All 6 failure patterns (FP-001 to FP-006) indexed
- [ ] All 6 fixes (FIX-001 to FIX-006) indexed
- [ ] memory-index.json is valid JSON
- [ ] Session init loads all memory files
- [ ] Retrieval guard searches before commands
- [ ] Failure guard saves fixes on success
- [ ] memory-index.json updated after each session
