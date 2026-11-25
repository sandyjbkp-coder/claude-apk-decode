# Session Init — Autostart Memory
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# 9 mandatory steps executed automatically at the start of every session.
# Loads identity, device, project memory, filters stale records, and
# reports which memories were used.

## Mandatory Load Steps (in order)

### Step 1: Load Identity Memory
Read `agent/skills/AGENT_RULES.md` for agent identity, version, and safety policy.
- Confirm agent version: 1.0.0
- Confirm platform: Rooted Android (KernelSU)

### Step 2: Load Device Memory
Capture device context:
- KernelSU status: `su -c 'id'` (expect uid=0)
- Android version: `getprop ro.build.version.release`
- SDK level: `getprop ro.build.version.sdk`
- Available tools: `which su python3 node rustc java clang`

### Step 3: Load Project Memory
Read the current task context from `agent/workspace/`:
- List recent task files
- Identify the active task (if any)
- Load prior session notes if present

### Step 4: Load Failure Patterns
Read `agent/memory/knowledge/failure-patterns.md`:
- Index all 6 patterns (FP-001 to FP-006)
- Map to 12 classifications (T-01 to T-12)
- Flag any patterns seen in the last session

### Step 5: Load Repair Playbook
Read `agent/memory/knowledge/command-repair-playbook.md`:
- Index all 6 fixes (FIX-001 to FIX-006)
- Map fixes to patterns via cross-reference table
- Mark fixes as "available" for this session

### Step 6: Load Retry Policy
Read `agent/memory/knowledge/retry-policy.md`:
- Confirm budget: 3 per command, 10 per session, 3 per family, 2 per endpoint
- Confirm backoff schedule for timeout and rate-limit classes
- Confirm header-aware parsing rules

### Step 7: Filter Expired / Superseded Records
Review all memory files and:
- Remove records marked `superseded: true`
- Remove records older than 30 days without recent recall
- Keep only verified fixes (`status: verified`)
- Log which records were filtered out

### Step 8: Load Memory Index
Read `agent/memory/indexes/memory-index.json`:
- Build lookup table for all memory files
- Index sessions, patterns, fixes, and alerts
- Prepare for fast lookup during command execution

### Step 9: Report Memory Usage
Output a summary:
```
=== Memory Loaded ===
Identity:      AGENT_RULES.md (loaded)
Failure patterns: 6 (FP-001..FP-006)
Repair fixes:     6 (FIX-001..FIX-006)
Retry policy:     loaded (3/10/3/2 budgets)
Filtered:         0 expired records
Index entries:    N
Session ready:    YES
```

## Session Start Log Entry

Append to `agent/logs/session-starts.log`:
```json
{"timestamp":"ISO","event":"session_start","version":"1.0.0","memories_loaded":[...],"status":"ready"}
```
