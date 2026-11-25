# Tool Inventory — All Capabilities
# Android Self-Healing Autonomous Agent (v1.0.0)

## P-01: Coding & Development

### Available Tools
| Tool | Purpose | Status |
|------|---------|--------|
| write_file | Create/overwrite any file | READY |
| edit_file | Surgical string replacement | READY |
| multi_edit | Multiple edits to one file | READY |
| apply_patch | Unified diff application | READY |
| python3 | Python execution | READY |
| node | Node.js execution | READY |
| gcc/clang | C/C++ compilation | READY |
| rustc | Rust compilation | READY |
| java/javac | Java compilation | READY |
| gradle | Gradle builds | PARTIAL |
| git | Version control | READY |
| bash | Shell scripting | READY |

### Verified Capabilities
- Full-stack scaffolding (Python, Node, C/C++, Rust, Bash)
- Surgical code edits via replace_file_content
- Bug fix & error analysis via logcat + stack trace parsing
- Refactoring & optimization across multi-file projects
- Full app building (CLI, API, web)
- Code review & security analysis
- Multi-file project handling
- Documentation generation

### Memory Integration Points
- `memory/knowledge/command-repair-playbook.md` — FIX-002 (wrong path), FIX-003 (wrong syntax)
- `memory/indexes/memory-index.json` — code patterns index
- `logs/successful-fixes.jsonl` — coding fixes applied

### Automation Possibilities
- Auto-scaffold project from template
- Auto-fix syntax errors using known fixes
- Auto-refactor repeated code patterns
- Auto-generate README and API docs

### Guardrails
- Do not modify `/system`, `/vendor`, `/data/system` without explicit confirmation
- Do not delete files without user confirmation
- Do not run `curl | bash` or remote code execution without review

### Real-World Tasks
1. Build a CLI tool
2. Fix a bug in existing code
3. Refactor a module
4. Generate documentation

### Status: READY

---

## P-02: Terminal & System Commands

### Available Tools
| Tool | Purpose | Status |
|------|---------|--------|
| shell | Execute shell commands | READY |
| shell_background | Long-running processes | READY |
| pkg_install | Install Linux packages | READY |
| root_shell | KernelSU root execution | READY |

### Verified Capabilities
- Shell command execution
- Package management (pkg/apt)
- Python & Node ecosystem (pip/npm)
- Git version control
- Filesystem manipulation
- Long-running processes & daemons

### Memory Integration Points
- `memory/knowledge/retry-policy.md` — retry budgets for commands
- `memory/knowledge/failure-patterns.md` — FP-001 (permission), FP-002 (wrong path), FP-006 (timeout)
- `agent/bin/agent-run` — root command executor with self-healing retry
- `agent/bin/root` — root shell wrapper

### Automation Possibilities
- Scheduled system health checks
- Auto-package installation on missing dependency
- Auto-retry failed commands with backoff
- Daemon management (start/stop/monitor)

### Guardrails
- Destructive commands (rm -rf, dd, mkfs, fdisk) require `confirm`
- Root commands go through `agent-run` with logging
- Package modifications logged to `agent/logs/package-changes.log`

### Real-World Tasks
5. Build a REST API
8. Monitor system health
9. Install a package
10. Clone a Git repo

### Status: READY

---

## P-03: Web Search & Live Research

### Available Tools
| Tool | Purpose | Status |
|------|---------|--------|
| web_search | Live web search | READY |
| web_fetch | Direct URL reading | READY |

### Verified Capabilities
- Live web search
- Direct URL reading
- API/framework docs verification
- Stack Overflow problem solving

### Memory Integration Points
- `memory/knowledge/failure-patterns.md` — FP-004 (network), FP-005 (rate limit)
- `memory/knowledge/retry-policy.md` — API endpoint budget (2 attempts)
- `logs/api-calls.log` — web request history

### Automation Possibilities
- Auto-fetch latest API docs on version change
- Auto-search Stack Overflow for error messages
- Auto-verify framework compatibility
- Scheduled research digests

### Guardrails
- Respect API rate limits (max 2 attempts per endpoint)
- Do not scrape sites that block bots (check robots.txt)
- Cache results to avoid redundant requests

### Real-World Tasks
11. Search web for latest docs

### Status: READY

---

## P-04: AI Agents & Automation

### Available Tools
| Tool | Purpose | Status |
|------|---------|--------|
| task | Subagent invocation | READY |
| shell_background | Timers & cron jobs | READY |

### Verified Capabilities
- Subagent invocation
- Timers & cron jobs
- Autonomous task manager (agy-daemon pattern)
- File watchers & system events
- Android status bar notifications

### Memory Integration Points
- `memory/sessions/` — subagent session data
- `memory/indexes/memory-index.json` — subagent task index
- `logs/subagent-log.jsonl` — subagent execution history

### Automation Possibilities
- Delegate complex tasks to subagents
- Schedule recurring maintenance jobs
- Watch files for changes and trigger repairs
- Send Android notifications on task completion

### Guardrails
- Subagents cannot modify system files without root confirmation
- Cron jobs limited to 10 concurrent
- File watchers limited to 5 watched paths

### Real-World Tasks
12. Launch a subagent
13. Schedule a cron job

### Status: READY

---

## P-05: Visual & Interactive UI

### Available Tools
| Tool | Purpose | Status |
|------|---------|--------|
| browser_screenshot | Capture UI screenshots | READY |
| browser_navigate | Navigate to URLs | READY |

### Verified Capabilities
- Generative inline UI (HTML generation)
- Data dashboards (text-based via agent-status)
- Mermaid diagrams (markdown rendering)
- Image generation (via external tools — PARTIAL)

### Memory Integration Points
- `monitoring/dashboards.md` — 4 dashboard templates
- `logs/ui-generation.log` — UI generation history

### Automation Possibilities
- Auto-generate dashboards from metrics
- Auto-create Mermaid diagrams from system architecture
- Auto-generate HTML reports from task results

### Guardrails
- No automatic image generation without user approval (external dependency)
- UI generation limited to read-only preview mode

### Real-World Tasks
14. Generate a dashboard
15. Create a Mermaid diagram

### Status: PARTIAL (image generation requires external tooling)

---

## P-06: Android-Specific Powers

### Available Tools
| Tool | Purpose | Status |
|------|---------|--------|
| root_shell | KernelSU root execution | READY |
| read_logcat | Android logcat reading | READY |

### Verified Capabilities
- Hardware & system properties (getprop)
- Radio & telephony inspection
- Shizuku integration (rish)
- Local web server hosting
- Native compilation (aarch64)

### Memory Integration Points
- `memory/knowledge/failure-patterns.md` — FP-001 (permission/root)
- `agent/bin/root` — root shell wrapper
- `agent/diagnosis/` — Android diagnostics

### Automation Possibilities
- Auto-read system properties on session start
- Auto-inspect telephony state for connectivity issues
- Host local web server for debugging
- Compile native aarch64 binaries on-device

### Guardrails
- Root access requires KernelSU grant
- Do not modify system partitions without documented reason
- Telephony inspection is read-only

### Real-World Tasks
16. Read Android system properties
17. Inspect telephony state
18. Compile C/C++ natively

### Status: READY

---

## P-07: Knowledge & Problem Solving

### Available Tools
| Tool | Purpose | Status |
|------|---------|--------|
| web_search | Live web search | READY |
| web_fetch | Direct URL reading | READY |

### Verified Capabilities
- DSA (data structures & algorithms)
- Math/science computation
- System design
- Logic breakdown

### Memory Integration Points
- `memory/knowledge/failure-patterns.md` — problem taxonomy
- `memory/knowledge/command-repair-playbook.md` — fix patterns
- `logs/knowledge-tasks.log` — knowledge task history

### Automation Possibilities
- Auto-solve DSA problems from LeetCode-style prompts
- Auto-compute math/science expressions
- Auto-design system architecture from requirements
- Auto-break down complex logic into steps

### Guardrails
- Math computation limited to local evaluation (no external API for sensitive data)
- System design recommendations are advisory only

### Real-World Tasks
19. Solve a DSA problem

### Status: READY

---

## P-08: Customization & Extensions

### Available Tools
| Tool | Purpose | Status |
|------|---------|--------|
| skill_manage | Create/patch/delete skills | READY |
| skill_view | Load skill instructions | READY |

### Verified Capabilities
- Project-specific rules (AGENT_RULES.md)
- On-demand skills (skill_manage/skill_view)
- MCP servers (configurable)
- Persistent learning (memory system)

### Memory Integration Points
- `memory/indexes/memory-index.json` — skill index
- `memory/sessions/` — persistent session data
- `skills/` — on-demand skill storage

### Automation Possibilities
- Auto-create skills for repeated task patterns
- Auto-load project rules on session start
- Auto-update skills from successful fixes
- MCP server integration for external tools

### Guardrails
- Skill creation requires user approval (skill_manage is modifying)
- MCP server connections require explicit configuration
- Persistent learning respects 30-day record retention

### Real-World Tasks
20. Create a custom skill

### Status: READY
