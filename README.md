# BMad Story Engine

A BMAD-METHOD external module providing a multi-agent story development engine with gap analysis, batch processing, playbook learning, and quality gates.

## Overview

This module extends the BMad Method with advanced implementation workflows that orchestrate multiple specialized AI agents to ensure high-quality code delivery through:

- **Multi-Agent Pipelines**: The Greek Pantheon - Metis (Builder) → Argus (Inspector) → Nemesis (Test Quality) → Reviewers → Themis (Arbiter) → Mnemosyne (Reflection)
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

## Agents - The Greek Pantheon

### Core Agents

| Agent | Greek Name | Domain | Role |
|-------|------------|--------|------|
| `builder` | **Metis** 🔨 | Goddess of Wisdom & Craft | TDD Implementation Specialist - "With the wisdom of the Titans" |
| `inspector` | **Argus** 👁️ | The All-Seeing Giant | Independent verification with code citations - "With a hundred eyes" |
| `test-quality` | **Nemesis** 🧪 | Goddess of Retribution & Balance | Test quality analyst - "Justice demands tests that assert the truth" |
| `arbiter` | **Themis** ⚖️ | Titan of Justice | Triages feedback into MUST_FIX / SHOULD_FIX / STYLE |
| `reflection` | **Mnemosyne** 📚 | Titan of Memory | Knowledge curator - "What is written in memory endures forever" |

### Reviewer Squad

The pipeline spawns specialized reviewers based on story complexity (6-tier scale):

| Reviewer | Greek Name | Domain | Focus |
|----------|------------|--------|-------|
| `security` | **Cerberus** 🔐 | Three-Headed Guardian | Vulnerabilities, auth, injection |
| `logic` | **Apollo** ⚡ | God of Reason & Truth | Bugs, edge cases, performance |
| `architect` | **Hestia** 🏛️ | Goddess of Structure | Patterns, integration, routes |
| `quality` | **Arete** ✨ | Personification of Excellence | Code smells, maintainability |
| `accessibility` | **Iris** 🌈 | Goddess of the Rainbow | WCAG, ARIA, screen readers |

### 6-Tier Complexity Routing

| Tier | Reviewers | Description |
|------|-----------|-------------|
| **trivial** | 1 (Argus only) | Static content, copy changes |
| **micro** | 2 (Cerberus + Hestia) | Simple component, no API |
| **light** | 3 (+Apollo) | Basic CRUD, simple form |
| **standard** | 4 (+Arete) | API integration, user input |
| **complex** | 5 (+Iris if frontend) | Auth, migration, database |
| **critical** | 6 (all reviewers) | Payment, encryption, PII |

### Conditional Reviewers

**Iris** (Accessibility) is **automatically added** when frontend files are detected:

```bash
# Iris is invoked when git diff includes frontend files:
git diff --name-only | grep -E "\.(tsx|jsx|vue|css|scss|html)$|components/|pages/"
```

## Story Pipeline v6.0 - The Greek Pantheon

```
┌─────────────────────────────────────────────────────────────┐
│           STORY PIPELINE v6.0 - GREEK PANTHEON               │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Phase 1: PREPARE ─────────────────────────────────────────│
│     Story quality gate + playbook query                     │
│                                                              │
│  Phase 2: BUILD ───────────────────────────────────────────│
│     🔨 Metis implements with TDD                            │
│                                                              │
│  Phase 3: VERIFY ──────────────────────────────────────────│
│     👁️ Argus (Inspector)     ┐                              │
│     🧪 Nemesis (Test Quality) ├─ Run in parallel            │
│     🔐 Cerberus (Security)    │                              │
│     ⚡ Apollo (Logic)         │                              │
│     🏛️ Hestia (Architecture)  │                              │
│     ✨ Arete (Quality)        │                              │
│     🌈 Iris (Accessibility)  ┘                              │
│                                                              │
│  Phase 4: ASSESS ──────────────────────────────────────────│
│     Coverage gate + ⚖️ Themis triages issues                │
│     (MUST_FIX / SHOULD_FIX / STYLE)                         │
│                                                              │
│  Phase 5: REFINE ──────────────────────────────────────────│
│     🔨 Metis fixes MUST_FIX issues                          │
│     Loop until clean (max 3 iterations)                     │
│                                                              │
│  Phase 6: COMMIT ──────────────────────────────────────────│
│     Reconcile story, update sprint status                   │
│                                                              │
│  Phase 7: REFLECT ─────────────────────────────────────────│
│     📚 Mnemosyne updates playbooks                          │
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
