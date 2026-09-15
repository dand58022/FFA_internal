---
node_id: design-stage-1-product
type: design
title: Stage 1 advisor document preparation prototype
created: 2026-09-14
updated: 2026-09-15
status: draft
category: cross-domain
tags: [product, frontend, backend]
author: Codex
reviewers: [project-owner, advisor-domain-reviewer, technical-reviewer]
summary: Defines the smallest complete local advisor meeting demonstration and its future boundaries.
---

# Stage 1 product specification

**Review status:** proposed; reviewers are roles awaiting assignment. No coding approved.

## Load-bearing decisions

- [Desktop and PDF stack](../decisions/2026-09-14-desktop-pdf-stack.md).
- [Canonical state and approval](../decisions/2026-09-14-state-and-approval.md).
- [Local storage and handoff](../decisions/2026-09-14-local-storage-and-handoff.md).

## A. Executive summary

Financial advisors repeatedly enter the same facts across forms while speaking with a client. Repetition interrupts the meeting and creates avoidable preparation errors that can contribute to Not In Good Order (NIGO) delays.

Stage 1 demonstrates one controlled client/case record driving a configurable package of interactive PDFs. An advisor can enter shared information, see the actual forms update, make an intentional exception on one form, find omissions, approve a specific revision, and open a prepared Classic Outlook draft. The app never sends it.

The VP should see a coherent meeting experience: enter once, preserve exceptions, catch missing answers, and finish with the actual documents attached. The strongest proof is the entire controlled workflow, not the number of fields or a claim that the application makes financial recommendations.

### Goals and success

- Complete the scripted demonstration in 3-5 minutes on a Windows laptop, without an application internet connection.
- Make all six document cards functional; two are the supplied TWS templates and four are visibly labeled sample forms.
- Prove shared data, direct editing, overrides, rule-based verification, approval, final PDFs, and a displayed unsent email draft.
- Keep client/case/audit persistence encrypted and templates unchanged.
- Make a seventh similar form a template/mapping/field-definition/test change rather than a new PDF engine.

## C. Recommended Stage 1 scope

### In scope

| Capability | Bounded Stage 1 behavior |
|---|---|
| Platform | Windows desktop, one local advisor login, one active app instance/case editor |
| Document selection | Six cards; any nonempty combination, deterministic ordering, selected count |
| Real forms | All pages available in private review; Presentation Mode withholds internal pages. Inventory-backed mappings for all non-signature widgets, with case/document-only ownership where appropriate |
| Sample forms | Four simple, polished, fillable PDFs using the same mapping/view/export engine; clear Sample branding |
| Workspace | Resizable data/PDF split, keyboard navigation, section completion, document tabs, scroll and zoom |
| Visual experience | Client-facing navy/light shell, semantic design system, locally owned components and Presentation Mode; see the [visual spec](2026-09-15-visual-design-system.md) |
| Reuse | Client profile plus active-case snapshot; shared name, sponsor, plan; sample documents also show address and other demographics |
| Direct PDF editing | Text and checkboxes; configured choice groups; document overrides and reset action |
| Persistence | Encrypted local JSON vault, reopen saved demo clients/cases, minimal local login |
| Verification | All selected documents checked against explicit preparation rules; issue navigation; red blockers; warning acknowledgements |
| Approval | Application-level advisor approval tied to exact verified package revision |
| Finalization | One PDF per selected document plus encrypted manifest; internal and client-facing variants where configured |
| Email | Main-process-controlled Classic Outlook draft with recipient/subject/body/attachments, never Send; local export fallback |
| Activity | Encrypted value-free audit events and understandable save/error status |
| Delivery | Windows packaged application and rehearsed offline demo on the target laptop |

"All widgets mapped" means each has a declared disposition: reusable/client-derived, case-derived, document-only, or downstream client signing. It does not mean every widget appears as a separate left-panel question or has a mandatory answer.

### Out of scope / non-goals

Cloud databases, telemetry, analytics, remote logs, cloud PDF/OCR/AI, licensing, subscriptions, SSO, enterprise users/roles, carrier website automation, automatic package recommendations, uploaded supporting documents, OCR, client signature capture, Adobe e-signing, financial suitability decisions, legal/compliance certification, automatic sending, arbitrary PDF import, arbitrary page editing, unrestricted configuration editors, PDF merging into one file, enterprise MSI/update infrastructure, or production deployment with real client data.

### Six-document catalog

| Card | Content to demonstrate | Selected fields / purpose |
|---|---|---|
| TWS Rollover Attestation | Original three-page form | Shared name/sponsor/plan; employment choice; conditional date; client signature stays empty |
| TWS Intake / Recommendation | Original seven-page form | Shared identity/plan; requested-document pairs; service/factor matrices; recommendation; fee comparison |
| Suitability Report - Sample | One page | Name, address, case financial snapshot, objective/risk preference; no suitability verdict |
| Product Application - Sample | One page | Name, address, requested account/product, amount, one beneficiary and allocation |
| Client Profile - Sample | One page | Name, address, email, DOB, masked-entry synthetic identifier, employment/ID examples |
| Disclosure / Authorization - Sample | One page | Name, contact details, document-preparation acknowledgement; blank downstream signature area |

Four sample PDFs are generated only during implementation. They use unique template IDs, exact field names, normal versioned mappings, and a printed "Sample - not for submission" footer. Do not imply carrier/firm approval. A sample acknowledgement must not masquerade as legal electronic consent.

### Evidence-driven changes to the initial idea

1. The real forms lack address/SSN fields. Use names and plan facts for the two-real-form demonstration, and sample forms for address reuse.
2. Avoid separate "Verify & Approve" and "Advisor Sign-off" confirmations. Use Verify Documents, then one intentional Approve Package action after review.
3. Start the viewer on the first data page (page 2 of each real form), while preserving page 1 consent and its navigation entry.
4. Do not let blank-looking original fields imply default answers. Initialize working copies from reviewed configuration.
5. Intake page 7 is internal. Proposed client export contains pages 1-6; internal archive retains 1-7. Preserve page order and log the export policy version.
6. Green means "Preparation checks passed; client signatures pending." It must never imply that client signing or compliance review is complete.

## Proposed approach

Use Electron + React + TypeScript, PDF.js for page/interactive widget display, pdf-lib for main-owned output generation, and a small revisioned domain reducer. See [architecture](../ARCHITECTURE.md). Implement the riskiest PDF and Outlook assumptions early, then polish a working end-to-end path.

## Alternatives considered

- **Recommended: full workflow with narrow rules and six working templates.** Delivers a convincing, honest demo and reusable seams; PDF integration needs a feasibility gate.
- **Static preview with left-only entry.** Fast, but fails the required direct PDF editing and override proof; rejected.
- **Production platform first.** Tenant management, carrier integrations, and commercial PDF SDK procurement may fit later; delays validation of the core meeting workflow; rejected for Stage 1.

## Rollout

Follow [milestones M0-M9](../plans/active/2026-09-14-stage-1-demo.md). Only fictional data is permitted. No production deployment is implied by completing the demo. When decisions change, update the owning spec/ADR and acceptance criteria before implementing the change.

## R. Roadmap

| Stage | Purpose | Candidate work and gates |
|---|---|---|
| Stage 2: firm pilot | Validate real advisor workflow with a firm-approved data policy | Reviewed/current form library; firm-owned required/conditional rules; supporting attachments (ID, OFAC report, illustrations, carrier files); recovery/retention; encrypted database if scale demands it; deployment/signing and security review before real PII |
| Stage 2 research | Discover external workflow contracts | Identify carrier APIs/vendors, approved authentication, supported automation and session limits; research Adobe/e-signature consent, identity, audit and evidence requirements separately |
| Stage 3: commercialization | Multi-firm product and support | Firm branding/mappings, version distribution/migrations, roles, seats/devices, licensing, enterprise MSI, code signing/reputation, secure updates, PII-free diagnostics, reviewed audit/retention and recovery controls |

Future `ExternalApplicationAdapter` is an interface candidate, not an empty Stage 1 framework: approved target, explicit field translation, authenticated session ownership, timeouts, and user-controlled completion. Prefer vendor APIs/approved paths; do not circumvent controls or retain credentials unnecessarily. Reusing PII on an external site changes the local-only boundary and requires an explicit later policy decision.

Future package rules can recommend forms from product/account/rollover choices only once specifications exist. Attachments and fillable forms should then be distinct package item types.

Future licensing exchanges only entitlement/license ID, app version, and a minimized device identifier. No client identifiers, client names, DOB/SSN, case answers, documents, or PDF hashes/content are sent. Keep licensing networking separate from repositories/PDF services. No licensing endpoint or code in Stage 1.

## Risks and open questions

The ranked [risk register](../references/risks-and-assumptions.md) owns unresolved matters, proposed defaults, and decision gates. No unanswered production question blocks drafting this plan.

## T. Final recommendation

Build one local Windows meeting workspace, six functioning templates, a single active-case state model, configuration-driven mappings and choice groups, encrypted persistence, and one controlled path to a Classic Outlook draft. Keep the PDF interaction adapter, storage interfaces, and template versions durable; defer enterprise machinery. Begin implementation with PDF feasibility on copies, including the intake matrices and original appearance anomalies, before investing in presentation polish.
