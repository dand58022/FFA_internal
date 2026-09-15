---
node_id: adr-2026-09-14-local-storage-and-handoff
type: adr
title: Local encrypted state and controlled document handoff
created: 2026-09-14
updated: 2026-09-14
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

## Options considered

- Plain JSON plus a login: simple but fails sensitive-data persistence requirements.
- Encrypted SQLite immediately: reasonable future direction but adds database/key integration without helping the small demonstration.
- Encrypted JSON vault plus repository interfaces: straightforward authenticated encryption and atomic updates for one advisor; limited scale.

For Outlook, a fixed PowerShell COM helper is smaller than a custom native bridge; native C# becomes a fallback if the target firm's script policy blocks it. Graph/web add-ins introduce online authentication and cloud dependencies that conflict with this demo.

## Decision

Choose one AES-256-GCM encrypted JSON vault for the small dataset, plus encrypted finalized artifact blobs, under `%LOCALAPPDATA%/FAAClientIntake/Demo/`. Wrap a random data key with Windows-backed Electron safeStorage. Keep repository APIs independent of file layout. Use a fixed, packaged PowerShell helper from main with structured input to create/save/display a Classic Outlook mail item. Do not include a send operation.

Default finalization retains editable signature fields and marks preparation fields read-only in the client copy; no full flattening. Archive the exact internal revision encrypted. Omit intake page 7 from the proposed client variant while retaining all original pages internally. These output policies need confirmation before real use.

## Consequences

DPAPI does not protect against other malicious processes running as the same Windows user; demo login is only a gate. Outlook and explicitly exported PDFs are outside application encryption. Plaintext attachment staging must have a controlled lifecycle; deletion is best effort, not secure erasure. Recoverability and enterprise controls need production review.

## Follow-ups

- [ ] M0/M7: prove the client copy keeps values/appearances and downstream blank fields; verify page selection.
- [ ] M1/M8: prove Classic Outlook availability and enterprise policy on the actual laptop.
- [ ] Before real PII: firm approves client page set, email delivery, retention and signing approach.

## References

[Security](../SECURITY.md), [Outlook design](../ARCHITECTURE.md#outlook-service), [risks](../references/risks-and-assumptions.md).
