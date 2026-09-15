---
node_id: ref-faa-process
type: reference
title: FAA specification-to-verification process
created: 2026-09-14
updated: 2026-09-14
status: active
category: reference
tags: [process]
summary: Product-specific adaptation of the inspected Harness workflow.
---

# Project process

## 1. Read the contract

Start with `.harness-context.md`, the README review index, and the relevant dated spec. User instructions and recorded scope approval govern. Draft documents do not authorize implementation.

## 2. Use evidence and decisions

Consult the PDF findings/inventory, architecture, security, and ADRs. Do not infer fields or checkbox semantics from generic PDF names. New load-bearing decisions belong in `docs/decisions/YYYY-MM-DD-slug.md`; proposed decisions may change during review. Accepted decisions are superseded with a new ADR rather than silently rewritten.

## 3. Plan and approval

Multi-session work uses `docs/plans/active/YYYY-MM-DD-slug.md` with Goal, Architecture, Tech Stack, Out of scope, Status, tasks, and session checklist. Record approval scope/date after it occurs. Keep implementation tasks unchecked until verified. A small follow-up remains in the existing plan; a new substantial scope gets its own spec/plan.

## 4. Implementation

After approval, implement the plan's next dependency-ready milestone. For domain behavior and integration risks, establish a failing acceptance test, implement, and rerun. Keep mappings outside React. Do not introduce cloud defaults from upstream examples. Choose simple local adapters and concrete interfaces.

## 5. Verification

Run the checks named by the milestone. Distinguish executed evidence from proposals. PDF tests must reopen logical fields AND render appearances; a save call alone proves neither. Verify the packaged Windows application on the demo machine before claiming the demo is ready.

## 6. Review

Review behavior against the spec and acceptance IDs. For an authorized PR include `spec:`, `plan:`, and applicable `tracker:` and `kb-update:` links or clear reasons those systems do not apply. No remote issue tracker, review bot, CI workflow, or external communication is configured by this planning task.

## 7. Preserve knowledge

At session end update plan Done / Next / Open decisions, relevant spec changes, and `.harness-context.md`. Capture an observed failure/root cause/control in `docs/learnings/` when there is a real learning. Move the plan to `docs/plans/completed/` only when its acceptance is met. Keep a single authoritative definition per concern and link to it.
