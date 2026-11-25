# Real-World Tasks — 20 Tasks Across All 8 Pillars
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# Each task maps to a pillar, includes memory integration, automation pattern,
# guardrails, and status. All 20 tasks are executable within the agent framework.

## Task Index by Pillar

| # | Task | Pillar | Status | Memory Integration |
|---|------|--------|--------|-------------------|
| T-01 | Build a CLI tool | P-01 | READY | repair-playbook, failure-patterns |
| T-02 | Fix a bug in existing code | P-01 | READY | failure-guard, retry-policy |
| T-03 | Refactor a module | P-01 | READY | AGENT_RULES.md |
| T-04 | Build a REST API | P-01, P-02 | PARTIAL | retry-policy (Gradle workaround) |
| T-05 | Build a web app | P-01 | READY | repair-playbook, failure-patterns |
| T-06 | Run security audit | P-01 | READY | AGENT_RULES.md safety policy |
| T-07 | Generate documentation | P-01 | READY | memory-index.json |
| T-08 | Monitor system health | P-02 | READY | monitoring logs, alerts |
| T-09 | Install a package | P-02 | READY | package-changes.log |
| T-10 | Clone a Git repo | P-02 | READY | failure-patterns (wrong path) |
| T-11 | Search web for latest docs | P-03 | READY | failure-patterns (network, rate-limit) |
| T-12 | Launch a subagent | P-04 | READY | sessions index |
| T-13 | Schedule a cron job | P-04 | READY | retry-policy |
| T-14 | Generate a dashboard | P-05 | PARTIAL | dashboards.md |
| T-15 | Create a Mermaid diagram | P-05 | READY | N/A (markdown) |
| T-16 | Read Android system properties | P-06 | READY | failure-patterns (permission) |
| T-17 | Inspect telephony state | P-06 | READY | failure-patterns (network) |
| T-18 | Compile C/C++ natively | P-06 | READY | retry-policy (timeout) |
| T-19 | Solve a DSA problem | P-07 | READY | failure-patterns, repair-playbook |
| T-20 | Create a custom skill | P-08 | READY | memory-index.json |

---

## T-01: Build a CLI Tool

**Pillar:** P-01 (Coding & Development)

**Memory Integration:**
- Load `memory/knowledge/command-repair-playbook.md` — FIX-002 (wrong path), FIX-003 (wrong syntax)
- Load `memory/knowledge/failure-patterns.md` — syntax/path failure patterns
- Update `memory/indexes/memory-index.json` — add CLI tool entry

**Automation Pattern:** Code Generation (Pattern 1)

**Steps:**
1. Determine CLI requirements (language, args, output)
2. Scaffold project directory in `agent/workspace/`
3. Write source code using write_file
4. Add shebang and make executable (`chmod +x`)
5. Run syntax check (`python3 -m py_compile` or `node --check`)
6. On syntax error: apply FIX-003, retry
7. Test CLI execution via `agent-run`
8. Save tool info to memory-index.json

**Guardrails:**
- No remote code execution (curl | bash)
- File writes confined to `agent/workspace/`
- Executable permission only on workspace files

**Status:** READY

---

## T-02: Fix a Bug in Existing Code

**Pillar:** P-01 (Coding & Development)

**Memory Integration:**
- Load `memory/autostart/failure-guard.md` — post-failure capture/fix/save
- Load `memory/knowledge/retry-policy.md` — retry budgets
- Log fix to `logs/successful-fixes.jsonl`

**Automation Pattern:** Bug Fixing (Pattern 2)

**Steps:**
1. Capture exact command + error output
2. Classify failure (12 types: TRANSIENT, PERMANENT, etc.)
3. Search `failure-patterns.md` for matching pattern (FP-001 to FP-006)
4. Form ≥2 root-cause hypotheses
5. Apply least-invasive fix (FIX-001 to FIX-006 order)
6. Retry with corrected command
7. On success: save fix to `command-repair-playbook.md`

**Guardrails:**
- Max 3 attempts per command, then escalate
- Do not rewrite entire file; surgical edits only
- Backup before modification

**Status:** READY

---

## T-03: Refactor a Module

**Pillar:** P-01 (Coding & Development)

**Memory Integration:**
- Load `agent/skills/AGENT_RULES.md` — safety policy
- Update `memory/indexes/memory-index.json` — refactor entry

**Automation Pattern:** Refactoring (Pattern 3)

**Steps:**
1. Read current file content
2. Identify code smells (duplication, long functions, etc.)
3. Apply surgical edits via edit_file/multi_edit
4. Run tests to verify behavior unchanged
5. On test failure: revert and diagnose
6. Save refactor notes to memory-index.json

**Guardrails:**
- Always backup before refactoring
- Verify tests pass after refactor
- No behavior changes without user approval

**Status:** READY

---

## T-04: Build a REST API

**Pillar:** P-01 (Coding & Development), P-02 (Terminal & System)

**Memory Integration:**
- Load `memory/knowledge/retry-policy.md` — API endpoint budget (2 attempts)
- Load `memory/knowledge/failure-patterns.md` — FP-004 (network), FP-005 (rate limit)
- Log API errors to `logs/api-calls.log`

**Automation Pattern:** App Building (Pattern 4)

**Steps:**
1. Select stack (Python/Node/Rust)
2. Scaffold API project structure
3. Write route handlers and models
4. Install dependencies (pip/npm)
5. Start server and test endpoints
6. On build failure: apply Pattern 2 (bug fixing)

**Guardrails:**
- Gradle/Android SDK in outer env — workaround documented (FU-1)
- API rate limits: max 2 attempts per endpoint
- Do not expose sensitive data in API responses

**Status:** PARTIAL (Gradle/Android SDK workaround required)

---

## T-05: Build a Web App

**Pillar:** P-01 (Coding & Development)

**Memory Integration:**
- Load `memory/knowledge/command-repair-playbook.md` — FIX-002, FIX-003
- Update `memory/indexes/memory-index.json` — web app entry

**Automation Pattern:** Code Generation (Pattern 1) + App Building (Pattern 4)

**Steps:**
1. Select frontend stack (HTML/JS or framework)
2. Scaffold web app directory
3. Write HTML, CSS, JavaScript
4. Set up local dev server (`python3 -m http.server`)
5. Test in browser via browser_navigate
6. On failure: apply Pattern 2 (bug fixing)

**Guardrails:**
- No external CDN without user approval
- Local hosting only (localhost)

**Status:** READY

---

## T-06: Run Security Audit

**Pillar:** P-01 (Coding & Development)

**Memory Integration:**
- Load `agent/skills/AGENT_RULES.md` — safety policy
- Log audit results to `logs/security-audit.log`

**Automation Pattern:** Code Generation (Pattern 1)

**Steps:**
1. Scan code for security issues (hardcoded secrets, injection, etc.)
2. Check file permissions on sensitive files
3. Verify root access is properly granted
4. Review log redaction rules (log-schema.json)
5. Generate audit report in `agent/workspace/`

**Guardrails:**
- Do not log secrets or tokens
- Security findings are advisory only
- Do not modify security-critical files without confirmation

**Status:** READY

---

## T-07: Generate Documentation

**Pillar:** P-01 (Coding & Development)

**Memory Integration:**
- Update `memory/indexes/memory-index.json` — documentation entry
- Save generated docs to `agent/workspace/`

**Automation Pattern:** Code Generation (Pattern 1)

**Steps:**
1. Scan project for source files
2. Extract function/class signatures
3. Generate README.md with usage examples
4. Generate API docs from code comments
5. Save docs to workspace/

**Guardrails:**
- Documentation is read-only
- Do not include secrets in generated docs

**Status:** READY

---

## T-08: Monitor System Health

**Pillar:** P-02 (Terminal & System Commands)

**Memory Integration:**
- Read `monitoring/logs.md` — 10 log types
- Read `monitoring/alerts.md` — 8 alerts
- Write metrics to `logs/metrics.jsonl`

**Automation Pattern:** System Monitoring (Pattern 5)

**Steps:**
1. Run `agent-status` for system overview
2. Check root availability (`su -c 'id'`)
3. Check memory files present
4. Check logs for errors
5. Check alerts (monitoring/alerts.md)
6. Generate dashboard (monitoring/dashboards.md)
7. Save metrics to metrics.jsonl

**Guardrails:**
- Read-only monitoring; no system modifications
- Log rotation: 30-day retention

**Status:** READY

---

## T-09: Install a Package

**Pillar:** P-02 (Terminal & System Commands)

**Memory Integration:**
- Log to `logs/package-changes.log`
- Check `memory/knowledge/failure-patterns.md` — FP-004 (missing dependency)

**Automation Pattern:** System Monitoring (Pattern 5)

**Steps:**
1. Check if package already installed (`which <pkg>`)
2. Install via `pkg_install` tool
3. Verify installation (`<pkg> --version`)
4. Log installation to package-changes.log
5. On failure: check FP-004 (missing dependency), apply fix

**Guardrails:**
- All package installs logged with timestamp
- System package modifications require documented justification
- No `curl | bash` installations

**Status:** READY

---

## T-10: Clone a Git Repo

**Pillar:** P-02 (Terminal & System Commands)

**Memory Integration:**
- Load `memory/knowledge/failure-patterns.md` — FP-002 (wrong path)
- Log clone to `logs/git-operations.log`

**Automation Pattern:** System Monitoring (Pattern 5)

**Steps:**
1. Verify git is available (`git --version`)
2. Clone repo to `agent/workspace/`
3. Verify clone succeeded
4. On failure: check FP-002 (wrong path/network), apply fix

**Guardrails:**
- Clone only to workspace directory
- No cloning to system directories
- Verify repo URL before cloning

**Status:** READY

---

## T-11: Search Web for Latest Docs

**Pillar:** P-03 (Web Search & Live Research)

**Memory Integration:**
- Load `memory/knowledge/failure-patterns.md` — FP-004 (network), FP-005 (rate limit)
- Cache results to avoid redundant requests

**Automation Pattern:** Web Research (Pattern 6)

**Steps:**
1. Search web_search for query
2. Fetch relevant URLs with web_fetch
3. Verify API/framework docs
4. Extract key information
5. Cache results
6. Save research to memory/sessions/

**Guardrails:**
- Max 2 attempts per API endpoint
- Respect rate limits (Retry-After header)
- Do not scrape sites that block bots

**Status:** READY

---

## T-12: Launch a Subagent

**Pillar:** P-04 (AI Agents & Automation)

**Memory Integration:**
- Write to `memory/sessions/` — subagent session data
- Log to `logs/subagent-log.jsonl`

**Automation Pattern:** Subagent Delegation (Pattern 7)

**Steps:**
1. Define subagent task and scope
2. Invoke task tool with detailed prompt
3. Monitor subagent progress
4. Collect subagent output
5. Integrate results into main task

**Guardrails:**
- Subagents cannot modify system files without root confirmation
- Subagents default to read-only mode

**Status:** READY

---

## T-13: Schedule a Cron Job

**Pillar:** P-04 (AI Agents & Automation)

**Memory Integration:**
- Log cron jobs to `logs/cron-jobs.log`
- Check retry-policy for rate limits

**Automation Pattern:** Scheduled Tasks (Pattern 8)

**Steps:**
1. Define schedule (cron expression)
2. Create background process via shell_background
3. Execute task on schedule
4. Log execution to logs/
5. On failure: apply Pattern 2 (bug fixing)

**Guardrails:**
- Max 10 concurrent cron jobs
- Cron jobs logged before scheduling

**Status:** READY

---

## T-14: Generate a Dashboard

**Pillar:** P-05 (Visual & Interactive UI)

**Memory Integration:**
- Read `monitoring/dashboards.md` — 4 dashboard templates
- Read metrics from `logs/metrics.jsonl`

**Automation Pattern:** UI Generation (Pattern 9)

**Steps:**
1. Determine dashboard type (task, API, memory, toolchain)
2. Fetch data from metrics/logs
3. Generate dashboard text
4. Save to workspace/
5. Offer preview

**Guardrails:**
- Dashboard generation is read-only
- No sensitive data in dashboards

**Status:** PARTIAL (interactive UI requires browser; text dashboards ready)

---

## T-15: Create a Mermaid Diagram

**Pillar:** P-05 (Visual & Interactive UI)

**Memory Integration:**
- Save diagram to `agent/workspace/`

**Automation Pattern:** UI Generation (Pattern 9)

**Steps:**
1. Determine diagram type (flowchart, sequence, architecture)
2. Write Mermaid syntax
3. Save to `.mmd` file or markdown
4. Render via markdown viewer

**Guardrails:**
- Diagrams are read-only representations
- No sensitive system info in diagrams

**Status:** READY

---

## T-16: Read Android System Properties

**Pillar:** P-06 (Android-Specific Powers)

**Memory Integration:**
- Load `memory/knowledge/failure-patterns.md` — FP-001 (permission/root)
- Save results to `agent/diagnosis/`

**Automation Pattern:** Android Inspection (Pattern 10)

**Steps:**
1. Check root status (`su -c 'id'`)
2. Read system properties (`getprop ro.build.version.*`)
3. Read hardware properties (`getprop ro.hardware.*`)
4. Save results to diagnosis/

**Guardrails:**
- Root requires KernelSU grant
- Read-only property access

**Status:** READY

---

## T-17: Inspect Telephony State

**Pillar:** P-06 (Android-Specific Powers)

**Memory Integration:**
- Load `memory/knowledge/failure-patterns.md` — FP-004 (network)
- Save results to `agent/diagnosis/`

**Automation Pattern:** Android Inspection (Pattern 10)

**Steps:**
1. Check connectivity (`ping -c 1 8.8.8.8`)
2. Inspect telephony state via service calls
3. Check Wi-Fi/cellular status
4. Save results to diagnosis/

**Guardrails:**
- Telephony inspection is read-only
- No network modifications without confirmation

**Status:** READY

---

## T-18: Compile C/C++ Natively

**Pillar:** P-06 (Android-Specific Powers)

**Memory Integration:**
- Load `memory/knowledge/retry-policy.md` — timeout backoff
- Log compilation to `logs/builds.log`

**Automation Pattern:** Android Inspection (Pattern 10)

**Steps:**
1. Verify clang/gcc available
2. Write C/C++ source to workspace/
3. Compile for aarch64 (`clang -target aarch64-linux-android`)
4. Verify binary executes
5. On timeout: apply backoff, retry

**Guardrails:**
- Native compilation confined to workspace/
- No system binary replacement without confirmation

**Status:** READY

---

## T-19: Solve a DSA Problem

**Pillar:** P-07 (Knowledge & Problem Solving)

**Memory Integration:**
- Load `memory/knowledge/failure-patterns.md` — problem taxonomy
- Save solution to `logs/knowledge-tasks.log`

**Automation Pattern:** Knowledge Tasks (Pattern 11)

**Steps:**
1. Parse problem statement
2. Search web for references if needed (Pattern 6)
3. Solve using local computation/reasoning
4. Verify solution
5. Document approach and solution

**Guardrails:**
- Math computation local only
- No external API for sensitive data

**Status:** READY

---

## T-20: Create a Custom Skill

**Pillar:** P-08 (Customization & Extensions)

**Memory Integration:**
- Create skill via `skill_manage`
- Index skill in `memory/indexes/memory-index.json`
- Load skill on future sessions via autostart

**Automation Pattern:** Custom Extensions (Pattern 12)

**Steps:**
1. Define skill requirements
2. Create skill via skill_manage
3. Test skill execution
4. Index skill in memory-index.json
5. Load skill on future sessions

**Guardrails:**
- Skill creation requires user approval
- Skill content must follow AGENT_RULES.md

**Status:** READY

---

## Task Readiness Summary

| Status | Count | Tasks |
|--------|-------|-------|
| READY | 18 | T-01, T-02, T-03, T-05, T-06, T-07, T-08, T-09, T-10, T-11, T-12, T-13, T-15, T-16, T-17, T-18, T-19, T-20 |
| PARTIAL | 2 | T-04 (Gradle workaround), T-14 (interactive UI) |
| BLOCKED | 0 | — |
