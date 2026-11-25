# Guardrails — All Capability Pillars
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# Defines guardrails for high-impact, irreversible, sensitive,
# rate-limited, process, package, and system actions.

## Guardrail 1: High-Impact Actions

| Action | Guardrail | Required |
|--------|-----------|----------|
| Full app build | Verify dependencies first; sandbox build | User confirmation |
| System-wide change | Document reason; backup first | Explicit confirmation |
| Mass file operation | Confirm file count > 10 | User confirmation |
| Subagent delegation | Define scope; limit to read-only by default | User approval |

### Enforcement
- Log high-impact actions to `agent/logs/high-impact.log`
- Require `confirm` for actions affecting >10 files
- Subagents default to read-only mode

---

## Guardrail 2: Irreversible Actions

| Action | Guardrail | Required |
|--------|-----------|----------|
| rm -rf / | Blocked entirely | Never allowed |
| dd on system partitions | Blocked entirely | Never allowed |
| mkfs | Blocked entirely | Never allowed |
| fdisk on system disks | Blocked entirely | Never allowed |
| File deletion | Confirm each file | User confirmation |
| Package removal | Log to package-changes.log | User confirmation |

### Enforcement
- `agent/bin/agent` blocks rm -rf /, dd, mkfs, fdisk by default
- Destructive commands require typing `confirm`
- All deletions logged before execution

---

## Guardrail 3: Sensitive Data

| Data Type | Guardrail | Required |
|-----------|-----------|----------|
| Passwords/tokens | Redacted in logs (log-schema.json) | Automatic |
| Emails | Redacted in logs | Automatic |
| SSNs | Redacted in logs | Automatic |
| API keys | Never logged in plaintext | Automatic |
| Personal data | Not exfiltrated off-device | Policy |

### Enforcement
- Redaction rules in `agent/monitoring/live/log-schema.json`
- Patterns: password/passwd/pwd/token/secret/key, email, SSN
- No data leaves the device without user consent

---

## Guardrail 4: API Rate Limits

| Limit | Guardrail | Required |
|-------|-----------|----------|
| Per endpoint | Max 2 attempts | retry-policy.md |
| Per session | Max 10 total attempts | retry-policy.md |
| HTTP 429 | Honor Retry-After header; 2x backoff | retry-policy.md |
| Web search | Cache results; avoid redundant requests | Policy |

### Enforcement
- `agent/bin/agent-run` checks budget before each attempt
- Rate-limit hits logged to `logs/retry-attempts.log`
- Backoff schedule: 2s, 4s, 8s for rate-limit class

---

## Guardrail 5: Process Control

| Action | Guardrail | Required |
|--------|-----------|----------|
| Long-running process | Use shell_background | Notification |
| Cron jobs | Max 10 concurrent | Policy |
| File watchers | Max 5 watched paths | Policy |
| Daemon management | Log start/stop | Automatic |

### Enforcement
- Background processes tracked in `bg_list`
- Cron job count enforced before scheduling
- File watcher count enforced before adding

---

## Guardrail 6: Package Modification

| Action | Guardrail | Required |
|--------|-----------|----------|
| Install package | Log to package-changes.log | Automatic |
| Remove package | User confirmation | Explicit |
| Modify system package | Blocked unless documented | User confirmation |
| Gradle/Android SDK | Workaround documented (FU-1) | Policy |

### Enforcement
- All package installs logged with timestamp and package name
- System package modifications require documented justification
- Gradle builds use chroot workaround

---

## Guardrail 7: System Modification

| Action | Guardrail | Required |
|--------|-----------|----------|
| /system, /vendor, /data/system | Blocked without reason | Documented justification |
| KernelSU settings | Requires grant in Manager app | User action |
| Build.prop changes | Blocked entirely | Never allowed |
| Boot partition | Blocked entirely | Never allowed |

### Enforcement
- `agent/bin/agent` safety policy blocks system partition modification
- Root access verified via `su -c 'id'` (expect uid=0)
- System modifications logged to `agent/logs/system-changes.log`

---

## Guardrail Enforcement Summary

| Guardrail | Enforcement Point | Log File |
|-----------|-------------------|----------|
| High-impact | agent/bin/agent | high-impact.log |
| Irreversible | agent/bin/agent (confirm prompt) | command-executions.log |
| Sensitive data | log-schema.json redaction | All JSONL logs |
| API rate limits | agent/bin/agent-run | retry-attempts.log |
| Process control | shell_background / bg_list | N/A |
| Package modification | pkg_install | package-changes.log |
| System modification | agent/bin/agent safety policy | system-changes.log |

---

## Violation Handling

1. **Warning:** Log violation to `agent/logs/guardrail-violations.log`
2. **Block:** Prevent action from executing
3. **Escalate:** For repeated violations, notify user and suggest policy review
4. **Override:** User may explicitly override with documented reason
