# AGENTS.md - FAA Client Intake

## Session start

1. Read `.harness-context.md` and `README.md`.
2. Read the relevant spec in `docs/specs/` and the Stage 1 plan in `docs/plans/active/`.
3. Read `docs/ARCHITECTURE.md`, `docs/SECURITY.md`, and relevant proposed/accepted ADRs before architecture changes.
4. Follow `WORKFLOW.md` and `docs/PROCESS.md`. Use the documents as durable project knowledge.

## Current authorization

Planning/specification only until the owner approves Stage 1 coding. Updating Markdown and inspecting supplied templates are authorized. Do not infer coding approval from a plan being present or from its `active/` directory.

## Commands

No app commands exist yet. Do not claim build, lint, typecheck, or app tests passed.
The plan names proposed future npm scripts; they become runnable only after the approved shell milestone.
For the initial documents, validate frontmatter with the inspected Harness checker and check Markdown links and PDF hashes. See `docs/references/harness-adoption.md`.

## Project structure

- Root PDFs: immutable supplied originals; use copies for future development.
- `docs/specs/`: dated product, UX, visual design system, data/mapping, and validation contracts.
- `docs/plans/active/`: proposed/ongoing work with Done / Next / Open decisions.
- `docs/plans/completed/`: plans move here only after acceptance evidence exists.
- `docs/decisions/`: architecture decision records (ADRs); supersede accepted records when decisions change.
- `docs/references/`: observed PDF inventory, demo script, risks, and Harness provenance.

## Boundaries

- Use fictional data only for this prototype. Never upload supplied PDFs, client data, case data, or filled artifacts to a cloud tool.
- No cloud backend, telemetry, remote logging, licensing, carrier automation, or client e-signing in Stage 1.
- PDF edits create document-specific overrides; they never silently update the reusable client profile.
- Never place an advisor signature in a client/investor or consent field.
- Outlook may create/display a draft only. No send capability or automatic send.
- Renderer has no arbitrary filesystem, shell, IPC-channel, or external-navigation access.
- Encrypt client/case/audit data locally; never persist values in logs, browser storage, or plaintext JSON.
- Keep originals untouched. Template hashes and mapping versions are part of document identity.
- Green verification means configured preparation checks passed; it is not a compliance or suitability judgment.
- Follow the visual design spec and frontend ADR for tokens/owned local components; Presentation Mode is privacy assistance, never PDF redaction or capture protection.

## Development workflow after approval

Keep scope, acceptance IDs, changes, checks, and remaining work current in the plan at session boundaries.
Record material decisions before implementing drift from the approved spec. Ask only for new scope or consequential unresolved decisions; do not re-request authorization for already approved routine work.
Use meaningful tests for domain invariants, PDF output, security boundaries, and Outlook handoff. Do not write tests that merely duplicate UI markup.
Do not push files, create issues, or message external services without session authorization.
The owner authorized direct commits/pushes to `main` for the planning phase on 2026-09-15, targeting `https://github.com/dand58022/FFA_internal`. Keep original PDFs ignored/local. For later implementation use `<type>/<slug>` branches unless instructed otherwise, with conventional commit subjects. PRs should link spec and plan and state tracker/knowledge-base applicability.

## Harness adaptation

These are product-owned documents following inspected Harness conventions, not generated managed blocks. No `.harness.lock`, vendored `docs/harness/`, plugin installation, or CI integration is claimed. Review an actual bootstrap separately after planning approval; do not overwrite product knowledge with unrelated web/cloud defaults.
