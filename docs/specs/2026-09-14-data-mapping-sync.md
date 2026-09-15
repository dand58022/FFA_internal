---
node_id: design-data-mapping-sync
type: design
title: Canonical data, PDF mappings and document overrides
created: 2026-09-14
updated: 2026-09-15
status: draft
category: cross-domain
tags: [frontend, backend, pdf]
author: Codex
reviewers: [project-owner, advisor-domain-reviewer, technical-reviewer]
summary: Precise source ownership, proposed mapping schema, real-field examples and synchronization invariants.
---

# Data, mapping and synchronization contract

## Load-bearing decisions

[Canonical state ADR](../decisions/2026-09-14-state-and-approval.md) and [desktop/PDF ADR](../decisions/2026-09-14-desktop-pdf-stack.md).

## Problem, goals and non-goals

One shared fact must populate many forms without letting a direct edit corrupt the reusable profile. Mapping must describe semantic questions and their PDF representation independently. Do not construct a general rule programming language, arbitrary executable transforms, or a user-facing mapping editor for the demo.

## F. Data model

Pseudotypes below are design examples, not application implementation. IDs are opaque random identifiers; dates are date-only ISO `YYYY-MM-DD`, timestamps UTC, and money uses decimal strings or integer minor units with an explicit currency. Do not use floating-point dollars for fee comparisons.

```typescript
type Answer = string | number | boolean | null | string[] | Record<string, unknown>;
type DateOnly = string;
type Revision = number;

interface ClientProfile {
  id: string; revision: Revision;
  firstName: string; middleName?: string; lastName: string;
  dob?: DateOnly; ssn?: string; taxId?: string;
  maritalStatus?: string; citizenship?: string;
  email?: string; phone?: string;
  address?: { line1: string; line2?: string; city: string; state: string; postalCode: string };
  identification?: { type: string; issuer: string; number: string;
                      issuedOn?: DateOnly; expiresOn?: DateOnly };
  employer?: { name: string; occupation?: string; businessAddress?: object };
}
interface AdvisorProfile {
  id: string; displayName: string; email?: string; firmName: string;
  signatureAssetId?: string; // encrypted asset; role-controlled, never inferred from field name
}
interface CaseRecord {
  id: string; clientId: string; advisorId: string; contentRevision: Revision;
  clientSnapshot: ClientProfile; adoptedProfileRevision: Revision;
  facts: {
    currentPlan?: { name?: string; sponsorName?: string; sponsorContact?: string };
    accountType?: string; product?: string;
    employment?: { sponsorRelationship?: string; retirementIntent?: string;
                   monthsUntilRetirement?: number; formerEmploymentDetails?: string[] };
    rolloverAttestation?: { employmentChoice?: string; departureDate?: DateOnly;
                            otherDescription?: string; recommendationDisclosure?: boolean };
    requestedDocuments?: Record<string, 'provided' | 'notProvided' | null>;
    investmentAlignment?: Record<string, 'high' | 'medium' | 'low' | null>;
    services?: Record<string, { need?: string; current?: string; newEmployer?: string; ira?: string }>;
    factors?: Record<string, { need?: string; bestAlignment?: string; description?: string }>;
    recommendation?: string;
    feeComparison?: { basis?: 'actual' | 'benchmark'; cells: Record<string, FeeCell> };
    financialSnapshot?: object; beneficiaries?: object[];
  };
  documents: DocumentInstance[];
  verification?: VerificationRecord; approval?: ApprovalRecord;
  finalizedPackageIds: string[]; workflowState: string;
}
interface FeeCell { amountText: string; unit: 'percentPerYear' | 'currencyPerYear' | 'rangeText'; }
interface DocumentInstance {
  id: string; templateId: string; templateVersion: string; templateSha256: string;
  mappingVersion: string; selected: boolean;
  answers: Record<string, Answer>; // document-only semantic answers
  overrides: Record<string, DocumentOverride>; // key is binding/group ID, never global field name
}
interface DocumentOverride {
  value: Answer; actorId: string; updatedAt: string; sourceRevisionAtEdit: Revision;
  kind: 'scalar' | 'choiceGroup'; // presence distinguishes override from absence
}
interface AuditEvent {
  id: string; sequence: number; timestamp: string; actorId?: string;
  eventType: string; caseId?: string; documentId?: string; bindingId?: string;
  contentRevision?: Revision; outcome: 'success' | 'failure'; errorCode?: string;
  // No old/new values, free-text payloads, client names, recipient, PDF bytes or filenames with PII.
}
interface VerificationRecord {
  id: string; contentRevision: Revision; rulesVersion: string; packageFingerprint: string;
  checkedAt: string; issues: ValidationIssue[]; acknowledgements: WarningAcknowledgement[];
}
interface ApprovalRecord {
  id: string; actorId: string; at: string; contentRevision: Revision;
  verificationId: string; packageFingerprint: string;
}
interface FinalizedArtifact {
  id: string; caseId: string; documentId: string; contentRevision: Revision;
  audience: 'internal' | 'client'; sourcePages: number[]; sha256: string;
  encryptedBlobId: string; exportPolicyVersion: string; createdAt: string;
}
interface FinalizedPackage {
  id: string; caseId: string; contentRevision: Revision;
  sourceSnapshotBlobId: string; // immutable encrypted client/case/document effective state
  verification: VerificationRecord; approval: ApprovalRecord;
  artifacts: FinalizedArtifact[]; packageFingerprint: string; createdAt: string;
  // Includes warning acknowledgements and pinned template/mapping/export versions.
}
interface EmailDraftRequest {
  requestId: string; caseId: string; approvedRevision: Revision;
  artifactIds: string[]; to: string; subject: string; body: string;
}
```

`ValidationIssue` and `WarningAcknowledgement` are defined by the [validation contract](2026-09-14-validation-acceptance.md). Numeric answers must be finite and within configured bounds; integer fields such as retirement months use numbers consistently in sources and overrides. Money remains decimal strings or integer minor units with explicit currency. `TemplateDefinition` owns display title, real/sample flag, bundled resource ID, hash, page metadata, mapping version and export policy. No absolute file paths are accepted from the renderer.

### Reusable versus case ownership

- Name, contact, identity and employer profile are reusable. They are still subject to confirmation over time.
- Sponsor of an old retirement plan can differ from the current employer. Store sponsor, plan, sponsor contact and transaction employment answers in the case.
- Financial circumstances, suitability preferences, requested documents, fee comparisons, recommendation and product selection are captured for this case. Do not silently reuse prior advice or attestation answers.
- Beneficiaries are transaction designations for the sample product, not necessarily a person's universal beneficiaries.
- Client full name is a derived value, not another editable master field. Store name components and format them consistently. Document-specific full-name spelling is an override.

### Reuse and history semantics

On case creation, copy the chosen reusable profile into `clientSnapshot`. For the active case, a left-side profile edit updates both that snapshot and the reusable profile in the same repository transaction; it immediately drives every selected document except overridden bindings. Other cases retain their snapshots, preventing historical drift.

On opening an older case with a newer profile, show "Newer client information available" with an explicit review/apply action. Applying selected changes updates the snapshot and invalidates approval; it does not clear overrides. A finalized case opens read-only; refresh/edit creates a new revision. This is one controlled source per active case, not a second competing editable field system.

### Repository contracts

Logical repositories expose `get`, `listSummaries`, and `save(expectedRevision)`; case mutations and audit writes run inside `VaultRepository.transaction`. `ClientRepository`, `CaseRepository`, `AdvisorRepository`, `AuditRepository`, and `ArtifactRepository` are interfaces over the same small encrypted vault initially. No generic ORM or database server.

Persist one authenticated workspace snapshot (profiles, cases, advisor, encrypted audit events and references) per atomic vault update; use separate encrypted PDF blobs and commit their references only after write/verification. Finalization stores an immutable `FinalizedPackage` containing its exact source snapshot, verification, warning acknowledgements, approval and manifest before the current case can advance. Later edits never replace those records; they create a new working revision and later a new finalized package. Single-instance app plus serialized updates avoids multiple writers. Failed saves leave the last valid disk revision recoverable and prevent approval/export.

## G. PDF mapping design

### Proposed schema contract

| Object / property | Meaning and validation |
|---|---|
| `schemaVersion`, `templateId`, `templateVersion`, `templateSha256`, `mappingVersion` | Stable schema and exact original compatibility; reject mismatch |
| `initialization` | Explicit new-case clearing policy; preserve original bytes; never reuse original factual `/V` values automatically |
| `bindings[].id`, `label`, `kind`, `section` | Stable semantic binding and UI location; IDs unique within template |
| `source.scope` | `client`, `case`, `advisor`, `document`, or `downstream`; cannot overlap ambiguously |
| `source.path` | Allowlisted typed semantic path; resolved client scope is active-case snapshot |
| `source.derived` | Named registered transform such as `fullName`; never evaluated JavaScript |
| `targets[]` | Exact field name, expected PDF type, physical page and optional rectangle tolerance; a logical binding may have multiple outputs |
| `kind` | `text`, `integer`, `date`, `currency`, `percent`, `boolean`, `enum`, `enumSet`, `signature`, or explicitly typed fee cell |
| `options[]` for choices | Semantic value and member field, on/off export values; no inference from checkbox order |
| `default` | Explicit semantic default; generally `null` for choices and no factual attestation defaults |
| `required`, `requiredWhen`, `validation` | Rules with stage, severity, IDs and plain-language issue messages |
| `visibleWhen`, `enabledWhen`, `applicableWhen` | Limited typed predicate AST; all/any/not, equals/in, present; no code strings |
| `inactivePolicy` | How an inapplicable answer is displayed/exported; Stage 1 projects blank and preserves retained data with a visible review notice |
| `format`, `transform` | Registered date/name/decimal formatting; no localization ambiguity or silent truncation |
| `overridePolicy` | Allowed for preparation bindings; direct PDF edits affect only this document |
| `signatureRole`, `completionStage` | Explicit client/advisor/consent semantics; Stage 1 downstream roles cannot be signed in app |
| `export` | Internal/client page selections, field treatment and policy version |
| `bindings[].sensitivity` | `identifier`, `personal`, or `ordinary`; drives SecureField masking/copy policy, not encryption scope (all case values remain encrypted) |
| `bindings[].audience`, `pages[].audience` | `clientVisible` or `internalOnly`; page list covers every physical page, including non-widget text. Intake page 7 is internal-only |

Presentation metadata is reviewed with the mapping version. Unknown sensitivity defaults to restricted copying; unknown page/binding audience is withheld during Presentation Mode until classified. Shared left-side source controls adopt the most restrictive sensitivity/audience among their selected bindings. A rule's issue details inherit the most restrictive audience of its targets/dependencies; document/package failures use a value-free public summary and retain internal detail privately. View filtering never changes applicability, stored values, verification counts/guards or export policy. The [UX spec](2026-09-14-stage-1-ux.md#presentation-mode) defines navigation and [security](../SECURITY.md) defines the limitation. These additions belong to the complete M3 schema; JSON examples below remain illustrative subsets.

All template widgets must match exactly one binding target or a declared downstream/ignored disposition. Unknown fields, unexpected duplicate names, wrong types and invalid export states fail template validation. Dynamic form UI is generated from selected binding/source definitions, deduplicated by source path; never from field names alone.

The initial predicate/transform registry is deliberately small. Unsupported predicates, paths or transforms fail fast. Derived dependency cycles are rejected. New PDFs using existing types need config and tests; truly new widget types or domain concepts require an explicit capability change.

### Realistic example: rollover

Illustrative subset of the proposed JSON schema, using exact discovered names. Omitted fields still need full mapping in M3.

```json
{
  "schemaVersion": 1,
  "templateId": "tws-rollover-attestation",
  "templateVersion": "2022-10-31",
  "templateSha256": "1cf50ec1d712c6063b7cc212689937dedfb724573312ef7a1719a68c4ad03534",
  "mappingVersion": "1.0.0-demo",
  "initialization": { "clearText": true, "clearChoices": true, "clearUnsignedSignaturePlaceholders": true },
  "bindings": [
    {
      "id": "investor-name", "label": "Investor name", "kind": "text", "section": "client",
      "source": { "scope": "client", "derived": "fullName" },
      "targets": [{ "fieldName": "text_01_es_:prefill", "expectedType": "Tx", "page": 2 }],
      "required": { "stage": "preparation", "severity": "error" },
      "overridePolicy": "document"
    },
    {
      "id": "plan-name", "label": "Current plan", "kind": "text", "section": "existing-plan",
      "source": { "scope": "case", "path": "facts.currentPlan.name" },
      "targets": [{ "fieldName": "text_03_es_:prefill", "expectedType": "Tx", "page": 2 }],
      "overridePolicy": "document"
    },
    {
      "id": "employment-attestation", "label": "Employment status for this rollover", "kind": "enum",
      "source": { "scope": "case", "path": "facts.rolloverAttestation.employmentChoice" },
      "cardinality": { "min": 0, "max": 1 }, "default": null,
      "required": { "stage": "preparation", "severity": "error" },
      "options": [
        { "value": "employedEligible", "fieldName": "CheckBox_01_es_:prefill", "page": 2, "on": "1", "off": "Off" },
        { "value": "departing", "fieldName": "CheckBox_02_es_:prefill", "page": 2, "on": "1", "off": "Off" },
        { "value": "other", "fieldName": "CheckBox_03_es_:prefill", "page": 2, "on": "1", "off": "Off" }
      ],
      "overridePolicy": "document"
    },
    {
      "id": "departure-date", "label": "Departure date", "kind": "date",
      "source": { "scope": "case", "path": "facts.rolloverAttestation.departureDate" },
      "targets": [{ "fieldName": "Date_04_af_date_es_:prefill", "expectedType": "Tx", "page": 2 }],
      "applicableWhen": { "op": "equals", "binding": "employment-attestation", "value": "departing" },
      "requiredWhen": { "op": "equals", "binding": "employment-attestation", "value": "departing" },
      "inactivePolicy": "blankProjectionPreserveAnswer", "format": "MM/dd/yyyy",
      "validation": [{ "id": "valid-departure-date", "rule": "calendarDate", "severity": "error" }],
      "overridePolicy": "document"
    },
    {
      "id": "investor-signature", "label": "Client signature - completed later", "kind": "signature",
      "source": { "scope": "downstream" }, "signatureRole": "client", "completionStage": "clientSigning",
      "targets": [{ "fieldName": "Signature16_es_:signer:signature", "expectedType": "Tx", "page": 3 }],
      "overridePolicy": "disabled"
    }
  ],
  "export": { "policyVersion": "1-demo", "internalPages": [1, 2, 3], "clientPages": [1, 2, 3], "preparationFields": "readOnly", "downstreamFields": "preserveBlank" }
}
```

JSON `on: "1"` represents the PDF name `/1`; `off: "Off"` represents `/Off`. The serializer must encode PDF names, not store these strings in text fields. Physical page metadata is an assertion/navigation hint checked against actual widgets, not a substitute for discovery.

### Realistic example: intake subset and a matrix cell

```json
{
  "schemaVersion": 1,
  "templateId": "tws-intake-recommendation",
  "templateVersion": "2022-10-31",
  "templateSha256": "2033f25c00beee6c5c152507b3a0ce45df826c0507b3b7cf1e81afee3e0cb6e3",
  "mappingVersion": "1.0.0-demo",
  "initialization": { "clearText": true, "clearChoices": true, "clearUnsignedSignaturePlaceholders": true },
  "bindings": [
    { "id": "investor-name", "label": "Investor name", "kind": "text", "section": "client",
      "source": { "scope": "client", "derived": "fullName" },
      "targets": [{ "fieldName": "text_01", "expectedType": "Tx", "page": 2 }], "overridePolicy": "document" },
    { "id": "plan-name", "label": "Current plan", "kind": "text", "section": "existing-plan",
      "source": { "scope": "case", "path": "facts.currentPlan.name" },
      "targets": [{ "fieldName": "text_04", "expectedType": "Tx", "page": 2 }], "overridePolicy": "document" },
    { "id": "quarterly-statements", "label": "Quarterly statements supplied", "kind": "enum", "default": null,
      "source": { "scope": "case", "path": "facts.requestedDocuments.quarterlyStatements" },
      "required": { "stage": "preparation", "severity": "error" }, "cardinality": { "min": 0, "max": 1 },
      "options": [
        { "value": "provided", "fieldName": "CheckBox_17", "page": 2, "on": "1", "off": "Off" },
        { "value": "notProvided", "fieldName": "CheckBox_18", "page": 2, "on": "1", "off": "Off" }
      ], "overridePolicy": "document" },
    { "id": "monitoring-current", "label": "Current plan offers ongoing monitoring", "kind": "enum", "default": null,
      "source": { "scope": "case", "path": "facts.services.monitoring.current" },
      "cardinality": { "min": 0, "max": 1 },
      "options": [
        { "value": "yes", "fieldName": "CheckBox_36", "page": 3, "on": "1", "off": "Off" },
        { "value": "no", "fieldName": "CheckBox_37", "page": 3, "on": "1", "off": "Off" }
      ], "overridePolicy": "document" },
    { "id": "investor-signature", "label": "Client signature - completed later", "kind": "signature",
      "source": { "scope": "downstream" }, "signatureRole": "client", "completionStage": "clientSigning",
      "targets": [{ "fieldName": "Signature1", "expectedType": "Sig", "page": 5 }], "overridePolicy": "disabled" }
  ],
  "export": { "policyVersion": "1-demo", "internalPages": [1, 2, 3, 4, 5, 6, 7], "clientPages": [1, 2, 3, 4, 5, 6], "preparationFields": "readOnly", "downstreamFields": "preserveBlank" }
}
```

Full mapping adds required rules, N/A handling, every matrix row, recommendation choices, fee units and all signature/date dispositions. `text_06`/`text_07` are two lines of one Other-factor description; use an explicit line split with combined overflow checks, not two unrelated client fields. The 12 fee cells support typed units/ranges; do not automatically sum percentages and dollar amounts or compare unlike units.

## H. Live synchronization and overrides

### Exact resolution order

1. Enforce role policy: downstream signature/date fields remain empty and cannot be written by advisor commands.
2. Resolve the semantic base: mapped client/case/advisor answer or document-only answer, if present.
3. If the binding has an explicit document override, use it instead of the base, including `null` (intentional blank), `false`, `0` and empty string.
4. If neither source nor override is present, use the reviewed mapping default, then the normalized template default/blank. Raw template selections are not approved defaults.
5. Evaluate document-local applicability from effective controlling bindings; format and project to target widgets. Inapplicable fields project blank while retaining their prior answer/override. Retained values are visibly indicated; configuration changes do not silently delete them.

Thus the value precedence is `present override > present mapped answer > reviewed default > blank`. Presence is a property/key check, never a truthiness check. Applicability/role policy controls whether that value can be emitted. A document override of the employment choice changes that document's conditional date requirements, not other documents' requirements.

### Mutation contract

- A left-panel master/case edit issues a typed command with expected content revision and unique command ID. The main reducer validates ownership/type, commits state and audit atomically, and returns the accepted revision plus effective-field patches.
- Direct PDF input dispatches `setDocumentOverride(documentId, bindingId, value)`. It never dispatches a profile patch. Unmapped-but-declared document-only fields update `answers` instead of a meaningless override.
- Every exclusive group is stored as one nullable enum override. Clicking Yes sets Yes and clears No in the same reducer transition. Later shared-source edits cannot partially overwrite another member. Reset group clears the group override as a whole.
- Programmatic widget updates are tagged with origin/revision and suppressed from user-edit capture. Prevent echo loops and stale patches. Widgets on multiple pages must all reflect their semantic binding if a future template repeats a field.
- Text reflects optimistic local typing immediately. Send lightweight value commands/patches, not rewritten PDF bytes per keystroke. Save can debounce (target 300-500 ms) but explicit Verify/Approve/close waits for the latest durable acknowledgement. Failed IPC/save rolls back or visibly marks unsaved state; never silently exports the optimistic version.
- A single active writer is sufficient. Reject stale expected revisions, resynchronize, and preserve the user's unaccepted input for retry. Coalesce sequential local edits without discarding their final value.
- Focus/scroll/zoom changes are view state, not material content; they do not invalidate approval. Values, overrides, document selection, template/mapping/export policy changes do.

### Scenarios

| Action | Master / case | PDF A | PDF B |
|---|---|---|---|
| Type client name Jordan Avery | Snapshot/profile update | Synced Jordan Avery | Synced Jordan Avery |
| Directly edit B name to Jordan A. Avery | Unchanged | Jordan Avery | Override Jordan A. Avery |
| Change master last name to Avery-Sample | Snapshot/profile update | Jordan Avery-Sample | Override remains Jordan A. Avery |
| Reset B name override | Unchanged | Same | Latest Jordan Avery-Sample |
| Directly clear B name | Unchanged | Same | Explicit blank override; required error |
| Change shared Yes/No answer after overriding B | Case updates | New case choice | Complete B choice override remains |
| Reset B choice group | Unchanged | Same | Entire latest case choice restored |
| Deselect/reselect B | Case package revision changes | Unchanged | Same instance/overrides retained; excluded while deselected |
| Restart | Authenticated vault restored | Same effective value | Same override, source and revision |

The requested address example is identical: master 100 Main Street; sample PDF B override 102 Main Street; sample PDF A stays 100 Main Street. Neither real TWS template has an address field. Clearing an override restores the currently adopted active-case source; it does not automatically refresh from a newer profile belonging to another case.

### Performance targets (proposed, not measured)

On the target laptop with all six forms: visible typed value to PDF widget update <=150 ms at p95; tab activation to current values <=500 ms once loaded; initial package usable <=3 seconds; Verify <=2 seconds for configured rules excluding export. Measure on packaged build with synthetic data. Rendering virtualizes pages and retains active controls; never rebuild the whole document on each keystroke.

## Alternatives, risks and rollout

Hardcoded React field wiring and PDF-byte-owned state are rejected because they cannot reliably preserve semantic overrides or old cases. Main-authoritative state adds a small acknowledgement layer but makes approval enforceable. The principal open risk is live PDF.js widget updates; M0 chooses the adapter, M2 proves reducer invariants, M3 maps all fields, and M4-M5 prove interaction. See [acceptance](2026-09-14-validation-acceptance.md).
