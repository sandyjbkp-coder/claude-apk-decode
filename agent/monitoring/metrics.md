# Production Metrics
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# 16 metrics tracked across task, API, memory, and command domains.
# Each metric has a type, target, and alert threshold.

## Task Metrics

| ID | Metric | Type | Target | Alert Threshold |
|----|--------|------|--------|-----------------|
| M-01 | Tasks Completed | Counter | ≥1 per session | 0 tasks in a session |
| M-02 | Tasks Failed | Counter | 0 | >0 failures |
| M-03 | Task Success Rate | Gauge | ≥95% | <90% |
| M-04 | Avg Task Duration | Histogram | <60s | >120s |

## API Metrics

| ID | Metric | Type | Target | Alert Threshold |
|----|--------|------|--------|-----------------|
| M-05 | API Calls Made | Counter | — | — |
| M-06 | API Errors | Counter | 0 | >5 per session |
| M-07 | API Success Rate | Gauge | ≥98% | <95% |
| M-08 | Rate Limit Hits (429) | Counter | 0 | >3 per session |
| M-09 | Avg API Latency | Histogram | <500ms | >2000ms |

## Memory Metrics

| ID | Metric | Type | Target | Alert Threshold |
|----|--------|------|--------|-----------------|
| M-10 | Memory Lookups | Counter | — | — |
| M-11 | Memory Hits | Gauge | ≥80% | <60% |
| M-12 | Cross-Session Recalls | Counter | ≥1 per session | 0 recalls |
| M-13 | Stale Records Filtered | Counter | — | — |

## Command Metrics

| ID | Metric | Type | Target | Alert Threshold |
|----|--------|------|--------|-----------------|
| M-14 | Commands Executed | Counter | — | — |
| M-15 | Command Failures | Counter | 0 | >10 per session |
| M-16 | Self-Healing Fixes Applied | Counter | ≥1 when failures occur | 0 fixes when failures >0 |

## Metric Collection

All metrics are logged in JSONL to `agent/logs/metrics.jsonl`:
```json
{"timestamp":"ISO","metric_id":"M-01","name":"Tasks Completed","value":1,"type":"counter"}
```

Metric collection points (10 instrumentation points):
1. Session start
2. Task planned
3. Command execution start
4. Command execution end (success)
5. Command execution end (failure)
6. Retry attempt
7. Fix applied
8. Memory lookup
9. Memory save
10. Session end

## Targets Summary

- Task success rate: ≥95%
- API success rate: ≥98%
- Memory hit rate: ≥80%
- Avg task duration: <60s
- Avg API latency: <500ms
