# Instrumentation Plan

## Overview

This plan defines 10 instrumentation points where every agent action emits structured logs, metrics, and alerts according to the monitoring schemas in `/agent/monitoring/live/`.

## Instrumentation Points

### IP-01: Session Start
**Trigger**: Agent session initialization
**Log event_type**: `session_start`
**Metrics emitted**: session_start counter, task_success_rate (baseline), task_failure_rate (baseline)
**Alerts checked**: none (baseline)
**Fields**: timestamp, session_id, task_id, agent_id, event_type, outcome, latency

### IP-02: Memory Load
**Trigger**: Memory index or session memory is loaded/retrieved
**Log event_type**: `memory_load`
**Metrics emitted**: memory_retrieval_accuracy, memory retrieval hit/miss
**Alerts checked**: memory_retrieval_failure (A-04)
**Fields**: timestamp, session_id, task_id, agent_id, event_type, memory_used, outcome, latency, error

### IP-03: Command Execution
**Trigger**: Before/after any shell/system command is executed
**Log event_type**: `command_executed`
**Metrics emitted**: command_failure_rate, tool_usage_rate, latency
**Alerts checked**: command_failure_spike (A-07)
**Fields**: timestamp, session_id, task_id, agent_id, event_type, command, error, retry_count, outcome, latency, cost

### IP-04: Error Capture
**Trigger**: Any exception, command failure, or API error is caught
**Log event_type**: `error_captured`
**Metrics emitted**: task_failure_rate, command_failure_rate, api errors
**Alerts checked**: command_failure_spike (A-07), task_failure_spike (A-08)
**Fields**: timestamp, session_id, task_id, agent_id, event_type, command, error, outcome, latency

### IP-05: Retry Attempt
**Trigger**: Before each retry of a failed operation
**Log event_type**: `retry_attempt`
**Metrics emitted**: retry_count, retry_budget_usage
**Alerts checked**: retry_budget_exhausted (A-01)
**Fields**: timestamp, session_id, task_id, agent_id, event_type, command, error, retry_count, outcome, latency

### IP-06: Fix Application
**Trigger**: A self-healing fix is applied to a failed command/task
**Log event_type**: `fix_applied`
**Metrics emitted**: fix_reuse_rate
**Alerts checked**: none
**Fields**: timestamp, session_id, task_id, agent_id, event_type, command, error, fix_applied, outcome, latency

### IP-07: Memory Write
**Trigger**: Any memory key/value is written to the memory index
**Log event_type**: `memory_write`
**Metrics emitted**: memory_write_count
**Alerts checked**: memory_write_failure (A-05)
**Fields**: timestamp, session_id, task_id, agent_id, event_type, memory_written, outcome, latency, error

### IP-08: API Call
**Trigger**: Any external API call is made
**Log event_type**: `api_call`
**Metrics emitted**: api_call_count, latency, cost
**Alerts checked**: api_rate_limit_hit (A-02)
**Fields**: timestamp, session_id, task_id, agent_id, event_type, api_call, outcome, latency, cost

### IP-09: Rate Limit Event
**Trigger**: A 429 / rate limit response is received from an API
**Log event_type**: `rate_limit_event`
**Metrics emitted**: rate_limit_hits, 429_count
**Alerts checked**: api_rate_limit_hit (A-02), repeated_429 (A-03)
**Fields**: timestamp, session_id, task_id, agent_id, event_type, api_call, rate_limit, outcome, latency

### IP-10: Task Completion
**Trigger**: A task finishes (success or failure)
**Log event_type**: `task_completed`
**Metrics emitted**: task_success_rate, task_failure_rate, cross_session_recall_rate
**Alerts checked**: cross_session_recall_failure (A-06), task_failure_spike (A-08)
**Fields**: timestamp, session_id, task_id, agent_id, event_type, outcome, latency, cost

## Log Rotation

- Format: JSONL (one JSON object per line)
- Max file size: 10 MB per file
- Retention: 30 days
- No secrets in logs
- Redaction applied to: command, error, stdout, stderr fields for passwords, tokens, keys, emails, SSNs

## Alert Severity Mapping

| Alert ID | Name | Severity | Trigger Condition |
|----------|------|----------|-------------------|
| A-01 | retry_budget_exhausted | HIGH | retry_budget_usage >= 100% |
| A-02 | api_rate_limit_hit | MEDIUM | rate_limit == true |
| A-03 | repeated_429 | HIGH | 429_count > 3 per session |
| A-04 | memory_retrieval_failure | MEDIUM | memory_retrieval_accuracy < 60% |
| A-05 | memory_write_failure | HIGH | memory write returns error |
| A-06 | cross_session_recall_failure | HIGH | cross_session_recall_rate < 60% |
| A-07 | command_failure_spike | HIGH | command_failure_rate > 10 per session |
| A-08 | task_failure_spike | HIGH | task_failure_rate > 10% |

## Output Files

- Logs: `agent/logs/events.jsonl` (JSONL, rotated)
- Metrics: `agent/logs/metrics.jsonl` (JSONL)
- Alerts: `agent/logs/alerts.jsonl` (JSONL)
- Dashboards: rendered on demand from log/metric files
