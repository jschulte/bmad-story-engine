# Agent Type Mapping - Hybrid Approach (v5.1)

This document defines the mapping between BSE pipeline roles and Claude Code's specialized agent types.

**Strategy:** Use the most capable Claude Code agent as the base, then layer our purpose-built persona on top.

## Builder Agents (Phase 2: BUILD)

| Story Type | Claude Code Agent | BSE Persona | Combined Power |
|------------|-------------------|-------------|----------------|
| React/Next.js components | `dev-frontend` | `builders/frontend-react.md` | Frontend expertise + Apollo persona |
| TypeScript APIs | `dev-typescript` | `builders/backend-typescript.md` | TS mastery + Hephaestus persona |
| Python backends | `dev-python` | `builders/backend-python.md` | Python expertise + Pythia persona |
| Go services | `dev-go` | `builders/backend-go.md` | Go mastery + Gopher persona |
| Database/Prisma | `database-administrator` | `builders/database-prisma.md` | DB expertise + Athena persona |
| Infrastructure | `engineer-deployment` | `builders/infrastructure.md` | DevOps + Atlas persona |
| General/Mixed | `general-purpose` | `builders/general.md` | Flexibility + Metis persona |

## Reviewer Agents (Phase 3: VERIFY)

| Review Type | Claude Code Agent | BSE Persona | Combined Power |
|-------------|-------------------|-------------|----------------|
| Security | `auditor-security` | `reviewers/security.md` | OWASP expertise + Cerberus persona |
| Architecture | `architect-reviewer` | `reviewers/architecture.md` | SOLID/patterns + Hestia persona |
| Performance | `optimizer-performance` | `reviewers/performance.md` | Perf optimization + Apollo persona |
| Test Quality | `testing-suite:test-engineer` | `validators/test-quality.md` | Test expertise + Nemesis persona |
| Accessibility | `accessibility-expert` | `reviewers/accessibility.md` | WCAG expertise + Iris persona |
| Code Quality | `general-purpose` | `reviewers/quality.md` | Flexibility + Arete persona |

## Validator Agents

| Validator | Claude Code Agent | BSE Persona | Combined Power |
|-----------|-------------------|-------------|----------------|
| Inspector (Argus) | `general-purpose` | `validators/inspector.md` | Task verification + evidence |
| Test Quality (Nemesis) | `testing-suite:test-engineer` | `validators/test-quality.md` | Test validation |

## Support Agents (Phase 4-7)

| Role | Claude Code Agent | BSE Persona | Combined Power |
|------|-------------------|-------------|----------------|
| Arbiter (Themis) | `general-purpose` | `support/arbiter.md` | Issue triage |
| Reflection+Report | `general-purpose` | `reflection-reporter.md` | Playbook updates + reporting |

## Multi-Reviewer (Consolidated Mode)

For trivial→standard complexity, use a single agent with all perspectives:

| Mode | Claude Code Agent | BSE Persona | Combined Power |
|------|-------------------|-------------|----------------|
| Consolidated | `architect-reviewer` | `multi-reviewer.md` | 4 perspectives in one pass |

## Implementation Notes

1. **Always layer the persona** - Even when using specialized agents, include the BSE persona for:
   - Pipeline-specific output formats (JSON artifacts)
   - Story context and acceptance criteria
   - Issue classification (MUST_FIX/SHOULD_FIX/STYLE)

2. **Model selection** - Use `opus` for builders and reviewers, `sonnet` for synthesis tasks

3. **Fresh context** - Reviewers should have `fresh_context: true` to avoid builder bias
