---
node_id: security-faa-local
type: domain
title: Local security and document handoff design
created: 2026-09-14
updated: 2026-09-14
last-useful: 2026-12-14
status: draft
category: security
tags: [security, local-storage]
summary: Prototype controls, cryptographic persistence, Electron boundaries, plaintext exports and production gaps.
---

# J. Security design

## Threat assumptions and scope

Stage 1 uses only fictional data on a trusted Windows advisor demo account. Controls protect against casual app access, accidental plaintext storage/logging, offline copying of data files without the Windows key context, and routine renderer misuse. They do not defend against an administrator, malware in the same Windows user session, screen capture, memory scraping, a compromised OS, or authorized recipients forwarding PDFs.

A local password gate is not the encryption root or production identity system. DPAPI-backed safeStorage protects key material in the Windows user context; other applications running as that user are outside that protection boundary. This limitation is explicit in [Electron safeStorage documentation](https://www.electronjs.org/docs/latest/api/safe-storage).

## Storage layout and encryption

Main resolves a Windows local application-data directory, e.g. `%LOCALAPPDATA%/FAAClientIntake/Demo/`, with app-owned child directories. Do not assume Electron's default `userData` is LocalAppData; explicitly configure appropriate userData/session paths before creating windows. Separate Chromium metadata from `vault/`, `artifacts/`, and controlled `staging/`. [Electron app paths](https://www.electronjs.org/docs/latest/api/app) document these distinct locations.

- Generate a cryptographically random 32-byte data-encryption key. Wrap its encoded bytes with Windows-backed Electron safeStorage; write only the wrapped key envelope. Prefer the supported asynchronous API in the pinned Electron version when available; fail closed if OS key protection is unavailable.
- Encrypt workspace JSON and each artifact blob with AES-256-GCM, a fresh random 12-byte nonce on every encryption, and a full 16-byte authentication tag. Never reuse a nonce with the same key. Use authenticated additional data containing schema version, record/blob ID and key version to bind identity.
- Store envelope version, algorithm, key ID, nonce, ciphertext and tag. Authenticate before parsing JSON. Never fall back to plaintext on errors. Validate sizes/schema after decrypting. Node's [crypto API](https://nodejs.org/api/crypto.html) provides authenticated ciphers and password derivation primitives.
- Key wrapping is separate from the data file; only ciphertext key material may sit in the app directory. No hardcoded keys, raw adjacent key files, renderer keys, `.env` secrets or key logging.
- Use opaque filenames/IDs. Client names, SSNs, DOB and email addresses must not appear in paths, temp names, logs or process arguments.
- A small single encrypted vault allows profile + active-case snapshot + audit update to commit together. Write ciphertext to a same-volume temporary file, flush it, atomically replace the current file, retain one previous authenticated ciphertext backup. Serialize writes and enforce a single app instance. Failed replacement preserves last good data and exposes Save failed.
- Artifact generation writes encrypted blobs first, then atomically records the complete verified manifest. A failed batch never reports a partially completed package as finalized; orphan encrypted blobs can be reclaimed on restart.
- Treat old keys/schema versions as explicit migrations. Missing/corrupt wrapped key returns an error; do not silently create a new key over existing data. No portable backup/restore or password-reset recovery promise in Stage 1.

Use Windows directory ACLs appropriate to the current user, allowing necessary OS administration. ACLs and authenticated encryption are complementary. Test copying a vault to another standard Windows user fails to unwrap. Confirm that restart under the original account decrypts successfully.

## Local credentials

One demo advisor account provisioned locally before the presentation. Store a random per-password salt and a parameterized `scrypt` password hash using Node crypto, with timing-safe comparison and modest failed-login throttling. Tune parameters to the target machine and record them in the envelope; do not invent a homegrown hash or persist the password. The hash is not a key used to encrypt client data. Password changes affect the demonstration login, not DPAPI recovery. No remote authentication, SSO or account-management service.

## Electron boundary and network policy

- Renderer: `contextIsolation: true`, `nodeIntegration: false`, `sandbox: true`, `webSecurity: true`. Narrow preload only; validate payload, active session, sender/frame, IDs and expected revisions in main.
- Deny arbitrary navigation, new windows, webviews, downloads, permission requests and external links. Stage 1 has no need to open websites.
- Restrictive packaged CSP: local script/style/worker sources only, no remote `connect-src`, no unsafe evaluation. Bundle PDF worker, character maps, fonts and icons. Permit only the tested local resource schemes needed by the viewer.
- Use an in-memory renderer session, no PII in localStorage, IndexedDB, service workers, persisted Zustand stores, caches, URLs, or browser history. [Electron session documentation](https://www.electronjs.org/docs/latest/api/session) distinguishes in-memory and persistent partitions. Verify the packaged disk footprint with synthetic canary values.
- Disable PDF scripting/actions, form submission, embedded file opening and external resource navigation. Only hash-checked bundled templates are accepted; future arbitrary imports need a new threat review.
- No analytics, telemetry, remote logging/crash reporting, updater, licensing client, AI/OCR API, CDN or backend. Disable unnecessary spellcheck/network-backed features; test packaged runtime egress with a Windows outbound-deny environment. CSP alone does not constrain Node main networking, so code/dependency review and an offline test are also required.
- DevTools/debug logs are development-only, using synthetic data. Do not enable crash-dump uploads. Local OS crash dumps, paging and hibernation can still hold memory; do not promise those are covered by app AES encryption.

These controls implement the applicable [Electron security recommendations](https://www.electronjs.org/docs/latest/tutorial/security). Development tooling may use the internet for dependency installation/research; the packaged application must not require it.

## Audit and diagnostic logging

Audit events: login success/failure, client opened, case created, document selected/deselected, master/case field committed, document override/checkbox changed, override cleared, verification started/failed/passed, warning acknowledged, approval, finalization, and draft preparation outcome. Record actor/time, opaque IDs, binding/rule identifiers and revisions, never old/new values. Coalesce text entry into meaningful committed edits instead of logging every keystroke.

Example allowed: `field.updated`, case ID, binding `client.address.line1`, revision 8. Forbidden: old/new address, plaintext SSN, password, recipient, email body, serialized IPC payload, PDF bytes or raw exception text that could contain values. Avoid free-text warning reasons; use a controlled acknowledgement reason code in Stage 1.

Store the audit with encrypted case/application data. A monotonic local sequence helps review event order, but the log is not immutable, independently timestamped or compliant evidence of nonrepudiation. Diagnostics outside the vault contain only stable error codes and counts; audit/login failures before unlock have value-free metadata and are encrypted where available. If encryption is unavailable, no plaintext fallback log.

## Plaintext lifecycle and the Outlook boundary

| Material | Where / how long | Cleanup and limitation |
|---|---|---|
| Original templates | Project/bundled resources; no client data | Read-only; hashes checked |
| Live case and working PDF representation | Process memory | Lock destroys visible renderer state and session immediately; main may retain unsaved data for reauthenticated recovery on save failure. Release when safe; JS cannot guarantee complete memory erasure |
| Case/audit/finalized internal/client artifacts | Encrypted app vault/blobs | Retained for local resume; explicit demo reset; no silent deletion on quit |
| Attachment staging PDFs | Random app-owned directory under LocalAppData; current operation only | Create just before handoff, attach by value, save/check draft, then delete on success |
| Failed/ambiguous staging | Controlled recovery directory with operation manifest | Keep until helper/draft status reconciled; retry cleanup on app restart and next cleanup pass, proposed stale threshold 24 hours; never delete a still-running operation's files |
| Explicit user export | Folder chosen through a main-owned save dialog | Ordinary readable PDFs outside app encryption; user/firm owns retention and deletion |
| Outlook draft/cache/server copies | Outlook-managed storage | App cannot guarantee retention, recall, encryption or local-only behavior; never delete Outlook data automatically |

Verify path containment under the exact staging root before deletion; reject reparse points/path escapes. Never recursively clean arbitrary temp/user directories. Antivirus/file locks cause a visible cleanup warning and later retry. Deletion is best effort, not secure wipe on SSDs/backups. No plaintext JSON/body request files are needed; the helper reads bounded structured input via pipe.

**Local-only qualifier:** app computation and internal persistence are local. Choosing Prepare Client Email intentionally transfers recipient/body/attachments to Outlook. A mailbox can synchronize its Drafts before the advisor presses Send. Therefore an absolute "PII never leaves this device" promise is incompatible with an online mailbox draft. Use fictional data and an offline Classic Outlook profile/work-offline rehearsal for Stage 1; before real use the firm must approve this delivery boundary. If strict no-egress is required for real data, Outlook handoff must be disabled or replaced by an approved later workflow.

## Demo security versus production review

| Demo control implemented after approval | Still needs production work |
|---|---|
| Encrypted authenticated local vault; DPAPI key wrapping | Enterprise key recovery/rotation, multi-user isolation, portable backup and disaster recovery |
| Local hashed-password gate | Firm identity, roles, authorization, lock/session policy and offboarding |
| Value-free encrypted audit | Tamper resistance, retention/legal holds, trusted time and firm audit requirements |
| Trusted bundled template library | Template provenance, current-form approval, malicious PDF isolation and update governance |
| Read-only preparation fields, blank client signing fields | Signing provider, consent/identity/evidence, permitted form modifications and client copy policy |
| Controlled draft and staging | Secure client delivery, mailbox/DLP policy, export retention and cleanup guarantees |
| Packaged offline demo | Code signing, vulnerability response, secure update channel, installer reputation and enterprise deployment |

No compliance certification or claim that a PNG constitutes a regulated electronic signature is made. These are implementation and review boundaries specific to the requested product, not an assertion of legal requirements.
