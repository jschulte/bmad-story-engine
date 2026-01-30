# BMad Story Engine

A BMAD-METHOD external module providing a multi-agent story development engine with gap analysis, batch processing, playbook learning, and quality gates.

## Overview

This module extends the BMad Method with advanced implementation workflows that orchestrate multiple specialized AI agents to ensure high-quality code delivery through:

- **Multi-Agent Pipelines**: Builder → Inspector → Test Quality → Reviewer → Fixer → Reflection
- **Gap Analysis**: Validate story tasks against actual codebase implementation
- **Batch Processing**: Process multiple stories with complexity-based routing
- **Playbook Learning**: Extract patterns from implementations for future agents
- **Quality Gates**: Enforce test coverage thresholds and code citation requirements

## Installation

### Prerequisites

- [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) v6.0.0 or later
- Node.js >= 18.0.0

### Install via BMAD Installer

```bash
npx bmad-method install
```

During installation, select both:
- ✅ BMad Method (BMM) - Required dependency
- ✅ BMad Story Engine (BSE) - This module

### Manual Registration (for development)

Add to your project's external modules or BMAD's `external-official-modules.yaml`:

```yaml
modules:
  bmad-story-engine:
    url: https://github.com/jschulte/bmad-story-engine
    module-definition: src/module.yaml
    code: bse
    name: "BMad Story Engine"
    description: "Multi-agent story development engine"
    defaultSelected: false
    type: community
    npmPackage: bmad-story-engine
```

## Workflows

| Workflow | Phase | Description |
|----------|-------|-------------|
| `story-pipeline` | Execution | Multi-agent TDD pipeline with 6 specialized agents |
| `batch-stories` | Orchestration | Process multiple stories with complexity routing |
| `gap-analysis` | Preparation | Validate story tasks against actual codebase (pre-dev) |
| `create-story-with-gap-analysis` | Preparation | Story creation with systematic codebase analysis |
| `validate` | Verification | Post-dev verification with quick/deep modes |
| `revalidate-story` | Verification | Quick single-story validation check |
| `multi-agent-review` | On-demand | Smart multi-agent code review |
| `detect-ghost-features` | Maintenance | Reverse gap analysis - find code without specs |

### Workflow Phases

```
┌─────────────────────────────────────────────────────────────────┐
│                    DEVELOPMENT LIFECYCLE                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  PREPARATION          EXECUTION           VERIFICATION          │
│  ────────────         ─────────           ────────────          │
│                                                                  │
│  gap-analysis    ──▶  story-pipeline  ──▶  validate             │
│  create-story-       (multi-agent)        revalidate-story      │
│  with-gap-analysis                                              │
│                                                                  │
│  ORCHESTRATION        ON-DEMAND           MAINTENANCE           │
│  ─────────────        ─────────           ───────────           │
│                                                                  │
│  batch-stories        multi-agent-        detect-ghost-         │
│  (runs multiple)      review              features              │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Agents

### Core Agents

| Agent | Persona | Role |
|-------|---------|------|
| `builder` | Mason the Craftsman | TDD Implementation Specialist - "Measure twice, cut once" |
| `inspector` | Vera the Code Detective | Independent verification with code citations |
| `reflection` | Rita the Wise Librarian | Knowledge curator for playbook learning |
| `test-quality` | Tessa the Test Scientist | Test quality analyst and coverage validator |

### Reviewer Squad

The pipeline spawns multiple specialized reviewers based on story complexity:

| Reviewer | Persona | Focus | Used In |
|----------|---------|-------|---------|
| `security` | Sasha the Security Sentinel | Vulnerabilities, auth, injection | All stories |
| `logic` | Leo the Logic Hunter | Bugs, edge cases, performance | Standard + Complex |
| `architect` | Rosie the Architecture Guardian | Patterns, integration, routes | All stories |
| `quality` | Quinn the Quality Crusader | Code smells, maintainability | Complex only |

**Complexity → Reviewers:**
- **Micro**: Sasha + Rosie (2 reviewers)
- **Standard**: Sasha + Leo + Rosie (3 reviewers)
- **Complex**: Sasha + Leo + Rosie + Quinn (4 reviewers)

### Conditional Reviewers

These reviewers are **automatically added** when relevant files are detected:

| Reviewer | Persona | Focus | Triggered By |
|----------|---------|-------|--------------|
| `ux_accessibility` | Ada the Accessibility Advocate | WCAG, ARIA, keyboard nav, screen readers | Frontend files (*.tsx, *.jsx, *.css, components/*, pages/*) |

**Smart Detection:**
```bash
# Ada is invoked when git diff includes frontend files:
git diff --name-only | grep -E "\.(tsx|jsx|vue|css|scss|html)$|components/|pages/"
```

**Example Outcomes:**
- Backend API story (only .ts service files) → **No Ada** (saves tokens)
- Full-stack story (API + components) → **Ada added** to reviewer squad
- UI-only story (component changes) → **Ada included** for accessibility review

## Story Pipeline Flow

```
┌─────────────────────────────────────────────────────────────┐
│                      STORY PIPELINE                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────┐    ┌───────────┐    ┌──────────────┐         │
│  │ BUILDER  │───▶│ INSPECTOR │───▶│ TEST QUALITY │         │
│  │  (Mason)   │    │  (Vera)   │    │   (Tessa)    │         │
│  └──────────┘    └───────────┘    └──────────────┘         │
│       │                                    │                 │
│       │         Steps 1-4              Steps 5-6            │
│       │         Implement              Validate              │
│       │                                    │                 │
│       │              ┌───────────┐        │                 │
│       │              │ REVIEWERS │◀───────┘                 │
│       │              │Sasha/Leo/ │                          │
│       │              │Rosie/Quinn│                          │
│       │              └───────────┘                          │
│       │                   │                                  │
│       │               Step 7                                │
│       │           Code Review                               │
│       │                   │                                  │
│       │              ┌────▼────┐                            │
│       └─────────────▶│  FIXER  │  (Mason resumes)          │
│                      │         │                            │
│                      └────┬────┘                            │
│                           │                                  │
│                       Steps 8-9                             │
│                       Fix Issues                            │
│                           │                                  │
│                      ┌────▼────┐                            │
│                      │REFLECTION│                           │
│                      │  (Rita)  │                           │
│                      └──────────┘                           │
│                           │                                  │
│                       Step 10                               │
│                    Playbook Learning                        │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## Configuration

The module adds these configuration options during installation:

| Option | Default | Description |
|--------|---------|-------------|
| `enable_playbooks` | `true` | Enable playbook learning from implementations |
| `coverage_threshold` | `80` | Minimum test coverage percentage required |
| `enable_batch_processing` | `true` | Allow processing multiple stories |
| `require_code_citations` | `true` | Inspector must provide file:line evidence |
| `auto_fix_critical_issues` | `true` | Builder resumes to fix critical/high issues |

## Complexity Routing

The batch processor routes stories based on complexity:

| Level | Criteria | Pipeline |
|-------|----------|----------|
| **Micro** | ≤3 tasks, ≤5 files, no risk keywords | Lightweight path |
| **Standard** | ≤15 tasks, ≤30 files | Full pipeline |
| **Complex** | >15 tasks, security/auth/payment keywords | Enhanced validation |

Risk keywords that increase complexity:
- **High**: auth, security, payment, encryption, migration, database, schema
- **Medium**: api, integration, external, third-party, cache
- **Low**: ui, style, config, docs, test

## Dependencies

This module depends on:
- **BMM (BMad Method)**: Uses BMM's config and agents (sm, tea, dev)

## License

MIT

## Contributing

Contributions welcome! Please read the [BMAD-METHOD contributing guide](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/CONTRIBUTING.md) first.
