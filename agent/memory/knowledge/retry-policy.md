# Retry Policy — Knowledge Base
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# Defines retry budgets, backoff schedules, and header-aware delay parsing.
# Loaded at session start for every command execution.

## Retry Budget

| Scope | Max Attempts | Description |
|-------|-------------|-------------|
| Per command signature | 3 | Same command+args, retried up to 3 times |
| Per session | 10 | Total attempts across all commands in one session |
| Per command family | 3 | Commands sharing the same base binary (e.g., all `pm` commands) |
| Per API endpoint | 2 | Same HTTP endpoint, max 2 attempts before backing off longer |

## Backoff Schedule (Exponential with Jitter)

### Timeout-class failures (TIMEOUT, NETWORK)
| Attempt | Delay (seconds) | Jitter |
|---------|----------------|--------|
| 1 → 2 | 1s | +0–1s random |
| 2 → 3 | 2s | +0–1s random |
| 3+ | 4s | +0–1s random (hard cap) |

### Rate-limit-class failures (RATE_LIMIT, 429)
| Attempt | Delay (seconds) | Jitter |
|---------|----------------|--------|
| 1 → 2 | 2s | +0–1s random |
| 2 → 3 | 4s | +0–1s random |
| 3+ | 8s | +0–1s random (hard cap) |

### Permission / Wrong Path / Wrong Syntax
- No automatic retry — these require a fix before retrying
- Budget consumed: 1 attempt only, then apply fix

## Header-Aware Delay Parsing

When an HTTP response includes rate-limit headers, parse them in priority order:

1. `Retry-After` (HTTP-date or delta-seconds)
2. `retry-after-ms` (milliseconds)
3. `x-ratelimit-reset` (Unix timestamp)
4. Fall back to exponential backoff

### Parsing Rules

```
if Retry-After is a numeric value:
    delay = Retry-After seconds
elif Retry-After is an HTTP-date:
    delay = (Retry-After date - now) in seconds
elif retry-after-ms present:
    delay = retry-after-ms / 1000 seconds
elif x-ratelimit-reset present:
    delay = (x-ratelimit-reset - now) in seconds
else:
    delay = exponential_backoff(attempt)
```

## Classification → Action Mapping

| Classification | Retry? | Action |
|---------------|--------|--------|
| TRANSIENT | Yes | Exponential backoff, retry |
| PERMANENT | No | Escalate, do not retry |
| CONFIGURATION | No | Fix config, then retry once |
| MISSING_DEPENDENCY | No | Install dependency, then retry |
| WRONG_PATH | No | Correct path, then retry |
| WRONG_SYNTAX | No | Fix syntax, then retry |
| PERMISSION | No | Grant permission, then retry |
| RATE_LIMIT | Yes | Honor Retry-After header, back off 2x |
| NETWORK | Yes | Exponential backoff, retry |
| TIMEOUT | Yes | Exponential backoff, retry |
| VALIDATION | No | Fix argument, then retry |
| UNKNOWN | Yes (1) | One retry with backoff, then diagnose |

## Session Budget Tracking

Tracked in `agent/logs/retry-attempts.jsonl`:
- `session_attempts` — running total (hard stop at 10)
- `command_family_attempts` — per base-binary count (hard stop at 3)
- `endpoint_attempts` — per API endpoint count (hard stop at 2)

When any budget is hit, the agent stops and reports the exhausted scope.
