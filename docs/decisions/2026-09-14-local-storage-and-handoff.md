---
node_id: adr-2026-09-14-local-storage-and-handoff
type: adr
title: Local encrypted state and controlled document handoff
created: 2026-09-14
updated: 2026-09-15
status: draft
adr_status: proposed
category: decision
tags: [security, outlook]
summary: Encrypted local JSON behind repositories, DPAPI-protected key, and explicit Outlook draft handoff.
---

# ADR: Local encrypted state and controlled handoff

## Status

Proposed. Firm approval of the eventual delivery/signing policy remains open.

## Context

The prototype must work offline and keep internal persistence encrypted. Sending PDFs through Outlook necessarily hands data to another application's storage and potentially its mailbox server, even before manual Send.

The COM path exists only in Classic Outlook. New Windows and Microsoft 365 installations increasingly default to New Outlook, which has no Object Model, so the presence of Classic Outlook on the demo laptop and on pilot machines is an assumption that must be checked first, not last.

## Options considered

- Plain JSON plus a login: simple but fails sensitive-data persistence requirements.
- Encrypted SQLite immediately: reasonable future direction but adds database/key integration without helping the small demonstration.
- Encrypted JSON vault plus repository interfaces: straightforward authenticated encryption and atomic updates for one advisor; limited scale.

For Outlook, a fixed PowerShell COM helper is smaller than a custom native bridge; native C# becomes a fallback if the target firm's script policy blocks it.

Microsoft Graph draft creation is the documented alternative: create a draft message in the signed-in mailbox and add file attachments through the API. It works with either Outlook variant and needs no local script policy, but it requires Entra sign-in, an application registration approved by the firm, and online access, and it changes the handoff boundary from a local COM call to a cloud API call that must be reviewed under the same egress rules as the draft itself. It conflicts with the offline Stage 1 demo and is therefore recorded, not built.

## Decision

Choose one AES-256-GCM encrypted JSON vault for the small dataset, plus encrypted finalized artifact blobs, under `%LOCALAPPDATA%/FAAClientIntake/Demo/`. Wrap a random data key with Windows-backed Electron safeStorage. Keep repository APIs independent of file layout. Use a fixed, packaged PowerShell helper from main with structured input to create/save/display a Classic Outlook mail item. Do not include a send operation.

Default finalization retains editable signature fields and marks preparation fields read-only in the client copy; no full flattening. Archive the exact internal revision encrypted. Omit intake page 7 from the proposed client variant while retaining all original pages internally. These output policies need confirmation before real use.

## Consequences

DPAPI does not protect against other malicious processes running as the same Windows user; demo login is only a gate. Outlook and explicitly exported PDFs are outside application encryption. Plaintext attachment staging must have a controlled lifecycle; deletion is best effort, not secure erasure. Recoverability and enterprise controls need production review.

## Follow-ups

- [ ] M0/M7: prove the client copy keeps values/appearances and downstream blank fields; verify page selection.
- [ ] M1, first task: confirm which Outlook variant, profile state and script policy exist on the actual laptop before any shell work; if only New Outlook is present, decide between installing Classic Outlook for the demo and advancing the Graph alternative.
- [ ] M8: prove the chosen handoff on the actual laptop with a disposable fictional draft.
- [ ] Before real PII: firm approves client page set, email delivery, retention and signing approach.

## References

[Security](../SECURITY.md), [Outlook design](../ARCHITECTURE.md#outlook-service), [risks](../references/risks-and-assumptions.md).
