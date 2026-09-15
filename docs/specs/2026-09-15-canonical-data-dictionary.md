---
node_id: design-canonical-data-dictionary
type: design
title: Canonical data dictionary
created: 2026-09-15
updated: 2026-09-15
status: draft
category: cross-domain
tags: [backend, pdf, mapping]
author: Claude Code
reviewers: [project-owner, advisor-domain-reviewer, technical-reviewer]
summary: The single allowlist of semantic paths that mapping sources, left-panel controls, validation rules and provenance keys may reference.
---

# Canonical data dictionary

## Purpose and ownership

Every template mapping binds PDF widgets to a semantic path. Without one owned list of those paths, each new template invents its own names and the "enter once" promise decays into per-form fields. This document is that list. It is the hub; each template mapping is a spoke. The [data and mapping spec](2026-09-14-data-mapping-sync.md) owns the storage types and resolution order; this document owns the paths, their kinds, ownership scope, sensitivity and permitted values.

Rules:

- A mapping `source.path` must resolve to an entry here with a compatible `kind`. Template validation fails otherwise.
- Adding a path is a dictionary change, a storage schema change and a test change in the same commit. Renaming a path requires a migration note for saved cases.
- Enum values listed here are the stored values. Display labels and PDF export values live in the mapping, never here.
- Sensitivity follows the mapping schema: `identifier`, `personal`, `ordinary`. Left-panel controls that serve several bindings adopt the most restrictive.
- Scope is `client` (reusable profile, resolved through the active-case snapshot), `case` (this transaction), `advisor`, or `document` (document-only answers, keyed by binding ID and not listed here).
- Entries marked *pending M3* have a defined kind but values or row keys that the advisor-domain reviewer confirms during M3. Nothing here asserts firm policy.

## Client scope

| Path | Kind | Sensitivity | Values / format | Notes |
|---|---|---|---|---|
| `client.firstName` | text | personal | | Required for any name-bearing form |
| `client.middleName` | text | personal | | Optional |
| `client.lastName` | text | personal | | Required for any name-bearing form |
| `client.fullName` | text | personal | derived `fullName` transform | Read-only derivation; document-specific spelling is an override |
| `client.dob` | date | personal | `YYYY-MM-DD` | Sample forms only in Stage 1 |
| `client.ssn` | text | identifier | configured syntax only | Sample forms only; fixture uses invalid all-zero value |
| `client.taxId` | text | identifier | configured syntax only | Sample forms only |
| `client.maritalStatus` | enum | personal | *pending M3* | No real-form target in Stage 1 |
| `client.citizenship` | text | personal | | No real-form target in Stage 1 |
| `client.email` | text | personal | syntactic check | Also the default email-draft recipient |
| `client.phone` | text | personal | configured format | |
| `client.address.line1` | text | personal | | Sample forms only |
| `client.address.line2` | text | personal | | Optional |
| `client.address.city` | text | personal | | |
| `client.address.state` | text | personal | two-letter code | |
| `client.address.postalCode` | text | personal | configured format | |
| `client.identification.type` | enum | personal | *pending M3* | Sample profile form |
| `client.identification.issuer` | text | personal | | |
| `client.identification.number` | text | identifier | | Masked and copy-restricted |
| `client.identification.issuedOn` | date | personal | `YYYY-MM-DD` | |
| `client.identification.expiresOn` | date | personal | `YYYY-MM-DD` | |
| `client.employer.name` | text | ordinary | | Current employer; never assumed to be the plan sponsor |
| `client.employer.occupation` | text | ordinary | | |

## Case scope

| Path | Kind | Sensitivity | Values / format | Notes |
|---|---|---|---|---|
| `case.currentPlan.name` | text | ordinary | | Rollover `text_03_es_:prefill`, intake `text_04` |
| `case.currentPlan.sponsorName` | text | ordinary | | Rollover `text_02_es_:prefill`, intake `text_03` |
| `case.currentPlan.sponsorContact` | text | ordinary | | Rollover `text_04_es_:prefill` |
| `case.currentPlan.sponsorNotApplicable` | boolean | ordinary | | Intake `CheckBox_08`; conflicts with a nonblank sponsor |
| `case.currentPlan.planNotApplicable` | boolean | ordinary | | Intake `CheckBox_09`; conflicts with a nonblank plan |
| `case.accountType` | enum | ordinary | `401k`, `403bErisa`, `403bNonErisa`, `457`, `pensionDefinedBenefit`, `otherEmployerPlan`, `ira` | Intake `CheckBox_01`-`07`; one selection |
| `case.product` | text | ordinary | | Sample product application |
| `case.employment.sponsorRelationship` | enum | ordinary | `currentlyEmployed`, `noLongerEmployed` | Intake `CheckBox_10`, `13` |
| `case.employment.retirementIntent` | enum | ordinary | `retiringWithinMonths`, `notPlanningDeparture` | Intake `CheckBox_11`, `12`; applicable while currently employed |
| `case.employment.monthsUntilRetirement` | integer | ordinary | bounded, finite | Applicable when retiring within months |
| `case.employment.formerEmploymentDetails` | enumSet | ordinary | `retired`, `employedByNewEmployer`, `notCurrentlyEmployed` | Intake `CheckBox_14`-`16`; independently selectable unless firm supplies exclusivity, *pending M3* |
| `case.newEmployerPlanAvailable` | boolean | ordinary | | Controls applicability of new-employer columns |
| `case.rolloverAttestation.employmentChoice` | enum | ordinary | `employedEligible`, `departing`, `other` | Rollover `CheckBox_01`-`03_es_:prefill`; one selection |
| `case.rolloverAttestation.departureDate` | date | ordinary | `YYYY-MM-DD` | Applicable when `departing` |
| `case.rolloverAttestation.otherDescription` | text | ordinary | | Applicable when `other` |
| `case.rolloverAttestation.recommendationDisclosure` | boolean | ordinary | | Rollover `CheckBox_04_es_:prefill`; advisor-entered, never inferred |
| `case.requestedDocuments.quarterlyStatements` | enum | ordinary | `provided`, `notProvided` | Intake `CheckBox_17`, `18` |
| `case.requestedDocuments.feeDisclosure` | enum | ordinary | `provided`, `notProvided` | Intake `CheckBox_19`, `20` |
| `case.requestedDocuments.planDescription` | enum | ordinary | `provided`, `notProvided` | Intake `CheckBox_21`, `22` |
| `case.investmentAlignment.currentPlan` | enum | ordinary | `high`, `medium`, `low` | Intake `CheckBox_23`-`25` |
| `case.investmentAlignment.newEmployerPlan` | enum | ordinary | `high`, `medium`, `low` | Intake `CheckBox_26`-`28` |
| `case.investmentAlignment.ira` | enum | ordinary | `high`, `medium`, `low` | Intake `CheckBox_29`-`31` |
| `case.services.<row>.need` | enum | ordinary | `high`, `medium`, `low`, `none` | Rows `monitoring`, `allocation`, `advice`, `management`, `planning`; intake `CheckBox_32`-`35` with suffix |
| `case.services.<row>.current` | enum | ordinary | `yes`, `no` | Intake `CheckBox_36`, `37` with suffix |
| `case.services.<row>.newEmployer` | enum | ordinary | `yes`, `no` | Intake `CheckBox_38`, `39` with suffix |
| `case.services.<row>.ira` | enum | ordinary | `yes`, `no` | Intake `CheckBox_40`, `41` with suffix |
| `case.factors.<row>.need` | enum | ordinary | `high`, `medium`, `low`, `none` | Rows `tax`, `beneficiary`, `guarantees`, `distributions`, `control`, `consolidation`, `protection`, `severingEmployer`, `other`; intake `CheckBox_42`-`45` with suffix |
| `case.factors.<row>.bestAlignment` | enum | ordinary | `currentPlan`, `newEmployerPlan`, `ira` | Intake `CheckBox_46`-`48` with suffix; single selection *pending M3* |
| `case.factors.other.description` | text | ordinary | two printed lines | Intake `text_06` + `text_07`; explicit line split with overflow check |
| `case.recommendation` | enum | ordinary | six values *pending M3* | Intake `CheckBox_49`-`54`; one selection under the declared single-transaction rule |
| `case.feeComparison.basis` | enum | ordinary | `actual`, `benchmark` | Intake `CheckBox_55`, `56`; internal-only page |
| `case.feeComparison.cells.<row>.<column>` | feeCell | ordinary | `{ amountText, unit }` | Four rows by three columns, keys *pending M3*; intake `text_09`-`text_20`; never summed across units |
| `case.financialSnapshot.<field>` | currency / text | personal | decimal string or minor units with currency | Sample suitability form; fields *pending M3* |
| `case.beneficiaries[].name` | text | personal | | Sample product application; transaction designation only |
| `case.beneficiaries[].relationship` | text | personal | | |
| `case.beneficiaries[].allocationPercent` | percent | ordinary | integer 0-100; single-beneficiary sample totals 100 | |

## Advisor scope

| Path | Kind | Sensitivity | Values / format | Notes |
|---|---|---|---|---|
| `advisor.displayName` | text | ordinary | | Email body signature; never written to a client signature field |
| `advisor.email` | text | personal | | Not a mapping target in Stage 1 |
| `advisor.firmName` | text | ordinary | | |

`advisor.signatureAssetId` is an encrypted asset reference, not a mappable value, and has no dictionary path.

## Document scope

Document-only answers are keyed by binding ID inside the document instance and are not enumerated here. Stage 1 examples: the Disclosure sample acknowledgement and any template-specific question with no reuse across forms. A document-only binding that later proves reusable is promoted by adding a path above and migrating the answer; it is never silently shared by matching field names.

## Downstream

Client signature, consent and signing-date fields have `source.scope: downstream` and no path. They are never written by the application.
