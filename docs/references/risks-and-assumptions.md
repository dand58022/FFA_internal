---
node_id: ref-risks-assumptions
type: reference
title: Ranked risks, assumptions and decision gates
created: 2026-09-14
updated: 2026-09-15
status: draft
category: reference
tags: [risks, decisions]
summary: Non-blocking Stage 1 defaults and the confirmations required at later implementation or production gates.
---

# S. Risks and open questions

None of the questions below prevents drafting the architecture. Owner approval of coding is the current action gate. Defaults are recommendations for the fictional demo, not firm approval.

## Critical

| Risk / question | Stage 1 default and mitigation | Confirm by / owner |
|---|---|---|
| Can the display adapter support immediate updates/direct edits without focus or state loss? | Build controlled overlays at annotation rectangles first (inventory has only text and checkbox widgets); PDF.js annotation-layer widgets are the fallback. Prove both real forms and dense matrices. Do not downgrade to static preview. | M0 exit / technical lead |
| Can the output library produce valid visible checkmarks, preserve field trees/signature areas and provably remove internal fee data? | pdf-lib is unmaintained since 2021 and does not garbage-collect on save. Build the client copy into a fresh document, search every decoded output object for page 7 content, and evaluate a maintained fork, MuPDF (license review required) and PDFium against the same fixtures before choosing. | M0/M7 / technical lead |
| Intake page 7 says Internal Use Only. Which pages may clients receive? | Proposed client pages 1-6, internal archive 1-7; explicit page policy in manifest. Do not email page 7 by default. | Demo final export policy M7 / owner; before real use / firm form owner |
| "PII stays local" versus online Outlook Drafts synchronization | App stays offline/local internally; handoff to Outlook is explicit. Demo uses fictional data and offline rehearsal. Absolute no-egress would require disabling online Outlook handoff. | Demo rehearsal / owner; before real PII / firm security |
| Are originals current/authorized and are downstream signatures preserved adequately? | Treat dated 2022 PDFs as representative demo templates; leave client consent/signature/date empty, no regulatory claims. | Before real submissions / firm form owner and signing-process owner |

## Important

| Risk / question | Assumption and mitigation | Confirm by / owner |
|---|---|---|
| Classic Outlook present/configured? PowerShell permitted? New Outlook default? | Classic Outlook with a configured local profile; fixed helper. C# bridge is contingency if scripts blocked, not a policy bypass. New installations increasingly default to New Outlook, which has no Object Model; check the actual laptop as the first M1 task and keep the Graph draft alternative in the handoff ADR as the documented fallback. | M1 first task and M8 / demo owner + IT |
| Which questions are required/conditional? | Explicit demo matrix, checked for consistency against printed form; green means configured preparation only. No inferred suitability or compliance rules. | M3 / advisor-domain reviewer |
| Advisor-domain reviewer is a placeholder role | The M3 requiredness matrix, the recommendation/employment exclusivity rules and the dictionary enum values all need a named reviewer with advisor experience. Without one, M3 ships demo assumptions as if they were firm rules. Assign before coding approval. | Coding approval / owner |
| Real form library contains flat or XFA templates | Stage 1 engine assumes AcroForms. Classify the pilot library early; flat forms need a coordinate placement path, XFA forms need replacement or flattening by the form owner. See the [template reference](template-classification-and-registry.md). | Stage 2 planning / owner + technical lead |
| Can more than one recommendation apply? Can retired and employed with new employer coexist? | One recommendation per Stage 1 transaction as a declared demo rule. Former-employment detail checkboxes remain independently selectable unless firm gives an exclusivity rule. | M3 / advisor-domain reviewer |
| Name/email shared; address demo cannot use real TWS fields | Demonstrate real-form name/sponsor/plan reuse, then address on sample forms. | Script review / owner |
| Preselected original checkboxes and whitespace name | Clear factual values in new working state; never infer IRA, documents received or disclosure acknowledgement from template `/V`. | M0/M3 / technical lead |
| Older cases versus updated reusable profile | Active-case snapshot updates explicitly; historical cases/finalized revisions remain stable until refresh/new revision. | Spec review / owner |
| Export can create ordinary plaintext files | Keep encrypted artifacts by default; stage only for draft or explicit export; cleanup after confirmed by-value attachment. | M7/M8 / technical lead |
| Windows key loss or corrupted vault | Clear failure and last-good encrypted backup; no silent reset. No portable recovery promise. | M2 / technical lead; production recovery review later |
| Fee cells have no universal unit | Typed amount/percent/range values, no automatic mixed-unit summation or financial comparison verdict. | M3 / advisor-domain reviewer |
| Long/non-ASCII values clip or fail encoding | Approved bundled font, appearance tests, explicit overflow blockers; no silent truncation. | M0/M7 / technical lead |
| Performance and tiny controls on presentation screen | Test target laptop/DPI early, measure p95 widget patch latency, enlarge document view without changing form content. | M4/M9 / demo owner |
| Branding/form use or commercial SDK rights | Samples visibly labeled; avoid claiming carrier approval; inventory dependency/font licenses. SDK only if proven need and later approved. | M1/M9 / owner |

## Later

Firm identity/roles, key escrow/recovery, encrypted database scale, compliant audit retention, legal holds, multi-advisor concurrency, supporting-document imports and malicious PDF handling, electronic signatures, carrier APIs/session rules, subscriptions/device licensing, firm mappings, migration, MSI/code signing/Defender reputation, secure updates and PII-free support diagnostics. These belong to Stage 2/3, not hidden Stage 1 deliverables. Template classification, the template registry and attachments as a package item type moved from Stage 3 to Stage 2 on 2026-09-15; see the [product roadmap](../specs/2026-09-14-stage-1-product.md#r-roadmap).

## Decision register

| Decision | Proposed direction | Durable record |
|---|---|---|
| Desktop/PDF | Electron/React/TS, PDF.js display + pdf-lib output, feasibility first | [Stack ADR](../decisions/2026-09-14-desktop-pdf-stack.md) |
| State/approval | Main-authoritative revisions, active client snapshot, semantic document overrides, immutable finalized packages | [State ADR](../decisions/2026-09-14-state-and-approval.md) |
| Storage/handoff | AES-GCM JSON + Windows key wrapping, controlled client output and Classic Outlook draft | [Storage ADR](../decisions/2026-09-14-local-storage-and-handoff.md) |

Review questions for the owner: does the six-card scope fit the VP story; is application approval the desired Stage 1 signature behavior; is the proposed internal/client page separation appropriate for the demo; and which machine/Classic Outlook installation is the presentation target? Answers improve execution preparation but need not interrupt this planning deliverable.
