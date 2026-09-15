---
node_id: design-validation-acceptance
type: design
title: Preparation validation and Stage 1 acceptance
created: 2026-09-14
updated: 2026-09-14
status: draft
category: cross-domain
tags: [frontend, backend, testing]
author: Codex
reviewers: [project-owner, advisor-domain-reviewer, technical-reviewer]
summary: Configured preparation rules, revision-aware approval and observable end-to-end acceptance criteria.
---

# Validation and acceptance

## Load-bearing decisions

[Canonical state/approval](../decisions/2026-09-14-state-and-approval.md), [local handoff](../decisions/2026-09-14-local-storage-and-handoff.md).

## Problem, goals and non-goals

Find provable preparation omissions before approval and help the advisor correct them in context. Validate what will actually appear in each document, including overrides. Do not claim to validate investment suitability, recommendation legality, client signing, or every possible firm NIGO rule.

## I. Validation model

### Issue contract

```typescript
interface IssueBase {
  id: string;
  severity: 'error' | 'warning'; code: string; message: string;
  caseId: string; contentRevision: number;
  acknowledgementAllowed: boolean;
}
type ValidationIssue =
  | (IssueBase & { scope: 'field'; documentId: string; bindingId: string;
      ruleId: string; ruleVersion: string;
      targets: { fieldName: string; page: number; rect: number[] }[] })
  | (IssueBase & { scope: 'document'; documentId: string;
      recoveryAction: 'retryDocumentLoad' | 'reviewTemplate' | 'retryExport' })
  | (IssueBase & { scope: 'package';
      recoveryAction: 'retrySave' | 'reviewConfiguration' | 'retryFinalization' });
interface WarningAcknowledgement {
  issueId: string; ruleVersion: string; contentRevision: number;
  actorId: string; at: string; reasonCode: 'reviewedWithClient' | 'advisorReviewed';
}
```

Messages state the rule and corrective action without copying sensitive field values. Repeated fields can have several targets; navigate to the primary and offer the others. Field issues require a nonempty target list and rule identity. Operational failures with no field (save failure, missing mapping, invalid artifact) use a document/package issue with a service recovery action rather than an invented rectangle or binding. Operational issues are blocking errors and cannot be acknowledged; warning acknowledgements apply only to configured field/group rules.

### Rule semantics

| Rule | Behavior |
|---|---|
| Required scalar | Trim-aware nonblank check, applied at preparation stage only; an explicit blank override still fails |
| Required choice | At least one semantic answer; enum has at most one at every mutation; blank allowed during editing |
| Date | Parse real calendar date, ISO storage, configured US display; reject invalid day/month/leap-year combinations, not merely bad punctuation |
| SSN/tax-ID sample field | Validate configured syntax only; clearly synthetic fixture. Never claim the number is assigned or belongs to a person |
| Email | Required for email preparation; syntactic check is not deliverability verification |
| Conditional fields | Evaluate from the effective document choice, not raw master choice; departure -> date, Other -> description, relevant employer plan -> applicable questions |
| Exclusive choice | Reject/import-flag multiple selections; direct UI/reducer change clears all other members atomically |
| Independent acknowledgement | Remains freely clickable unless an explicit rule requires it; never auto-assert disclosure truth |
| Text capacity | Warn during entry; block final output if values would be clipped/unrepresentable. No silent truncation or unreadably small font |
| Fee values | Validate configured units/format only; do not add percentages to dollar amounts or generate a best-interest conclusion |
| Cross-document difference | Only configured comparison warnings; an intentional override is otherwise legitimate |
| Signature | Downstream client signatures/dates excluded from advisor-preparation blockers, even if AcroForm Required flag is set |
| Rule/template error | Block finalization; unknown rule cannot be treated as passing |

### Stage 1 rule baseline

These are **proposed demonstration preparation rules**, not firm compliance approval:

- Rollover: investor name, sponsor, current plan, sponsor contact, one employment attestation choice; departure date if departing; Other description if Other. Printed investor name must be present. Client consent/signature/date remain pending downstream. Recommendation disclosure stays a deliberate advisor-entered case answer; no inference.
- Intake: investor name; one account type; sponsor/current plan or corresponding explicit N/A; employer relationship if employer-plan account; retirement months when that intent is selected; one response for each applicable requested-document pair; one recommendation under the declared single-transaction assumption. Printed investor name present; signatures/date pending downstream.
- Alignment/service/factor rows: enforce max-one in each unambiguous need/Yes-No/enum group. Proposed demo requires applicable non-Other rows; new-employer column is conditional on a case-level "new employer plan available" fact. Other factor is optional, but choosing a need/alignment requires description. Firm reviewer must approve a complete per-group requiredness matrix in M3; never infer mandatory answers solely from there being a checkbox.
- Intake fee page: basis required when fee values are entered. Use actual versus benchmark-specific rules only when configured; no automatic totals of mixed units. Internal requirements may still block package preparation even though page 7 is omitted from client output.
- Samples: client name/address required for Profile/Product/Suitability; account/product and amount required for Product; any entered beneficiary requires name and a valid allocation (single-beneficiary sample totals 100%); Disclosure sample acknowledgement is optional unless its sample mapping explicitly requires it.
- Warning example: requested statements = Not Provided -> "Quarterly statements are not marked provided. Review before approval." Acknowledgement is permitted and audited. The answer remains Not Provided; acknowledging does not fabricate document receipt.

Before implementation, M3 produces the complete configured requirement matrix with stable rule IDs. A document can be green only for the declared rule set. The UI always qualifies it as preparation checks, and the spec never claims those rules cover all NIGO causes.

### Verification and approval guards

Flush renderer edits and require durable main revision before Verify. Run all selected documents from one consistent snapshot and record rule/template/mapping/export versions. Red errors prevent approval unconditionally. All configured warnings must be acknowledged at this revision before approval, but remain visible as acknowledged. Any content change invalidates the verification result, approval and all warning acknowledgements (simple conservative Stage 1 rule).

Check guards in main, including forged IPC tests. Approval creates an application event; it cannot invoke a client-signature writer. Finalizer rechecks revision equality, no pending saves, selected document identity and artifact policy before output. Reopen generated PDFs and compare expected effective values before reporting success.

## P. Test strategy

Use deterministic synthetic fixtures. Unit tests cover semantic invariants; PDF tests cover the real field structures and appearances; integration tests exercise storage and privileged commands; E2E tests exercise the meeting workflow. Manual inspection is necessary for final PDF appearances and the actual Windows Outlook experience. Tests described here are future requirements, not executed application tests.

### Twelve requested proofs

| ID | Test / observable pass condition | Layer / milestone |
|---|---|---|
| A01 | Enter shared client name and plan name; both real PDFs contain/display the mapped values. Address updates at least two selected sample PDFs. | Mapping + E2E, M3-M5 |
| A02 | Direct edit on document B changes only B; client profile, case source and A unchanged. Restart preserves it. | Reducer + persistence + E2E, M2/M5 |
| A03 | Change master again; override survives. Reset B returns latest adopted case value, including clearing a group override. | Unit + E2E, M2/M5 |
| A04 | Check/uncheck every real checkbox using actual `/1` and `/Off`; reopen fields and widgets and verify readable on/off appearances. Originals' hashes remain identical. | PDF fixtures, M0/M3/M7 |
| A05 | Every configured Yes/No or single-choice group remains max-one under left entry, direct clicks, rapid changes, reset and stale commands; invalid imported state blocks approval. | Unit + widget tests, M2/M4 |
| A06 | Blank a required effective field, including blank override; Verify reports red and main rejects approval/finalization even with a forged enabled UI. | Domain + integration, M6 |
| A07 | Click an issue on a non-active page; correct document opens, page scrolls into view and exact widget highlights/focuses at 75/100/150% zoom. | E2E/manual, M6 |
| A08 | Final output reopens with expected text/checkbox values and appearances, blank client signing fields, correct page audience and no hidden internal fee data. | PDF/export integration, M7 |
| A09 | SHA-256 of both original templates is identical before/after load, edit, verify, finalize, failure and retry. | PDF/integration, all PDF milestones |
| A10 | Classic Outlook displays saved unsent draft with correct To/Subject/Body and all client-facing attachments. No Send API, SendKeys or network mail path; inspect Drafts and unchanged Outbox/Sent counts in isolated profile. | Static + Windows integration, M1/M8 |
| A11 | Quit/restart under same Windows user decrypts saved client, case, overrides/audit and artifacts; other-user/missing-key/tamper cases fail safely. | Crypto + Windows integration, M2/M8 |
| A12 | Synthetic canary name/SSN/address never appear in app logs, browser storage, plaintext JSON, crash-upload configuration or command-line args; ciphertext and intentional attachment staging/export are distinguished. | Security scan + packaged test, M2/M8/M9 |

### Additional mandatory checks

- Unit: derived-name formatting; absence versus empty/null/false/zero; applicability; transactional group changes; input-loop suppression; source snapshot refresh; single-writer/stale command behavior; verification/approval invalidation; warning identity and acknowledgements.
- Mapping: each of 189 original fields has exactly one intended disposition; every declared name/type/page/export state exists; all new sample fields are covered; no cycles/unknown paths; original preselected boxes cleared on new case; all signature roles explicit.
- Date/fee fixtures: leap years, empty values, multiline/long names, apostrophes and supported non-ASCII names, display overflow, dollar/percentage distinctions, N/A and inactive overrides.
- Encryption: random nonces, authentication tamper rejection, wrong key/AAD, roundtrip and truncated envelopes, failed atomic replacement/disk full, last-good snapshot recovery, no plaintext error paths.
- IPC: reject renderer paths, `..`, UNC locations, untrusted senders, huge inputs, arbitrary command requests, signature writes and unsigned/outdated artifact IDs.
- Export: client intake subset removes page-7 widgets/field values and unreachable data; all seven pages retained internally; separate output docs prevent identical consent names colliding; retry produces one complete manifest and no plaintext working copies.
- Outlook: already running/cold start, configured offline profile, missing app/new-only, policy restriction, attachment failure, draft-saved-but-display-failed, 30-second timeout, crash/restart and duplicate-click cases. Never run a test that actually sends mail.
- Keyboard/visual: tab order, visible focus, label/readout, separator adjustment, DPI/zoom alignment, no per-keystroke reload, no unnecessary modal, all-six performance at target display sizes.
- Packaged EXE: clean Windows account, no Node/Python/npm installed, no network, correct bundled assets/worker/helper/fonts, launch from path with spaces, local appdata permissions, save/restart, final PDFs and Outlook. Installer/uninstaller must not accidentally erase case data; explicit demo cleanup owns deletion.

### Evidence and commands

M1 will define `npm run lint`, `npm run typecheck`, `npm run test:unit`, `npm run test:pdf`, `npm run test:integration`, `npm run test:e2e`, `npm run build`, `npm run package:win`. Until then these are proposed names, not existing executable commands. Each milestone records exact versions, command, result and artifact evidence in the plan. Timing measurements are local synthetic metrics, not telemetry.

### Release/demo exit criteria

All A01-A12 and relevant additional checks pass; six cards function; all selected outputs are current and correct; blocking issues cannot be bypassed; signatures stay with the client; original hashes match; offline packaged workflow and actual Outlook draft are rehearsed. Any unresolved appearance/export bug is a demo blocker, even if unit tests pass. Document remaining production gaps without presenting the prototype as production-ready.

## Alternatives and open questions

Visual-only PDF checks miss stale canonical field values; data-only checks miss invisible marks and clipping. Both are required. The outstanding firm requiredness, signing/export and email policy questions are tracked in the [risk register](../references/risks-and-assumptions.md); use stated demo defaults until confirmed.
