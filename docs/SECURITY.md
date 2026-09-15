---
node_id: security-faa-local
type: domain
title: Local security and document handoff design
created: 2026-09-14
updated: 2026-09-15
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

**Local-only qualifier:** app computation and internal persistence are local. Choosing Open Outlook Draft intentionally transfers recipient/body/attachments to Outlook. A mailbox can synchronize its Drafts before the advisor presses Send. Therefore an absolute "PII never leaves this device" promise is incompatible with an online mailbox draft. Use fictional data and an offline Classic Outlook profile/work-offline rehearsal for Stage 1; before real use the firm must approve this delivery boundary. If strict no-egress is required for real data, Outlook handoff must be disabled or replaced by an approved later workflow.

## Practical Windows gap review (2026-09-15)

The existing encryption, process/IPC boundary, template hashes, no-script PDF viewer, no-network runtime, revision-bound approval, staging and Outlook sync limitations remain the baseline. The additions below close interaction and lifecycle gaps; they do not replace those controls. All Stage 1 controls are planned, not yet implemented.

### Clipboard, context menus, printing and drag/drop

- **Sensitive identifiers:** SecureField blocks Copy/Cut and selection drag for SSN, Tax ID and government-ID values whether revealed or masked. Paste is allowed into an authorized editor as bounded plain text, with normal field normalization/validation; never execute or preserve pasted HTML. Masked rendering and assistive text must not contain the full value. Classification comes from versioned binding metadata, not a visual label heuristic; unknown PDF widgets default to restricted.
- **PDF surface:** disable Copy/Cut across canvas text selection and PDF widgets in Stage 1, including keyboard shortcuts and context-menu paths. This avoids treating unclassified printed PDF text as safe to copy. Text selection can remain for reading; direct field editing/paste still works. Never automatically copy PII, package contents or an identifier as a side effect of navigation, validation or export.
- **Other form text:** explicit Copy/Cut may use normal editing behavior for unrestricted fields (e.g. client name). This can still place PII in Windows clipboard history/cloud sync. Show the limitation once in Settings/privacy help; do not claim all PII copying is prevented or interrupt each edit. Pasting never clears or replaces the user's clipboard.
- **Clipboard lifetime:** Stage 1 has no programmatic sensitive-copy feature and no global clipboard timer/clear-on-lock sweep. Do not erase clipboard content the app cannot prove it owns. If a future explicit sensitive-copy feature is approved, require a main-owned short lease (proposed 60 seconds), clearing on expiry/lock only when Windows ownership/sequence identity still matches. A read-then-clear string comparison alone is racy and insufficient; implement/test an atomic ownership-checked Windows helper before enabling that feature, otherwise leave copying disabled. Clearing current clipboard cannot revoke clipboard history, synchronized copies or already pasted data. Electron's clipboard API belongs behind the main boundary; no generic clipboard bridge. [Electron clipboard API](https://www.electronjs.org/docs/latest/api/clipboard).
- **Printing:** unnecessary for Stage 1. Remove PDF print UI, Ctrl+P/menu accelerators and print handlers; expose no print IPC. Future printing is explicit plaintext egress to spooler/device and needs its own policy.
- **Owned menus only:** editable form fields have only permitted text-edit actions; PDF fields offer Paste where editable and Reset to synced value where applicable. No browser-default Save As, Print, Open Link, Inspect, download or embedded attachment action. Export remains an explicit main-owned finalized-package action. Development-only Inspect is not included in the packaged demo.
- **Drag/drop:** deny PDF/file drag-out, file/text drops into the PDF viewer, arbitrary external PDF import and navigation by drop. No `startDrag` capability or dropped file-path bridge. Block default drag/drop on the renderer surface, including form-input drop; ordinary explicit clipboard paste remains available. Reordering/moving text by drag is not needed in Stage 1.

These are owned UI affordance controls, not defenses against a compromised renderer or OS. Test mouse, keyboard, accessibility actions and app menus, not only hidden toolbar icons.

### Presentation and screen capture

Presentation Mode is a privacy aid, **not a security boundary**. Visible information can be captured by screenshots, cameras, Teams/Zoom sharing or recording. The app cannot detect/control all captures or revoke recordings. Encryption at rest does not encrypt pixels or plaintext used in process memory; OS dumps, pagefile, hibernation and administrator access remain outside this guarantee. Do not promise screenshot blocking or secure memory erasure.

Mask configured left-side identifiers; suppress internal notes, advisor-only metadata and debug information. Keep SaveStatus and actionable errors visible. Default identifiers remain masked, with per-field Reveal/Hide and explicit revealed status. The component policy permits a future firm to invert ordinary-view defaults; no production policy administration is built in Stage 1. Reentry, blur and lock clear transient reveal state. Exact interactions belong to the [UX spec](specs/2026-09-14-stage-1-ux.md#presentation-mode) and [design spec](specs/2026-09-15-visual-design-system.md).

Client-visible PDF pages remain accurate: never substitute bullets into printed values or describe visual masking as redaction. Show a restrained “PDF values may be visible to viewers” note. Audience metadata marks intake page 7 internal-only. In Presentation Mode withhold internal pages/notes from canvas, thumbnails, text/search layer, accessible content and issue snippets; show a neutral placeholder. Internal issue navigation asks the advisor to leave Presentation Mode before displaying its details. Main still validates every selected document/internal requirement; presentation filtering never bypasses approval blockers or changes output policy. PDF bytes may still exist in process memory, so this filtering is not isolation from software inspecting memory.

### Windows shell, windows and lifecycle events

- Working plaintext stays in process memory or the already specified app-owned staging root; never auto-save working PDFs under Desktop, Documents, Downloads or the general OS temp directory. Use opaque operation IDs and non-PII form filenames. Do not call `addRecentDocument`, register case jump-list tasks or file associations, or pass case paths into shell history. Explicit user exports/Outlook may create external MRU/search/index records outside our control; do not clear global Windows history. [Electron Recent Documents](https://www.electronjs.org/docs/latest/tutorial/recent-documents).
- Enforce `requestSingleInstanceLock` before a case window opens. A second launch can focus the existing window but cannot unlock it, open a second case window or accept a case path/URI from its command line. No custom protocol/deep links or arbitrary command-line file opening in Stage 1. Deny renderer `window.open` and BrowserWindow creation; any future secondary window needs an explicit main-owned design.
- Main subscribes to Windows workstation `lock-screen` and OS `suspend`: immediately revoke session, hide/destroy renderer PII and reject in-flight stale case commands under the existing lock/save-failure policy. Resume/unlock shows the local login, never automatically restores visible data. Test races with verification, finalization and handoff; an already created external Outlook draft cannot be hidden or recalled by app lock.
- App blur immediately remasks revealed fields. Continuous background time of **10 minutes** locks the local session; measure elapsed time in main and recheck before showing data on focus, since timers may be delayed. Keep the event policy independent of component focus. No foreground idle timeout in the Stage 1 demo, to avoid interrupting a live conversation; configurable foreground idle/session policy is production work. Presentation Mode never suppresses OS lock or suspend.
- Electron exposes Windows lock and suspend/resume events through [powerMonitor](https://www.electronjs.org/docs/latest/api/power-monitor). A shutdown callback is not a Windows durability guarantee. Normal-close save handling remains in UX; forced termination/power loss restores only the last durable authenticated revision.

### Retention, reset, corruption and uninstall

| State / operation | Explicit policy |
|---|---|
| Active case | Encrypted autosave and one authenticated previous backup; preserved across close/lock/restart |
| Completed case | Retain encrypted source/revisions/audit/finalized artifacts for resume/review; finishing a session does not delete it |
| Demo reset in Settings | One clearly scoped confirmation deletes app-owned demo profiles/cases/artifacts, their backup and case audit history; reset only after ongoing operations are resolved. Preserve provisioned login/settings and immutable bundled templates. No reset during active handoff |
| Failed reset | Report incomplete cleanup and keep a recoverable operation state; never claim all data removed. Retry only within verified app-owned paths |
| Corrupted vault/key | Fail closed, offer verified last-good recovery when decryptable; never overwrite on startup. Destructive demo reset is a separate explicit choice identifying lost data. No promise of recovery without the original Windows key context |
| Outlook staging | Use the existing reconciliation/cleanup policy above, including pending/locked files; reset cannot discard an active handoff's evidence/files |
| User export / Outlook copies | User/firm-owned plaintext retention. Reset/uninstall cannot recall drafts, attachments, sent copies, recordings or external exports |
| Uninstall / reinstall | Preserve vault, wrapped key and encrypted artifacts by default; no automatic user-data deletion. Explicit in-app demo reset is separate. Reinstall under the same Windows user can recover only if compatible data/key material remains |

Deletion is not secure erasure of SSDs, backups or OS memory. Production retention periods, legal holds, approved disposal, backup recovery and firm offboarding require firm policy; they are not inferred from closing a demo case.

### Supply chain, template trust and demo identity

M1 commits a dependency lockfile, pins a supported Electron version and resolved dependencies, uses reproducible `npm ci`, inventories direct/transitive licenses and bundles all runtime assets. Before each demo release, review `npm audit`/advisories and triage runtime-reachable findings; no blind `audit fix --force`. Block known exploitable critical/high runtime issues unless an explicit technical risk review establishes a mitigation. Audit results are development evidence, not runtime network traffic. Production adds a named patch owner/SLA, SBOM, signed executable/helper and signed installer, provenance verification and a reviewed signed update/rollback strategy. Stage 1 has no updater and must not be presented as signed if it is not.

Existing template hashes and versioned mapping identity remain mandatory. The originals contain date-format JavaScript: preserve immutable originals but **never execute** their scripts/actions, submission or external links in the viewer. Future template updates require provenance/owner approval, reviewed hash + mapping + rule + audience versions, full regression/export checks and case-version migration rules. No arbitrary external templates or runtime downloads in Stage 1; hashes establish identity, not proof that a PDF is harmless.

Local username/password remains a deliberately simple demo gate. It is not Windows/Entra identity, multi-user authorization, remote revocation or firm offboarding. Do not expand it into a homegrown enterprise identity system.

The [VP fixture](references/vp-demo-script.md#synthetic-fixture-contract) is the durable synthetic dataset specification. It uses `.invalid` email, explicitly fictional organizations/address and invalid all-zero SSN; never use plausible randomly generated SSNs. M3 materializes and validates it as a local executable fixture after coding approval. The planning fixture is not evidence of a running seeded application.

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
