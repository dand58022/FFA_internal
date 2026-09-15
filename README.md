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

## Frontend, visual design and privacy planning pass

- [Visual design system](docs/specs/2026-09-15-visual-design-system.md): semantic tokens, concrete screen layouts, reusable components and Windows display matrix.
- [Frontend design ADR](docs/decisions/2026-09-15-frontend-design-system.md): Tailwind, locally owned shadcn/Radix, Lucide, local fonts and CSS motion.
- [Security gap review](docs/SECURITY.md#practical-windows-gap-review-2026-09-15): clipboard, presentation, Windows lifecycle and retention additions.
- [Acceptance](docs/specs/2026-09-14-validation-acceptance.md#meeting-design-and-practical-privacy-proofs): additional A13–A18 gates integrated into M1–M9.

## Design review updates (2026-09-15)

- [Canonical data dictionary](docs/specs/2026-09-15-canonical-data-dictionary.md): the single allowlist of semantic paths for mappings, controls, rules and provenance.
- [Template classification and registry](docs/references/template-classification-and-registry.md): AcroForm, flat, XFA and eSignature-native classes, registry fields and attachments as a Stage 2 package item type.
- [Stack ADR](docs/decisions/2026-09-14-desktop-pdf-stack.md): overlay-first display adapter; output library chosen by M0 evidence, with pdf-lib's maintenance and garbage-collection limits recorded.
- [Handoff ADR](docs/decisions/2026-09-14-local-storage-and-handoff.md): New Outlook risk and the Graph draft alternative recorded, not built.
- [Data model](docs/specs/2026-09-14-data-mapping-sync.md#value-provenance): value provenance and confirmation per stored value.
- [Risk register](docs/references/risks-and-assumptions.md): output library, Outlook variant, flat/XFA templates and unassigned domain reviewer.

## Harness and future sessions

- [AGENTS.md](AGENTS.md): session entry and project boundaries.
- [.harness-context.md](.harness-context.md): current status, next task, approval gate.
- [WORKFLOW.md](WORKFLOW.md) and [process](docs/PROCESS.md): spec -> plan -> implementation -> verification -> updated knowledge.
- [Harness provenance](docs/references/harness-adoption.md): inspected upstream commit, actual conventions, and deliberate adaptations.
- [Decision records](docs/decisions/2026-09-14-desktop-pdf-stack.md): proposed architecture decisions; all await review.

The planning package contains Markdown documents, two static PNG design boards and repository housekeeping only. The two supplied root PDFs are unchanged and excluded from Git under the original no-upload instruction. There is no application scaffold, dependency installation, or automatic Harness bootstrap.

Repository: [FFA_internal](https://github.com/dand58022/FFA_internal). The owner authorized commits directly to `main` during the planning phase on 2026-09-15. Fresh clones need the two original PDFs supplied locally before PDF inspection or implementation tests can run.
