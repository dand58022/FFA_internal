---
node_id: ref-vp-demo-script
type: reference
title: Four-minute VP demonstration
created: 2026-09-14
updated: 2026-09-15
status: draft
category: reference
tags: [demo, product]
summary: Click-by-click fictional-data walkthrough from selection to unsent Classic Outlook draft.
---

# Q. VP demonstration - target 4 minutes 30 seconds

## Before the meeting

Use the packaged build and an offline Classic Outlook demo profile. Load only fictional fixtures; no real inbox/client data on screen. Rehearse draft opening, app restart/decrypt, zoom and display scaling. Preflight template hashes and clear leftover demo drafts manually. Do not send any email during the demo.

Fixture: Jordan Avery, `jordan.avery@example.invalid`, 100 Example Lane, Sample City, FL 00000; fictional Harbor Example Company and Harbor Example Retirement Plan. Avoid geocoding/address-verification claims. If an identifier is needed, use clearly invalid synthetic `000-00-0000` under a demo syntax-only rule; never present it as a legitimate SSN. DOB/financial data are invented. Fixture is an explicit presenter action, never a business-logic recommendation.

Load Demo Client fills all applicable demo-required fields across the selected package except **Quarterly statements supplied** on intake page 2. The controlled fixture uses a consistent employer-plan case and fills declared applicable matrix cells with fictional answers. Unit tests enforce the intentional one-issue fixture so evolving mappings do not wreck the script.

## Synthetic fixture contract

This table is the canonical planning fixture; M3 creates the runnable local fixture and its rule-completeness tests after coding approval. Values are invented, not imported client records. Demo field rules may accept invalid all-zero identifier syntax; production identity validity is not implied.

| Field | Fictional value / rule |
|---|---|
| Client name | Jordan Avery |
| Email | `jordan.avery@example.invalid` |
| Address | 100 Example Lane, Sample City, FL 00000; explicitly non-deliverable demo address |
| Phone | 202-555-0100 (fictional-use 555-01xx range) |
| DOB | 1980-01-15; invented |
| SSN / Tax ID | `000-00-0000` / `00-0000000`; deliberately invalid, sample fields only |
| Government ID | `DEMO-NOT-VALID`; clearly synthetic label |
| Sponsor / plan | Harbor Example Company / Harbor Example Retirement Plan |
| Sponsor contact | Demo Plan Desk, 202-555-0101 |
| Employer status | Former employee; departure 2025-12-31 |
| Account / recommendation | Employer plan; rollover to IRA, manually fixture-selected with no recommendation logic |
| Sample transaction amount | 100000.00 USD; invented |
| Sample beneficiary | Casey Example, spouse, 100% allocation |
| Required matrix cells | M3 assigns explicit synthetic answers for each applicable configured row and tests exact issue set; no financial inference from this prose |
| Intentional omission | Quarterly statements supplied on intake page 2 only |
| Client signatures/dates | Blank, downstream signing pending |

Before screen sharing, enable Presentation Mode and verify the current PDF page is suitable to show. Use its quiet PDF visibility note when explaining privacy; never show actual account data or Outlook inboxes. Demonstrate Reveal/Hide on a sample identifier if useful, then remask. Internal page review happens only after screen sharing is paused outside the app and Presentation Mode is explicitly left; the app cannot pause Teams/Zoom itself.

## Script

| Time | Click / action | Say | Visible proof |
|---|---|---|---|
| 0:00-0:20 | Log in | "This is a local advisor workspace for preparing a document package during the meeting." | Simple login, six professional cards |
| 0:20-0:40 | Select both TWS forms, Client Profile - Sample and Product Application - Sample; Continue | "Choose the forms needed for this case. The sample forms show how the same workflow can expand." | Four selected tabs, real/sample badges, split workspace |
| 0:40-1:00 | Type Jordan Avery once; toggle TWS tabs | "The name is entered once and appears in both actual forms." | Real rollover `text_01_es_:prefill` and intake `text_01` update |
| 1:00-1:20 | Load Demo Client; open sample tabs; change address to 102 Example Lane | "Case facts and client information are reused wherever the selected forms ask for them." | Both samples update address; real forms retain their relevant plan/name fields |
| 1:20-1:50 | Focus Document; edit intake's current-plan name directly to Harbor Example Plan - Legacy | "This is an intentional exception for this form. It does not change the client or the other document." | Manual override indicator; rollover retains shared plan name |
| 1:50-2:10 | Change shared plan name, then reset intake override | "Shared edits preserve that exception until I explicitly restore synchronization." | Override survives shared change; Reset to synced value restores latest plan |
| 2:10-2:30 | Intake page 3: click Monitoring / Current plan Yes then No | "The form knows these boxes are one choice, so it keeps the pair consistent." | Yes turns off when No turns on; no suitability recommendation is generated |
| 2:30-3:00 | Verify Documents; click the missing quarterly-statements issue | "Before approval, the app checks the requirements we configured and takes me directly to the missing answer." | Red issue, approval disabled, page 2 exact pair highlighted |
| 3:00-3:20 | Choose Provided directly on the highlighted pair; Verify again | "We can correct it right here. Client signatures are still a separate downstream step." | No blockers; preparation passed and signatures pending |
| 3:20-3:40 | Approve Package; Finalize Package | "My approval applies to this exact revision. The app prepares the client copies and keeps the internal record." | App approval event; finalized attachment list; intake internal fee page excluded from client copy |
| 3:40-4:20 | Open Outlook Draft | "Outlook opens with the documents attached. The advisor reviews the draft and controls sending." | Normal Classic Outlook draft with recipient, subject, body and four client PDFs; no automatic send |
| 4:20-4:30 | Leave draft open | "The proof is one entry workflow, controlled exceptions, checks before completion, and the actual documents ready for review." | Completed preparation handoff, not a Sent claim |

## Fallback if Outlook is unavailable during presentation

Show the genuine error and the verified client-facing export list, then export the fictional PDFs if useful. Say "This machine does not have the configured Classic Outlook integration; the prepared PDFs remain available." This is a graceful fallback, not successful proof of the Outlook milestone. Rehearsal must establish the real Outlook moment before considering the demo ready.
