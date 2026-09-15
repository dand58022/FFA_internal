---
node_id: ref-template-classification-registry
type: reference
title: Template classification and registry
created: 2026-09-15
updated: 2026-09-15
status: draft
category: reference
tags: [pdf, templates, roadmap]
summary: How a real form library is classified by PDF structure, what each class needs from the engine, and the registry fields required before Stage 2.
---

# Template classification and registry

## Why this exists now

The Stage 1 engine is built for the two supplied forms, which are AcroForms with text fields and independent checkboxes. A pilot library will not be uniform. Classifying it early decides how much of the current engine transfers and which forms cannot be promised. The registry replaces the Stage 1 assumption that six bundled templates with fixed hashes are the whole world.

## Classification

| Class | How to recognize it | Engine implication |
|---|---|---|
| AcroForm, text and checkbox only | `/AcroForm` present, widgets of type `/Tx` and `/Btn` without radio parents, no XFA | Supported by the Stage 1 mapping, display adapter and output path |
| AcroForm with radio groups, choice fields, multi-widget fields or `/Sig` beyond signature placeholders | Radio parents with `/Kids`, `/Ch` fields, one field with several widgets | Mapping schema needs the corresponding kinds; display adapter needs the corresponding controls; output path must write parent/kid states correctly |
| Flat PDF | No `/AcroForm` or empty field tree; boxes are printed graphics | Needs a separate coordinate placement path: reviewed rectangles per field, text drawn at those rectangles, no interactive widgets to reuse. Mapping targets become rectangles, not field names |
| XFA | `/AcroForm` carries `/XFA`; fields are defined in embedded XML | Not supported by the planned libraries or PDF.js form rendering. The form owner must supply an AcroForm or flat equivalent, or the form is out of scope |
| eSignature-native | The custodian or carrier distributes the form as an eSignature template or API rather than a fillable PDF | PDF fill is not the interface. Out of scope for the PDF engine; a future adapter would target the provider's template fields |

Classification is a read-only inspection like the one recorded in [PDF findings](pdf-findings.md): field tree, widget types, page geometry, XFA presence, encryption and hash. It takes minutes per form and should run on the entire candidate library before any Stage 2 scope commitment.

## Registry fields

Every template the application will fill needs one registry entry. Stage 1 keeps this in the bundled catalog; Stage 2 needs it as a maintained record.

| Field | Meaning |
|---|---|
| `templateId` | Stable opaque identifier; never the filename |
| `title`, `issuer` | Display name and the custodian, carrier or firm that owns the form |
| `class` | One of the classes above |
| `templateVersion`, `templateSha256` | The exact revision the mapping was written against; hash mismatch fails validation |
| `effectiveFrom`, `retiredOn` | Dates the issuer accepts this revision; an outdated form version is a leading cause of Not In Good Order returns |
| `mappingVersion`, `rulesVersion`, `exportPolicyVersion` | The reviewed configuration bound to this template revision |
| `owner`, `reviewedOn` | Who at the firm approved the mapping and requiredness and when |
| `supersedes` | Previous template revision, so saved cases can be migrated or flagged |
| `signingRequirements` | Wet signature, eSignature, notarization or Medallion guarantee; the application never satisfies these, only records them |

A registry entry with a missing or expired `effectiveFrom`/`retiredOn` window blocks the template from selection rather than silently filling a stale form.

## Attachments as a package item type

Fillable forms are one item type. Supporting documents such as identification copies, statements, OFAC reports, illustrations and carrier files are another. They are not filled, but their absence returns a package just as surely as a blank field. Stage 2 verification needs a per-package list of required attachments, a presence check per item, and the same audience classification as pages, so an internal-only attachment is never staged for a client email. Stage 1 does not implement this; the roadmap in the [product spec](../specs/2026-09-14-stage-1-product.md#r-roadmap) records it as a Stage 2 requirement.

## Stage 1 status

Both supplied TWS forms are AcroForm, text and checkbox only. The four sample forms will be generated in the same class. No flat, XFA or eSignature-native template is in Stage 1 scope, and the demo must not imply support for them.
