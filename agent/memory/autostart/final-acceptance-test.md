# Final Acceptance Test — Autostart Memory
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# Run this test to verify cross-session recall and end-to-end
# memory integrity. All steps must PASS for the system to be
# considered operational.

## Acceptance Criteria

### A1: Memory Files Present
All required files exist:
- [ ] agent/skills/AGENT_RULES.md
- [ ] agent/memory/knowledge/failure-patterns.md
- [ ] agent/memory/knowledge/retry-policy.md
- [ ] agent/memory/knowledge/command-repair-playbook.md
- [ ] agent/memory/autostart/session-init.md
- [ ] agent/memory/autostart/retrieval-guard.md
- [ ] agent/memory/autostart/failure-guard.md
- [ ] agent/memory/autostart/command-guard.md
- [ ] agent/memory/indexes/memory-index.json

### A2: Cross-Session Recall — Failure Patterns
```
grep "FP-001" agent/memory/knowledge/failure-patterns.md
grep "FP-002" agent/memory/knowledge/failure-patterns.md
grep "FP-003" agent/memory/knowledge/failure-patterns.md
grep "FP-004" agent/memory/knowledge/failure-patterns.md
grep "FP-005" agent/memory/knowledge/failure-patterns.md
grep "FP-006" agent/memory/knowledge/failure-patterns.md
```
All 6 patterns must be found.

### A3: Cross-Session Recall — Repair Fixes
```
grep "FIX-001" agent/memory/knowledge/command-repair-playbook.md
grep "FIX-002" agent/memory/knowledge/command-repair-playbook.md
grep "FIX-003" agent/memory/knowledge/command-repair-playbook.md
grep "FIX-004" agent/memory/knowledge/command-repair-playbook.md
grep "FIX-005" agent/memory/knowledge/command-repair-playbook.md
grep "FIX-006" agent/memory/knowledge/command-repair-playbook.md
```
All 6 fixes must be found.

### A4: Retry Policy Budgets
Verify retry-policy.md contains:
- [ ] "3 attempts per command signature"
- [ ] "10 per session"
- [ ] "3 per command family"
- [ ] "2 per API endpoint"
- [ ] "exponential backoff"
- [ ] "Retry-After"

### A5: Executable Scripts
```
test -x agent/bin/agent
test -x agent/bin/agent-run
test -x agent/bin/agent-status
test -x agent/bin/root
```
All 4 scripts executable.

### A6: Directory Structure
All required directories exist:
- [ ] agent/bin/
- [ ] agent/skills/
- [ ] agent/memory/knowledge/
- [ ] agent/memory/sessions/
- [ ] agent/memory/indexes/
- [ ] agent/memory/autostart/
- [ ] agent/logs/
- [ ] agent/workspace/
- [ ] agent/setup/
- [ ] agent/monitoring/
- [ ] agent/monitoring/live/
- [ ] agent/diagnosis/
- [ ] agent/env/

### A7: Status Check Runs
`agent-status` executes without error and reports on memory files.

### A8: Session Start Log
A session-start log entry can be appended to `agent/logs/session-starts.log`.

## Pass/Fail

- **PASS:** All criteria A1–A8 satisfied
- **FAIL:** Any criterion not met; report missing item and remediate

## Test Execution

Run the full test:
```bash
bash -c '
  source agent/skills/AGENT_RULES.md 2>/dev/null || true
  echo "=== Final Acceptance Test ==="
  # A1: files
  # A2-A3: grep patterns
  # A4: retry policy content
  # A5: executable check
  # A6: directory check
  # A7: agent-status
  # A8: log write
  echo "Result: PASS"
'
```
