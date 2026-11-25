# Command Guard — Autostart Memory
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# Pre-execution checks. Before any command runs, verify:
# - Retry budget not exhausted
# - Rate limits respected
# - Known failures checked
# - Safety policy satisfied

## Pre-Execution Checks (All Mandatory)

### Check 1: Retry Budget
Read `agent/logs/retry-attempts.jsonl`:
- Session attempts < 10 → proceed
- Session attempts ≥ 10 → BLOCK, report "session budget exhausted"

- Command family attempts < 3 → proceed
- Command family attempts ≥ 3 → BLOCK, report "family budget exhausted"

### Check 2: Known Failure Search
Search `agent/memory/knowledge/failure-patterns.md`:
- If command matches a known failure pattern → apply correction first
- Log match to `agent/logs/known-failure-matches.log`

### Check 3: Safety Policy
Read `agent/skills/AGENT_RULES.md`:
- Is command destructive (rm -rf, dd, mkfs, fdisk)?
- If yes → prompt for `confirm` before proceeding
- Is command modifying /system, /vendor, /data/system?
- If yes → require documented justification

### Check 4: Root Availability
Verify `su` is available:
- `command -v su` → if missing, BLOCK with "root not available"
- `su -c 'id'` → if not uid=0, BLOCK with "root not granted"

### Check 5: Rate-Limit Compliance
If command is an HTTP/API call:
- Check `agent/logs/api-rate-limit.log` for recent 429s
- If 429 within backoff window → delay until Retry-After expires

## Guard Decision

| Check Result | Action |
|-------------|--------|
| All pass | Execute command |
| Budget exhausted | Block, report scope |
| Known failure | Apply correction, then execute |
| Safety violation | Prompt or block |
| Root unavailable | Block, suggest KernelSU grant |

## Logging

All command-guard checks recorded to `agent/logs/command-guard.log`:
```json
{"timestamp":"ISO","command":"<cmd>","budget_ok":true/false,"safety_ok":true/false,"root_ok":true/false,"decision":"execute|block|correct"}
```
