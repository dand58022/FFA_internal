---
node_id: adr-2026-09-14-desktop-pdf-stack
type: adr
title: Desktop and interactive PDF stack
created: 2026-09-14
updated: 2026-09-14
status: draft
adr_status: proposed
category: decision
tags: [desktop, pdf]
summary: Prefer Electron and a PDF.js plus pdf-lib adapter, conditional on real-template feasibility.
---

# ADR: Desktop and interactive PDF stack

## Status

Proposed; owner review pending and PDF spike required.

## Context

The demo needs rich interactive PDF forms, immediate shared-data updates, deliberate document edits, Windows local storage, and Classic Outlook. Existing templates use AcroForms, including 152 intake checkboxes. pdf-lib 1.17.1 can enumerate them, but interaction/write/render fidelity has not yet been proven.

## Options considered

| Option | Pros | Cons |
|---|---|---|
| Electron + React + PDF.js + pdf-lib | Matches preferred skills; one TS domain; bundled rendering runtime; rapid polished UI | Larger application; careful sandbox/IPC design; PDF widget adapter must be built and tested |
| Tauri + React + PDF.js | Smaller host; explicit Rust command boundary | Adds Rust and WebView2 deployment/version concerns; PDF sync work remains |
| .NET WPF/WinUI | Strong Windows/COM integration and native controls | Different UI/domain stack; interactive PDF still needs WebView2/PDF.js or a separately evaluated SDK |

## Decision

Recommend Electron + React + TypeScript. PDF.js owns the page and interactive display layer; main-owned pdf-lib writes outputs from canonical state. Neither library independently owns case values. Start with PDF.js's form annotation layer behind an adapter; if live updates cannot reliably update mounted controls, use controlled HTML widgets aligned to PDF.js viewports for supported field types. Decide in M0, not by silently dropping direct editing.

## Consequences

Positive: one model supports sample and real forms; no cloud PDF dependency; vendor replacement stays behind a narrow adapter. Trade-off: repeated appearances, read-only export flags, font handling and signature preservation require fixture tests. Do not modify original templates or rely on internal library APIs throughout UI code.

## Follow-ups

- [ ] M0 technical lead: prove check/uncheck appearances and stable focus/scroll/zoom, choose display adapter.
- [ ] M0: verify output fonts and signature preservation; document any copy-only normalization.
- [ ] M1: pin compatible supported package versions and record licenses; read-only inspection version is not a recommendation to freeze all dependencies at that version.

## References

[Stack details](../ARCHITECTURE.md), [PDF findings](../references/pdf-findings.md), [plan](../plans/active/2026-09-14-stage-1-demo.md).
