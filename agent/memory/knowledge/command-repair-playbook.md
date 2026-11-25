# Command Repair Playbook — Knowledge Base
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# Documents 6 verified fixes with reusable patterns. Each fix maps to a
# failure pattern in failure-patterns.md. Loaded at session start.

---

## FIX-001: Grant KernelSU / Root Permission

- **Addresses:** FP-001 (PERMISSION)
- **Symptom:** `su: permission denied`, `eacces`, `operation not permitted`
- **Diagnosis:** KernelSU not authorized for caller; su binary present but denied
- **Steps:**
  1. Open KernelSU Manager app
  2. Find the calling app (e.g., Termux, AndroidHarness)
  3. Set access to "Grant" or "Allow"
  4. Retry: `su -c 'id'` → should return `uid=0(root)`
- **Verification:** `su -c 'id'` returns uid=0
- **Least-invasive:** Yes — only changes authorization, no system modification

---

## FIX-002: Correct Command Path

- **Addresses:** FP-002 (WRONG_PATH)
- **Symptom:** `command not found`, `no such file or directory`
- **Diagnosis:** Binary not in PATH, or wrong absolute path used
- **Steps:**
  1. Locate binary: `su -c 'which pm'` or `su -c 'find /system -name pm'`
  2. Use absolute path: `/system/bin/pm list packages`
  3. Or export PATH: `export PATH=$PATH:/system/bin`
- **Verification:** Command runs with exit code 0
- **Least-invasive:** Yes — no system changes

---

## FIX-003: Fix Command Syntax

- **Addresses:** FP-003 (WRONG_SYNTAX)
- **Symptom:** `syntax error`, `invalid option`, `unexpected token`
- **Diagnosis:** Misquoted args, wrong flag, unsupported option
- **Steps:**
  1. Check help: `<cmd> --help` or `man <cmd>`
  2. Re-quote arguments with double quotes
  3. Correct flag spelling (e.g., `--head` → `-n` or `| head`)
  4. Escape special characters (`\ `, `'`, `"`)
- **Verification:** Command parses without syntax error
- **Least-invasive:** Yes — only changes command text

---

## FIX-004: Restore Network Connectivity

- **Addresses:** FP-004 (NETWORK)
- **Symptom:** `Connection refused`, `Network unreachable`, DNS failure
- **Diagnosis:** Airplane mode, no Wi-Fi/cellular, wrong endpoint, proxy
- **Steps:**
  1. Check connectivity: `ping -c 1 8.8.8.8`
  2. Toggle airplane mode off
  3. Verify Wi-Fi/cellular is active
  4. Confirm endpoint DNS resolves: `nslookup <host>`
  5. Check proxy: `env | grep -i proxy`
- **Verification:** `ping 8.8.8.8` succeeds
- **Least-invasive:** Yes — no system modification

---

## FIX-005: Honor Rate-Limit Headers and Back Off

- **Addresses:** FP-005 (RATE_LIMIT)
- **Symptom:** HTTP 429, `Too Many Requests`
- **Diagnosis:** Too many requests in time window
- **Steps:**
  1. Read response headers: `Retry-After`, `retry-after-ms`, `x-ratelimit-reset`
  2. Wait the indicated duration before retrying
  3. Apply 2x exponential backoff on subsequent 429s
  4. Reduce request frequency going forward
- **Verification:** Subsequent request returns HTTP 200
- **Least-invasive:** Yes — only timing changes

---

## FIX-006: Increase Timeout / Run in Background

- **Addresses:** FP-006 (TIMEOUT)
- **Symptom:** `timed out`, `deadline exceeded`, empty output
- **Diagnosis:** Hung process, slow device, blocked I/O
- **Steps:**
  1. Increase timeout: `timeout 60 <cmd>` instead of `timeout 10 <cmd>`
  2. Run in background: `<cmd> &` then poll
  3. Check for blocking I/O: `su -c 'dmesg | tail'`
  4. Break large task into smaller chunks
- **Verification:** Command completes within new timeout
- **Least-invasive:** Yes — only timing/execution mode changes

---

## Fix Application Order (Least Invasive First)

When a failure occurs, apply fixes in this order:

1. **Syntax/path fix** (FIX-002, FIX-003) — no system change, just command text
2. **Permission grant** (FIX-001) — authorization only
3. **Connectivity restore** (FIX-004) — network toggle, no system change
4. **Rate-limit backoff** (FIX-005) — timing only
5. **Timeout increase** (FIX-006) — timing only

Never modify system partitions as a first resort.

---

## Cross-Reference Index

| Fix | Pattern | Classification | Knowledge File |
|-----|---------|---------------|----------------|
| FIX-001 | FP-001 | PERMISSION | failure-patterns.md |
| FIX-002 | FP-002 | WRONG_PATH | failure-patterns.md |
| FIX-003 | FP-003 | WRONG_SYNTAX | failure-patterns.md |
| FIX-004 | FP-004 | NETWORK | failure-patterns.md |
| FIX-005 | FP-005 | RATE_LIMIT | failure-patterns.md |
| FIX-006 | FP-006 | TIMEOUT | failure-patterns.md |
