# Story Pipeline v6.0 - Greek Pantheon Edition

Enhanced multi-agent pipeline featuring the Greek Pantheon: Metis (builder), Argus (inspector), Nemesis (test quality), specialized reviewers (Cerberus, Apollo, Hestia, Arete, Iris), Themis (arbiter for triage), and Mnemosyne (reflection).

## What's New in v4.0

### 1. Resume Builder (v3.2+)
**Token Efficiency: 50-70% savings**

- Phase 3 now RESUMES Builder agent with review findings
- Builder already has full codebase context
- More efficient than spawning fresh Fixer agent

### 2. Inspector Code Citations (v4.0)
**Evidence-Based Verification**

- Inspector must map EVERY task to file:line citations
- Example: "Create component" → "src/Component.tsx:45-67"
- No more "trust me, it works" - requires proof
- Returns structured JSON with code evidence per task

### 3. Remove Hospital-Grade Framing (v4.0)
**Focus on Concrete Verification**

- Dropped psychological appeal language
- Kept rigorous verification gates and bash checks
- Replaced with patterns/verification.md + patterns/tdd.md

### 4. Micro Stories Get Security Scan (v4.0)
**Even Simple Stories Need Security**

- No longer skip ALL review for micro stories
- Still get 2 reviewers: Security + Architect
- Lightweight but catches critical vulnerabilities

### 5. Test Quality Agent + Coverage Gate (v4.0)
**Validate Test Completeness**

- New Test Quality Agent validates:
  - Edge cases covered (null, empty, invalid)
  - Error conditions tested
  - Meaningful assertions (not just "doesn't crash")
  - No flaky tests (random data, timing)
- Automated Coverage Gate enforces 80% threshold
- Builder must fix test gaps before proceeding

### 6. Playbook Learning System (v4.0)
**Continuous Improvement Through Reflection**

- **Phase 0:** Query playbooks before implementation
- Builder gets relevant patterns/gotchas upfront
- **Phase 6:** Reflection agent extracts learnings
- Auto-generates playbook updates for future agents
- Bootstrap mode: auto-initializes playbooks if missing

## Pipeline Flow - 7 Named Phases

```
Phase 1: PREPARE ──────────────────────────────────────
         Story quality gate + playbook query
         ↓
Phase 2: BUILD ────────────────────────────────────────
         🔨 Metis (initial implementation with TDD)
         ↓
Phase 3: VERIFY ───────────────────────────────────────
         👁️ Argus + 🧪 Nemesis + Reviewers (parallel)
         ↓
Phase 4: ASSESS ───────────────────────────────────────
         Coverage gate + ⚖️ Themis triages issues
         ↓
Phase 5: REFINE ───────────────────────────────────────
         🔨 Metis fixes MUST_FIX (iterative loop, max 3)
         ↓
Phase 6: COMMIT ───────────────────────────────────────
         Orchestrator reconciliation (evidence-based)
         ↓
Phase 7: REFLECT ──────────────────────────────────────
         📚 Mnemosyne updates playbooks
```

## 6-Tier Complexity Routing

| Tier | Phase 3 Agents | Reviewers |
|------|----------------|-----------|
| **trivial** | Argus only | None (1 agent) |
| **micro** | Argus + Nemesis + 2 | Cerberus + Hestia |
| **light** | Argus + Nemesis + 3 | Cerberus + Hestia + Apollo |
| **standard** | Argus + Nemesis + 4 | Cerberus + Hestia + Apollo + Arete |
| **complex** | Argus + Nemesis + 5 | All above + Iris (if frontend) |
| **critical** | Argus + Nemesis + 6 | All reviewers |

## Quality Gates

- **Coverage Threshold:** 80% line coverage required
- **Task Verification:** ALL tasks need file:line evidence
- **Critical Issues:** MUST fix
- **High Issues:** MUST fix

## Token Efficiency

- Phase 2 agents spawn in parallel (same cost, faster)
- Phase 3 resumes Builder (50-70% token savings vs fresh agent)
- Phase 4 Inspector only (no full re-review)

## Playbook Configuration

```yaml
playbooks:
  enabled: true
  directory: "docs/playbooks/implementation-playbooks"
  bootstrap_mode: true  # Auto-initialize if missing
  max_load: 3
  auto_apply_updates: false  # Require manual review
  discovery:
    enabled: true
    sources: ["git_history", "docs", "existing_code"]
```

## How It Avoids CooperBench Coordination Failures

Unlike the multi-agent coordination failures documented in CooperBench (Stanford/SAP 2026):

1. **Sequential Implementation** - ONE Builder agent implements entire story (no parallel implementation conflicts)
2. **Parallel Review** - Multiple agents review in parallel (safe read-only operations)
3. **Context Reuse** - SAME agent fixes issues (no expectation failures about partner state)
4. **Evidence-Based** - file:line citations prevent vague communication
5. **Clear Roles** - Builder writes, reviewers validate (no overlapping responsibilities)

The workflow uses agents for **verification parallelism**, not **implementation parallelism** - avoiding the "curse of coordination."

## Files - Greek Pantheon Agents

**Core Agents:**
- `agents/builder.md` - **Metis** 🔨 - Implementation agent (with playbook awareness)
- `agents/inspector.md` - **Argus** 👁️ - Validation agent (requires code citations)
- `agents/test-quality.md` - **Nemesis** 🧪 - Test quality validation
- `agents/arbiter.md` - **Themis** ⚖️ - Issue triage (MUST_FIX/SHOULD_FIX/STYLE)
- `agents/fixer.md` - **Metis** 🔨 (resumed) - Issue resolution
- `agents/reflection.md` - **Mnemosyne** 📚 - Playbook learning agent

**Reviewer Squad:**
- `agents/security-reviewer.md` - **Cerberus** 🔐 - Security review
- `agents/logic-reviewer.md` - **Apollo** ⚡ - Logic/performance review
- `agents/architect-integration-reviewer.md` - **Hestia** 🏛️ - Architecture review
- `agents/quality-reviewer.md` - **Arete** ✨ - Code quality review
- `agents/ux-accessibility-reviewer.md` - **Iris** 🌈 - Accessibility review

**Workflow Config:**
- `workflow.yaml` - Main configuration (v4.0)
- `workflow.md` - Complete step-by-step documentation

**Templates:**
- `../templates/implementation-playbook-template.md` - Playbook structure

## Usage

```bash
# Run story-pipeline
/story-pipeline story_key=17-10
```

## Backward Compatibility

Falls back to single-agent mode if multi-agent execution fails.
