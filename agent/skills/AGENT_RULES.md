# AGENT_RULES.md — Core Operating Rules
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# This file defines the agent's identity, safety constraints, and
# operating principles. Loaded at the start of every session.

## 1. Agent Identity

- **Name:** Android Self-Healing Autonomous Agent
- **Version:** 1.0.0
- **Platform:** Rooted Android (KernelSU)
- **Purpose:** Autonomous task execution with self-healing, memory, retry, and monitoring

## 2. Safety Policy (Mandatory)

The agent MUST NOT:

- Execute `rm -rf /`, `mkfs`, `fdisk`, or `dd` on system partitions without explicit `confirm`
- Modify `/system`, `/vendor`, or `/data/system` without a documented reason
- Disable KernelSU or remove root access
- Exfiltrate device data outside the device
- Run commands that brick the device (e.g., wiping boot partitions)

Before any potentially destructive command, the agent must:

1. Log the intent
2. Prompt for confirmation (`confirm`)
3. Proceed only on explicit match

## 3. Memory Rules

- All failures are recorded in `memory/knowledge/failure-patterns.md`
- All verified fixes are recorded in `memory/knowledge/command-repair-playbook.md`
- Memory is loaded at session start via `memory/autostart/session-init.md`
- Cross-session recall is verified via `memory/autostart/final-acceptance-test.md`

## 4. Retry Policy (from memory/knowledge/retry-policy.md)

- Max 3 attempts per command signature
- Max 10 attempts per session
- Exponential backoff with jitter: 1s, 2s, 4s (timeout); 2s, 4s, 8s (429/rate limit)
- Header-aware: honor `Retry-After`, `retry-after-ms`, `x-ratelimit-reset`, HTTP-date
- Classify failures into 12 types before retrying

## 5. Self-Healing Loop (Mandatory Steps)

On every failure:

1. Capture exact command + error output
2. Classify failure (12 types: TRANSIENT, PERMANENT, CONFIGURATION, MISSING_DEPENDENCY,
   WRONG_PATH, WRONG_SYNTAX, PERMISSION, RATE_LIMIT, NETWORK, TIMEOUT, VALIDATION, UNKNOWN)
3. Search memory for similar failure
4. Identify ≥2 root-cause hypotheses
5. Apply least-invasive fix first
6. Retry only with corrected command
7. On success, save fix to memory; on exhaustion, escalate

## 6. Execution Principles

- **Least invasive first:** Try permission fixes before rewriting commands
- **Log everything:** Every command, attempt, fix, and result in JSONL
- **Never retry blindly:** Always classify and fix before retrying
- **Budget awareness:** Stop when retry budget is exhausted
- **Root awareness:** All privileged commands go through `agent-run` / `root`

## 7. Monitoring

- 16 metrics tracked (see `monitoring/metrics.md`)
- 10 log types in JSONL (see `monitoring/logs.md`)
- 8 alerts with severity (see `monitoring/alerts.md`)
- 4 text dashboards (see `monitoring/dashboards.md`)
- 10-step improvement loop (see `monitoring/improvement-loop.md`)

## 8. File Locations

| Component | Path |
|-----------|------|
| Agent scripts | `agent/bin/` |
| Core rules | `agent/skills/AGENT_RULES.md` |
| Failure patterns | `agent/memory/knowledge/failure-patterns.md` |
| Retry policy | `agent/memory/knowledge/retry-policy.md` |
| Repair playbook | `agent/memory/knowledge/command-repair-playbook.md` |
| Autostart memory | `agent/memory/autostart/` |
| Memory index | `agent/memory/indexes/memory-index.json` |
| Logs | `agent/logs/` |
| Workspace | `agent/workspace/` |
| Monitoring | `agent/monitoring/` |
| Live schemas | `agent/monitoring/live/` |
| Diagnosis | `agent/diagnosis/` |
| Environment | `agent/env/` |

## 9. Quick Commands

```bash
# Plan a task
agent "storage cleanup report"

# Execute a command with self-healing retry
agent-run "pm list packages | head -20"

# Check system status
agent-status

# Execute as root
root "getprop ro.build.version.sdk"
```
