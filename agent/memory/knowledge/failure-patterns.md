# Failure Patterns — Knowledge Base
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# Documents 6 recurring failure patterns with taxonomy across 12
# classification types. Loaded at session start for cross-session recall.

## Failure Pattern Taxonomy (12 Types)

| ID | Type | Description |
|----|------|-------------|
| T-01 | TRANSIENT | Temporary glitch, succeeds on retry |
| T-02 | PERMANENT | Will never succeed without intervention |
| T-03 | CONFIGURATION | Misconfigured config file or setting |
| T-04 | MISSING_DEPENDENCY | Required library/binary/tool absent |
| T-05 | WRONG_PATH | Incorrect file/command path |
| T-06 | WRONG_SYNTAX | Malformed command or invalid option |
| T-07 | PERMISSION | Insufficient privileges (EACCES, EPERM) |
| T-08 | RATE_LIMIT | HTTP 429 / API throttling |
| T-09 | NETWORK | DNS, connection refused, unreachable |
| T-10 | TIMEOUT | Command or API deadline exceeded |
| T-11 | VALIDATION | Bad argument / failed input check |
| T-12 | UNKNOWN | No clear match; requires diagnosis |

---

## Pattern FP-001: Permission Denied on Root Command

- **Type:** PERMISSION (T-07)
- **Trigger:** `su -c <cmd>` returns "Permission denied" or EACCES
- **Root cause:** KernelSU not granted for calling app, or su binary missing
- **Symptom:** `su: permission denied` / `eacces` / `operation not permitted`
- **Fix:** Grant KernelSU authorization in KernelSU Manager app; verify `su` exists
- **Severity:** HIGH
- **First seen:** Session init
- **Recall count:** N/A (template)

---

## Pattern FP-002: Command Not Found / Wrong Path

- **Type:** WRONG_PATH (T-05)
- **Trigger:** `command not found` or `no such file or directory`
- **Root cause:** Binary not in PATH, wrong absolute path, or not installed
- **Symptom:** `sh: pm: not found` / `/system/bin/foo: No such file`
- **Fix:** Use absolute path (`/system/bin/pm`), install missing package, or update PATH
- **Severity:** MEDIUM
- **First seen:** Session init
- **Recall count:** N/A (template)

---

## Pattern FP-003: Syntax Error in Command

- **Type:** WRONG_SYNTAX (T-06)
- **Trigger:** Shell returns syntax error or invalid option
- **Root cause:** Misquoted string, missing flag, unsupported option
- **Symptom:** `syntax error near unexpected token` / `invalid option -- 'x'`
- **Fix:** Re-quote arguments, correct flag spelling, check `--help`
- **Severity:** MEDIUM
- **First seen:** Session init
- **Recall count:** N/A (template)

---

## Pattern FP-004: Network Connectivity Failure

- **Type:** NETWORK (T-09)
- **Trigger:** Connection refused, DNS failure, host unreachable
- **Root cause:** Airplane mode, no Wi-Fi/cellular, firewall, wrong endpoint
- **Symptom:** `Connection refused` / `Network is unreachable` / `Temporary failure in name resolution`
- **Fix:** Toggle connectivity, verify endpoint DNS, check proxy settings
- **Severity:** HIGH
- **First seen:** Session init
- **Recall count:** N/A (template)

---

## Pattern FP-005: Rate Limit (HTTP 429)

- **Type:** RATE_LIMIT (T-08)
- **Trigger:** API returns HTTP 429 Too Many Requests
- **Root cause:** Too many requests in a time window
- **Symptom:** `429` / `Too Many Requests` / `x-ratelimit-limit` headers
- **Fix:** Honor `Retry-After` / `retry-after-ms` header; back off 2x base delay
- **Severity:** MEDIUM
- **First seen:** Session init
- **Recall count:** N/A (template)

---

## Pattern FP-006: Command Timeout

- **Type:** TIMEOUT (T-10)
- **Trigger:** Command exceeds deadline, no output for extended period
- **Root cause:** Hung process, slow device, blocked I/O
- **Symptom:** `timed out` / `deadline exceeded` / empty output with nonzero exit
- **Fix:** Increase timeout, run in background with `&`, check for blocking I/O
- **Severity:** HIGH
- **First seen:** Session init
- **Recall count:** N/A (template)

---

## Cross-Reference Index

| Pattern | Classification | Primary Fix Location |
|---------|---------------|---------------------|
| FP-001 | PERMISSION | command-repair-playbook.md FIX-001 |
| FP-002 | WRONG_PATH | command-repair-playbook.md FIX-002 |
| FP-003 | WRONG_SYNTAX | command-repair-playbook.md FIX-003 |
| FP-004 | NETWORK | command-repair-playbook.md FIX-004 |
| FP-005 | RATE_LIMIT | command-repair-playbook.md FIX-005 |
| FP-006 | TIMEOUT | command-repair-playbook.md FIX-006 |
