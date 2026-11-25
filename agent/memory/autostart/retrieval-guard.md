# Retrieval Guard — Autostart Memory
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# Pre-command search rules. Before executing any command, the agent
# must search memory for known failures and apply corrections.

## Pre-Command Checklist

### 1. Normalize the Command
Strip whitespace, resolve relative paths to absolute, identify the base binary.

### 2. Search Failure Patterns
Search `agent/memory/knowledge/failure-patterns.md` for:
- The base binary (e.g., `pm`, `am`, `su`, `dumpsys`)
- Similar command structures previously attempted
- Any pattern matching the command class

### 3. Search Repair Playbook
Search `agent/memory/knowledge/command-repair-playbook.md` for:
- FIX entries matching the command's failure class
- Fixes marked `verified: true`
- Least-invasive fix first (per FIX application order)

### 4. Check Retry Budget
Read `agent/logs/retry-attempts.jsonl` (if exists):
- `session_attempts` < 10?
- `command_family_attempts[base_binary]` < 3?
- If any budget is exhausted, do NOT execute; report budget status

### 5. Apply Corrected Command
If memory contains a known fix:
- Substitute the corrected command (e.g., add absolute path)
- Log the substitution in `agent/logs/command-substitutions.log`
- Proceed with corrected command

### 6. Log the Intention
Before execution, append to `agent/logs/command-intentions.log`:
```json
{"timestamp":"ISO","command":"<original>","corrected":"<corrected or same>","memory_match":true/false,"budget_ok":true/false}
```

## Guard Decision Matrix

| Memory Match | Budget OK | Action |
|-------------|-----------|--------|
| Yes | Yes | Use corrected command, execute |
| Yes | No | Block, report budget exhausted |
| No | Yes | Execute original, monitor for failure |
| No | No | Block, report budget exhausted |

## Logging Format

All retrieval-guard checks are recorded in JSONL at `agent/logs/retrieval-guard.log`.
