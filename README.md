# FAA Client Intake - Stage 1 planning package

**Status: draft for owner review. Application implementation has not started.**

A local Windows meeting workspace that collects client/case information once, fills selected forms, preserves document-specific edits, checks configured requirements, and prepares an advisor-reviewed Classic Outlook draft.

Start with the [product specification](docs/specs/2026-09-14-stage-1-product.md), then the [implementation plan](docs/plans/active/2026-09-14-stage-1-demo.md). The plan is pending approval; its location under `active/` does not authorize coding.

## Review order - requested deliverables A through T

| Deliverable | Source of truth |
|---|---|
| A. Executive summary | [Product spec](docs/specs/2026-09-14-stage-1-product.md#a-executive-summary) |
| B. Findings and field appendix | [PDF findings](docs/references/pdf-findings.md), [complete inventory](docs/references/pdf-field-inventory.md) |
| C. Stage 1 scope | [Product spec](docs/specs/2026-09-14-stage-1-product.md#c-recommended-stage-1-scope) |
| D. User workflow | [UX spec](docs/specs/2026-09-14-stage-1-ux.md#d-user-workflow) |
| E. Screen specifications | [UX spec](docs/specs/2026-09-14-stage-1-ux.md#e-screen-specification) |
| F. Data model | [Data and mapping spec](docs/specs/2026-09-14-data-mapping-sync.md#f-data-model) |
| G. Mapping schema and real examples | [Data and mapping spec](docs/specs/2026-09-14-data-mapping-sync.md#g-pdf-mapping-design) |
| H. Synchronization and overrides | [Data and mapping spec](docs/specs/2026-09-14-data-mapping-sync.md#h-live-synchronization-and-overrides) |
| I. Validation | [Validation and acceptance](docs/specs/2026-09-14-validation-acceptance.md#i-validation-model) |
| J. Security | [Security design](docs/SECURITY.md) |
| K. Stack decision | [Architecture](docs/ARCHITECTURE.md#k-tech-stack-decision) |
| L. Application architecture | [Architecture](docs/ARCHITECTURE.md#l-application-architecture) |
| M. Proposed folder structure | [Architecture](docs/ARCHITECTURE.md#m-proposed-project-structure) |
| N. State machine | [UX spec](docs/specs/2026-09-14-stage-1-ux.md#n-state-machine) |
| O. Implementation phases | [Stage 1 plan](docs/plans/active/2026-09-14-stage-1-demo.md) |
| P. Test strategy and 12 required proofs | [Validation and acceptance](docs/specs/2026-09-14-validation-acceptance.md#p-test-strategy) |
| Q. VP demo script | [Demo script](docs/references/vp-demo-script.md) |
| R. Stage 2 / Stage 3 | [Product roadmap](docs/specs/2026-09-14-stage-1-product.md#r-roadmap) |
| S. Ranked risks and assumptions | [Decisions and risks](docs/references/risks-and-assumptions.md) |
| T. Final recommendation | [Product spec](docs/specs/2026-09-14-stage-1-product.md#t-final-recommendation) |

## Harness and future sessions

- [AGENTS.md](AGENTS.md): session entry and project boundaries.
- [.harness-context.md](.harness-context.md): current status, next task, approval gate.
- [WORKFLOW.md](WORKFLOW.md) and [process](docs/PROCESS.md): spec -> plan -> implementation -> verification -> updated knowledge.
- [Harness provenance](docs/references/harness-adoption.md): inspected upstream commit, actual conventions, and deliberate adaptations.
- [Decision records](docs/decisions/2026-09-14-desktop-pdf-stack.md): proposed architecture decisions; all await review.

The planning package contains Markdown documents and repository housekeeping only. The two supplied root PDFs are unchanged and excluded from Git under the original no-upload instruction. There is no application scaffold, dependency installation, or automatic Harness bootstrap.

Repository: [FFA_internal](https://github.com/dand58022/FFA_internal). The owner authorized commits directly to `main` during the planning phase on 2026-09-15. Fresh clones need the two original PDFs supplied locally before PDF inspection or implementation tests can run.
