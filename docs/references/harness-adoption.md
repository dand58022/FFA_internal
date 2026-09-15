---
node_id: ref-harness-adoption
type: reference
title: Harness inspection and documentation adaptation
created: 2026-09-14
updated: 2026-09-14
status: active
category: reference
tags: [harness, provenance]
summary: Records the actual upstream conventions inspected before authoring this package.
---

# Harness reference and adaptation

## Evidence

Source: [yoji-labs/harness](https://github.com/yoji-labs/harness), commit `c2fd6d0e392ca21dcc40152bde8e8e423cb555b7`, inspected 2026-09-14. Public web fetch returned 404; local `git ls-remote` and shallow clone succeeded. The reference clone is outside the project at `C:/Users/dand5/.codex/tmp/faa-harness-reference-20260914`. Authentication/access differences explain the usable Git path; repository visibility was not independently queried.

Inspected: `README.md`, `WORKFLOW.md`, `docs/PROCESS.md`, `docs/OPERATING.md`, `docs/references/schema.md`, `templates/spec.template.md`, `templates/plan.template.md`, `templates/adr.md`, `templates/agents-md.md`, executable frontmatter checker, and the 2026-08-16 bootstrap spec / 2026-08-17 completed plan examples.

## Actual conventions adopted

| Upstream convention | FAA adaptation |
|---|---|
| Dated design specs in `docs/specs/` | Four focused design specs, linked from README in the requested A-T review order |
| Plans under `docs/plans/active/`, moved on completion | One Stage 1 milestone plan, `status: draft`, explicit pending-approval status |
| Product ADRs in `docs/decisions/` | Three proposed decisions with alternatives, consequences, and follow-ups |
| Stable `node_id` and YAML lifecycle metadata | All authored `docs/**/*.md` have frontmatter; proposed ADRs include `adr_status: proposed` |
| Short root `AGENTS.md` | Project entry point with concrete boundaries and no invented commands |
| `.harness-context.md` and declarative `WORKFLOW.md` | Durable current state and a short workflow linked to the project process |
| Domain docs and references | Architecture/security own technical contracts; references own observed evidence and demo guidance |
| Session status/checklists and decision updates | Done / Next / Open decisions retained in plan and session context |

The upstream ADR template omits `adr_status`, but its executable checker requires it; this package follows the checker. Cross-domain specs include reviewer roles, explicitly unassigned until the owner names people. This is routing metadata, not a claim that a firm or security reviewer approved the design.

## Deliberate limits

This is a **documentation-only adaptation**, not a Harness installation. No bootstrap script was run, no generated `.harness.lock` was fabricated, no upstream managed blocks were hand-edited, and no CI, plugins, cloud services, or issue tracker were installed. Product docs remain in the product-owned locations upstream recommends. An eventual bootstrap should preserve these files and choose a prototype/local-desktop configuration rather than blindly selecting web SaaS defaults.

The user requested both a draft spec AND a plan before review, overriding the generic skill sequence that normally pauses between those artifacts. No app implementation snippets or executable mappings are created; pseudotypes and JSON examples are design contracts. The workspace was not a Git repository, so no design commit is claimed. Git setup belongs in the approved development phase.

## Documentation verification

Use the checked-out upstream `checks.check_frontmatter.run` against every authored `docs/**/*.md`. Independently check relative Markdown links, unique node IDs, A-T coverage, and plan-to-spec consistency. Compare SHA-256 hashes of the two original PDFs against [PDF findings](pdf-findings.md). Runtime feasibility and app tests remain future work.
