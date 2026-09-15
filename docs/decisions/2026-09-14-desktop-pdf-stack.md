---
node_id: adr-2026-09-14-desktop-pdf-stack
type: adr
title: Desktop and interactive PDF stack
created: 2026-09-14
updated: 2026-09-15
status: draft
adr_status: proposed
category: decision
tags: [desktop, pdf]
summary: Prefer Electron, an overlay-first PDF.js display adapter and a main-owned output library chosen by M0 proof.
---

# ADR: Desktop and interactive PDF stack

## Status

Proposed; owner review pending and PDF spike required.

## Context

The demo needs rich interactive PDF forms, immediate shared-data updates, deliberate document edits, Windows local storage, and Classic Outlook. Existing templates use AcroForms, including 152 intake checkboxes. pdf-lib 1.17.1 can enumerate them, but interaction/write/render fidelity has not yet been proven.

Two facts sharpen the output-side risk. pdf-lib's last published release is 1.17.1 (2021), and its save routine serializes every object in the loaded document context without garbage collection. Removing a page from a full copy therefore leaves that page's content streams and field values in the output as unreferenced objects. The client copy that must omit intake page 7 cannot be produced that way. The inventory also shows only `/Tx` text fields and independent `/Btn` checkboxes, with no radio parents, choice fields or multi-widget fields; that narrows what the display adapter must support.

## Options considered

| Option | Pros | Cons |
|---|---|---|
| Electron + React + PDF.js + pdf-lib | Matches preferred skills; one TS domain; bundled rendering runtime; rapid polished UI | Larger application; careful sandbox/IPC design; PDF widget adapter must be built and tested |
| Tauri + React + PDF.js | Smaller host; explicit Rust command boundary | Adds Rust and WebView2 deployment/version concerns; PDF sync work remains |
| .NET WPF/WinUI | Strong Windows/COM integration and native controls | Different UI/domain stack; interactive PDF still needs WebView2/PDF.js or a separately evaluated SDK |

Output library candidates for main-owned writing, all evaluated on disposable copies in M0:

| Candidate | Assessment |
|---|---|
| pdf-lib 1.17.1 | Proven read-only enumeration here; unmaintained; no garbage collection on save; appearance regeneration for custom on-states must be tested |
| Maintained pdf-lib fork | Same API surface with active fixes; verify provenance, license, release cadence and the same garbage-collection behavior |
| MuPDF JavaScript/WebAssembly build | Mature engine with forms support and garbage collection on save; AGPL or commercial license must be cleared by the owner before adoption |
| PDFium WebAssembly binding | Chromium's engine under a permissive license; form fill, appearance generation and save support of the specific binding must be verified |

Display adapter candidates:

| Candidate | Assessment |
|---|---|
| Controlled HTML overlays at PDF.js annotation rectangles | Fully app-owned text and checkbox controls; covers every widget type in the inventory; alignment under zoom and Windows scaling must be proven |
| PDF.js annotation-layer widgets with event capture | Reuses viewer widgets; annotation storage and mounted-control rerendering are internal viewer behaviors that change across versions |

## Decision

Recommend Electron + React + TypeScript. PDF.js owns page rendering and the text layer; a main-owned output library writes outputs from canonical state. Neither library independently owns case values.

**Display adapter: overlay-first.** Build controlled, accessible HTML text and checkbox overlays positioned at PDF.js annotation rectangles as the primary adapter. Use PDF.js's own annotation-layer widgets only if overlays fail the M0 zoom/scroll alignment or keyboard criteria. Decide in M0, not by silently dropping direct editing.

**Output library: pdf-lib is the M0 baseline, not the decision.** Construct every audience subset by copying only the permitted pages into a fresh document; never remove pages from a full copy. Evaluate the candidates above in M0 against the same fixtures and choose by proof, license and maintenance status. Record the choice here before M3.

## Consequences

Positive: one model supports sample and real forms; no cloud PDF dependency; vendor replacement stays behind a narrow adapter. Overlays keep widget semantics in app code rather than viewer internals. Trade-off: repeated appearances, read-only export flags, font handling and signature preservation require fixture tests; fresh-document construction must recreate AcroForm field relationships for copied pages. Do not modify original templates or rely on internal library APIs throughout UI code.

## Follow-ups

- [ ] M0 technical lead: prove overlay alignment, focus, keyboard editing and check/uncheck appearances at 75/100/150% zoom and 100/125/150% Windows scaling; fall back to annotation-layer widgets only on documented failure.
- [ ] M0: build the client copy as a fresh document with pages 1-6 and prove, by searching every decoded object of the output, that no page 7 field name, value or content stream remains.
- [ ] M0: run the same fill/subset/reopen fixtures against each output candidate; record license, maintenance status and result; choose one.
- [ ] M0: verify output fonts and signature preservation; document any copy-only normalization.
- [ ] M1: pin compatible supported package versions and record licenses; read-only inspection version is not a recommendation to freeze all dependencies at that version.

## References

[Stack details](../ARCHITECTURE.md), [PDF findings](../references/pdf-findings.md), [plan](../plans/active/2026-09-14-stage-1-demo.md).
