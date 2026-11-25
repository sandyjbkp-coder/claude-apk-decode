# Production Alerts
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# 8 alerts: 5 HIGH, 2 MEDIUM, 1 LOW.
# Each alert has a condition, severity, and escalation action.

## Alert Definitions

| ID | Alert | Severity | Condition | Action |
|----|-------|----------|-----------|--------|
| A-01 | Root Access Lost | HIGH | `su -c 'id'` not uid=0 | Re-grant KernelSU; halt commands |
| A-02 | Session Budget Exhausted | HIGH | session_attempts ≥ 10 | Stop agent; notify user |
| A-03 | Command Failure Rate High | HIGH | >10 command failures/session | Investigate patterns; escalate |
| A-04 | Task Success Rate <90% | HIGH | task_success_rate < 90% | Review failing tasks; fix root causes |
| A-05 | Cross-Session Recall Failure | HIGH | 0 recalls in session | Verify memory files intact |
| A-06 | API Error Rate High | MEDIUM | >5 API errors/session | Check endpoints; back off |
| A-07 | Memory Hit Rate Low | MEDIUM | memory_hit_rate < 60% | Review failure patterns; reindex |
| A-08 | Log Rotation Overdue | LOW | logs >30 days old | Rotate; compress old logs |

## Alert States

- `active` — condition currently true
- `acknowledged` — user has seen it
- `resolved` — condition no longer true
- `suppressed` — intentionally silenced

## Escalation Path

1. Agent logs alert to `agent/logs/alerts.jsonl`
2. `agent-status` shows active alerts
3. For HIGH severity: user notification required
4. For MEDIUM: report on next status check
5. For LOW: log only, no action needed

## Example Alert Log

```json
{"timestamp":"ISO","alert_id":"A-01","severity":"HIGH","condition":"root_access_lost","action":"re-grant_kernelSU","status":"active"}
```

## Alert Checking

Alerts are evaluated:
- After every command execution
- At session start and end
- On demand via `agent-status`
