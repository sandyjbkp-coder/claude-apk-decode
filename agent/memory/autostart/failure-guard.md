# Failure Guard — Autostart Memory
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# Post-failure capture, diagnosis, fix, and save rules. After any
# command failure, the agent executes this guard before retrying.

## Post-Failure Steps (Mandatory, in Order)

### Step 1: Capture Exact Command + Error
Record:
- Full command string
- Exit code
- Complete stderr and stdout
- Timestamp and session ID

Append to `agent/logs/failure-captures.jsonl`:
```json
{"timestamp":"ISO","session_id":"<id>","command":"<cmd>","exit_code":N,"stdout":"...","stderr":"..."}
```

### Step 2: Classify Failure (12 Types)
Match error output against the taxonomy in `failure-patterns.md`:
- TRANSIENT, PERMANENT, CONFIGURATION, MISSING_DEPENDENCY,
- WRONG_PATH, WRONG_SYNTAX, PERMISSION, RATE_LIMIT,
- NETWORK, TIMEOUT, VALIDATION, UNKNOWN

Log classification to `agent/logs/failure-classifications.jsonl`.

### Step 3: Search Memory for Similar Failure
Query `failure-patterns.md` and `command-repair-playbook.md`:
- Does this failure match a known pattern (FP-001 to FP-006)?
- Is there a verified fix (FIX-001 to FIX-006)?
- Record match result in memory.

### Step 4: Identify ≥2 Root-Cause Hypotheses
For every failure, form at least two hypotheses:
1. Primary hypothesis (most likely from classification)
2. Secondary hypothesis (alternative explanation)

Example for PERMISSION:
- H1: KernelSU not granted for calling app
- H2: su binary missing or not executable

Log hypotheses to `agent/logs/failure-hypotheses.jsonl`.

### Step 5: Apply Least-Invasive Fix First
Apply fixes in order (FIX-001 → FIX-006 per playbook order):
- Syntax/path → permission → connectivity → rate-limit → timeout
- Log each fix attempt to `agent/logs/fix-attempts.jsonl`

### Step 6: Retry with Corrected Command
Only retry after a fix has been applied:
- Use corrected command (not the original failing one)
- Respect backoff schedule from retry-policy.md
- Log retry to `agent/logs/retry-attempts.jsonl`

### Step 7: Save Fix on Success
If the retry succeeds:
- Record the fix in `command-repair-playbook.md` (or reference existing FIX)
- Tag with `status: verified`, `session_id`, `timestamp`
- Update `memory-index.json`
- Log to `agent/logs/successful-fixes.jsonl`

### Step 8: Exhaustion Handling
If all 3 attempts fail:
- Do NOT retry further
- Escalate: log to `agent/logs/failures-exhausted.jsonl`
- Report to user with full capture + hypotheses
- Mark pattern as `needs_manual_intervention: true`

## Guard Logging

All failure-guard events go to `agent/logs/failure-guard.log` in JSONL format.
