# Automation Playbook — All Pillars
# Android Self-Healing Autonomous Agent (v1.0.0)
#
# Automation patterns for code generation, bug fixing, refactoring,
# app building, system monitoring, web research, subagent delegation,
# scheduled tasks, UI generation, Android inspection, knowledge tasks,
# and custom extensions.

## Universal Automation Pattern

Every automation follows this template:

```
1. Trigger: <event or schedule>
2. Load Memory: session-init.md + pillar-specific memory
3. Search Patterns: failure-patterns.md for known issues
4. Check Budget: retry-policy.md (3/10/3/2)
5. Execute: run the automation steps
6. Self-Heal: on failure, classify → fix → retry
7. Save: log results, update memory-index.json
8. Report: output to user or workspace
```

---

## Automation Pattern 1: Code Generation

**Trigger:** User requests new code module or project

**Steps:**
1. Load P-01 memory (AGENT_RULES.md, repair playbook)
2. Determine language/framework from requirements
3. Scaffold directory structure
4. Write source files using write_file/edit_file
5. Run syntax check (python3 -m py_compile, node --check, etc.)
6. On syntax error: apply FIX-003, retry
7. Save to memory-index.json

**Guardrail:** No remote code execution (curl \| bash)

---

## Automation Pattern 2: Bug Fixing

**Trigger:** Error message or stack trace provided

**Steps:**
1. Capture exact command + error (failure-guard Step 1)
2. Classify failure (12 types) (failure-guard Step 2)
3. Search failure-patterns.md for match (failure-guard Step 3)
4. Form ≥2 root-cause hypotheses (failure-guard Step 4)
5. Apply least-invasive fix (failure-guard Step 5)
6. Retry with corrected command (failure-guard Step 6)
7. Save fix on success (failure-guard Step 7)

**Guardrail:** Max 3 attempts per command, then escalate

---

## Automation Pattern 3: Refactoring

**Trigger:** User requests refactor of existing module

**Steps:**
1. Read current file content
2. Identify code smells (duplication, long functions, etc.)
3. Apply surgical edits via edit_file/multi_edit
4. Run tests to verify behavior unchanged
5. On test failure: revert and diagnose
6. Save refactor notes to memory-index.json

**Guardrail:** Always backup before refactoring; verify tests pass

---

## Automation Pattern 4: App Building

**Trigger:** User requests full app build (CLI, API, web, Android)

**Steps:**
1. Select stack based on requirements (Python/Node/Rust/etc.)
2. Scaffold project structure
3. Write source code (Pattern 1)
4. Install dependencies (pkg_install or pip/npm)
5. Build/compile (gcc/clang/rustc/java/gradle)
6. Run the app to verify
7. On build failure: apply Pattern 2 (bug fixing)

**Guardrail:** Gradle/Android SDK in outer env — workaround documented (FU-1)

---

## Automation Pattern 5: System Monitoring

**Trigger:** Scheduled or on-demand health check

**Steps:**
1. Run agent-status for system overview
2. Check root availability (su -c 'id')
3. Check memory files present
4. Check logs for errors (tail command-executions.log, retry-attempts.log)
5. Check alerts (monitoring/alerts.md)
6. Generate dashboard (monitoring/dashboards.md)
7. Save metrics to metrics.jsonl

**Guardrail:** Read-only monitoring; no system modifications

---

## Automation Pattern 6: Web Research

**Trigger:** User needs latest docs or web info

**Steps:**
1. Search web_search for query
2. Fetch relevant URLs with web_fetch
3. Verify API/framework docs
4. Extract key information
5. Cache results to avoid redundant requests
6. Save research to memory/sessions/

**Guardrail:** Max 2 attempts per API endpoint; respect rate limits

---

## Automation Pattern 7: Subagent Delegation

**Trigger:** Complex task suitable for subagent

**Steps:**
1. Define subagent task and scope
2. Invoke task tool with detailed prompt
3. Monitor subagent progress
4. Collect subagent output
5. Integrate results into main task
6. Log subagent execution to subagent-log.jsonl

**Guardrail:** Subagents cannot modify system files without root confirmation

---

## Automation Pattern 8: Scheduled Tasks

**Trigger:** Cron schedule or timer

**Steps:**
1. Define schedule (cron expression)
2. Create background process via shell_background
3. Execute task on schedule
4. Log execution to logs/
5. On failure: apply Pattern 2 (bug fixing)
6. Notify user via Android status bar (if available)

**Guardrail:** Max 10 concurrent cron jobs

---

## Automation Pattern 9: UI Generation

**Trigger:** User needs dashboard, diagram, or HTML UI

**Steps:**
1. Determine UI type (dashboard, Mermaid, HTML)
2. Fetch data from metrics/logs if needed
3. Generate UI content
4. Save to workspace/
5. Offer preview via browser_navigate

**Guardrail:** Image generation requires user approval (external dependency)

---

## Automation Pattern 10: Android Inspection

**Trigger:** User needs Android system info

**Steps:**
1. Read system properties (getprop ro.build.version.*)
2. Check root status (su -c 'id')
3. Read logcat (read_logcat tool)
4. Inspect telephony state
5. Compile native code if needed (aarch64)
6. Save results to diagnosis/

**Guardrail:** Root requires KernelSU grant; read-only telephony

---

## Automation Pattern 11: Knowledge Tasks

**Trigger:** DSA, math, system design, logic problem

**Steps:**
1. Parse problem statement
2. Search web for references if needed (Pattern 6)
3. Solve using local computation/reasoning
4. Verify solution
5. Document approach and solution
6. Save to knowledge-tasks.log

**Guardrail:** Math computation local only; no external API for sensitive data

---

## Automation Pattern 12: Custom Extensions

**Trigger:** User needs new skill or capability

**Steps:**
1. Define skill requirements
2. Create skill via skill_manage
3. Test skill execution
4. Index skill in memory-index.json
5. Load skill on future sessions via autostart

**Guardrail:** Skill creation requires user approval

---

## Automation Readiness Matrix

| Pattern | Pillar | Status | Memory Required |
|---------|--------|--------|-----------------|
| Code Generation | P-01 | READY | repair-playbook, failure-patterns |
| Bug Fixing | P-01, P-02 | READY | failure-guard, retry-policy |
| Refactoring | P-01 | READY | AGENT_RULES.md |
| App Building | P-01, P-02 | PARTIAL | retry-policy (Gradle workaround) |
| System Monitoring | P-02 | READY | monitoring logs, alerts |
| Web Research | P-03 | READY | failure-patterns (network, rate-limit) |
| Subagent Delegation | P-04 | READY | sessions index |
| Scheduled Tasks | P-04 | READY | retry-policy |
| UI Generation | P-05 | PARTIAL | dashboards.md |
| Android Inspection | P-06 | READY | failure-patterns (permission) |
| Knowledge Tasks | P-07 | READY | failure-patterns, repair-playbook |
| Custom Extensions | P-08 | READY | memory-index.json |
