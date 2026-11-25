# Improvement Loop
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# 10-step continuous improvement cycle.
# Runs after every session to refine memory, retry policy, and fixes.

## The 10-Step Cycle

### Step 1: Collect Session Metrics
Read all metrics from `agent/logs/metrics.jsonl` for the completed session.
Compute success rate, failure rate, fix rate, memory hit rate.

### Step 2: Identify New Failures
Scan `agent/logs/failures-exhausted.jsonl` for commands that exhausted retry.
These are candidate new failure patterns.

### Step 3: Classify New Failures
Apply the 12-type taxonomy to each new failure.
If it matches an existing pattern (FP-001 to FP-006), link it.
If not, create a new pattern entry.

### Step 4: Propose New Fixes
For each unclassified failure, hypothesize ≥2 root causes.
Propose a least-invasive fix following the FIX application order.

### Step 5: Validate Fixes
Test each proposed fix in a safe context.
Mark fixes as `verified` only after a successful retry.

### Step 6: Update Memory
Add verified fixes to `command-repair-playbook.md`.
Add new patterns to `failure-patterns.md`.
Update `memory-index.json` with new entries.

### Step 7: Tune Retry Policy
If a failure class retries too often, consider reclassifying.
Adjust backoff if rate-limit hits are frequent.
Update `retry-policy.md` if budget thresholds need changing.

### Step 8: Filter Stale Records
Remove records older than 30 days without recall.
Remove `superseded: true` records.
Compact `memory-index.json`.

### Step 9: Report Improvement
Generate an improvement report:
- New patterns discovered: N
- New fixes verified: N
- Retry policy changes: N
- Memory entries filtered: N

Save to `agent/workspace/improvement-report-<session>.md`.

### Step 10: Seed Next Session
Ensure all updated memory files are in place for autostart.
The next session loads the improved memory automatically.

## Improvement Metrics

Tracked in `agent/logs/improvements.jsonl`:
```json
{"timestamp":"ISO","session_id":"<id>","new_patterns":N,"new_fixes":N,"policy_changes":N,"filtered_records":N}
```

## Loop Frequency

- After every session (automatic)
- On demand: run the 10 steps manually
- Weekly: full memory compaction and policy review
