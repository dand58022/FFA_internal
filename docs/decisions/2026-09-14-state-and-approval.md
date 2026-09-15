---
node_id: adr-2026-09-14-state-and-approval
type: adr
title: Canonical state and revision-bound approval
created: 2026-09-14
updated: 2026-09-14
status: draft
adr_status: proposed
category: decision
tags: [state, validation]
summary: Separate reusable profiles, case snapshots and document overrides, with main-authoritative transitions.
---

# ADR: Canonical state and revision-bound approval

## Status

Proposed.

## Context

An advisor must edit one PDF without changing the master or other documents. Verification and approval must not remain valid after material edits. Reopening older cases must not silently change them when a reusable profile has changed.

## Options considered

- PDF bytes as source of truth: convenient for direct editing, but mixes duplicate client values and makes override provenance ambiguous.
- Renderer store as sole authority: fast, but privileged operations could finalize stale or forged state.
- Main-authoritative revisioned domain with renderer projection: explicit invariants and serialized commands; requires acknowledgement and stale-command handling.

## Decision

Use the third option. Reusable profiles are copied to a case snapshot when starting/resuming with explicit adoption. A left-side reusable-data edit updates the active snapshot and profile in one local transaction; other cases retain their snapshots. Direct PDF edits set document overrides or document-only answers. One main reducer computes effective values and validates commands. The renderer displays an optimistic projection of the same deterministic rules.

Approval records the exact package content revision and mapping/template hashes. Any data/override/selection change invalidates verification, warning acknowledgements, and approval. Finalized artifacts are immutable revisions; correction creates a new draft revision.

## Consequences

Extra snapshot storage is trivial for the demo and preserves history. The UI must distinguish a newer profile from an older case snapshot and offer an explicit refresh diff. Resetting an override uses the latest active-case source, not its creation-time value. Blank and unchecked are valid override values, not absence.

## Follow-ups

- [ ] M2: tests for stale commands, explicit blanks, atomic choice changes, active versus older case behavior.
- [ ] M6: demonstrate re-verification after any content change and reject stale approval from main.

## References

[Data contract](../specs/2026-09-14-data-mapping-sync.md), [workflow](../specs/2026-09-14-stage-1-ux.md), [plan](../plans/active/2026-09-14-stage-1-demo.md).
