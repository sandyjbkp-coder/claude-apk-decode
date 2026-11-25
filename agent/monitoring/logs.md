# Production Logs
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# 10 log types in JSONL format, 30-day rotation.
# All logs stored under agent/logs/.

## Log Types

| ID | Log File | Format | Description |
|----|----------|--------|-------------|
| L-01 | agent-session.log | Text | Agent session start/end, task planning |
| L-02 | command-executions.log | JSONL | Every command, exit code, output |
| L-03 | retry-attempts.log | JSONL | Retry attempts, classification, backoff |
| L-04 | applied-fixes.log | JSONL | Fixes applied, result, command |
| L-05 | failure-captures.jsonl | JSONL | Exact command + error on failure |
| L-06 | failure-classifications.jsonl | JSONL | Failure classified into 12 types |
| L-07 | fix-attempts.jsonl | JSONL | Each fix attempt during self-healing |
| L-08 | successful-fixes.jsonl | JSONL | Fixes that resolved the failure |
| L-09 | failures-exhausted.jsonl | JSONL | Commands that exhausted retry budget |
| L-10 | session-starts.log | JSONL | Session start events, memory loaded |

## JSONL Log Schema (Common Fields)

Every JSONL log line includes:
- `timestamp` — ISO 8601 UTC
- `session_id` — current session identifier
- `level` — INFO, WARN, ERROR, FATAL
- `event_type` — one of L-01 to L-10
- `data` — event-specific fields

## Log Rotation

- Retention: 30 days
- Rotation: daily at midnight
- Compression: gzip rotated logs after 7 days
- Max size per log: 100MB

## Example Log Lines

Command execution:
```json
{"timestamp":"2024-01-15T10:30:00Z","session_id":"s123","level":"INFO","event_type":"command_execution","command":"pm list packages","exit_code":0,"duration_ms":245}
```

Failure capture:
```json
{"timestamp":"2024-01-15T10:30:05Z","session_id":"s123","level":"ERROR","event_type":"failure_capture","command":"su -c 'id'","exit_code":1,"stderr":"su: permission denied"}
```

Retry attempt:
```json
{"timestamp":"2024-01-15T10:30:10Z","session_id":"s123","level":"WARN","event_type":"retry_attempt","attempt":2,"command":"su -c 'id'","classification":"PERMISSION","backoff_s":2}
```

## Log Access

View recent logs:
```bash
tail -50 agent/logs/command-executions.log
tail -50 agent/logs/retry-attempts.log
```
