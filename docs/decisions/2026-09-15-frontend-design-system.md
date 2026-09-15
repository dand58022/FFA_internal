---
node_id: adr-2026-09-15-frontend-design-system
type: adr
title: Locally owned desktop design system
created: 2026-09-15
updated: 2026-09-15
status: draft
adr_status: proposed
category: decision
tags: [frontend, accessibility, offline]
summary: Final planning recommendation for styling, primitives, icons, fonts and restrained motion.
---

# ADR: Locally owned desktop design system

## Status and context

Proposed for owner review; this settles the planning recommendation, not coding authorization. The [desktop ADR](2026-09-14-desktop-pdf-stack.md) remains unchanged: Electron, React and TypeScript fit the offline meeting application. The unfinished decision was how to turn accessible primitives into a consistent client-facing financial-services UI.

## Decision

Use React + TypeScript, electron-vite/Vite, **Tailwind CSS 4 compiled locally**, **locally owned shadcn/ui components using Radix primitives**, and **lucide-react** named icon imports. Check compatible supported package versions together in M1 and commit the lockfile. shadcn supports different primitive choices; explicitly use the Radix implementation consistently, not a mixture of primitive systems.

Copy only needed components into the repository, review their source, and customize them to the [visual design specification](../specs/2026-09-15-visual-design-system.md). shadcn is a code distribution model, so the project owns accessibility, maintenance and updates to the copied code. Never blindly regenerate customized components. [shadcn documentation](https://ui.shadcn.com/docs), [Vite integration](https://ui.shadcn.com/docs/installation/vite).

Semantic CSS custom properties are the token source of truth; map them to Tailwind theme utilities. Avoid arbitrary color/spacing literals in feature components. Use no Next.js runtime, server rendering, hosted component service, CDN or runtime registry. Tailwind builds static styles through Vite; installation tools may use the network during development, while the packaged app loads only local assets. [Tailwind Vite installation](https://tailwindcss.com/docs/installation/using-vite).

Use `"Segoe UI", system-ui, sans-serif` on Windows. No remote fonts or font download. PDF fonts/character maps remain locally bundled and governed by the PDF adapter; application typography never substitutes the printed PDF font. Bundle named Lucide SVG icons through the application build. [Lucide React](https://lucide.dev/guide/react).

Keep React Hook Form for section forms, Zod for bounded schemas, Zustand for the transient renderer projection, and main-owned domain validation/revision rules. No component library becomes the data authority; no persisted form/Zustand plugin stores PII.

Use CSS transitions only in Stage 1: 120–180 ms opacity or short drawer translation; no ordinary form-entry animation. Respect `prefers-reduced-motion`. Motion for React (formerly Framer Motion) is compatible with bundled offline use and has reduced-motion support, but these few transitions do not justify another runtime dependency. Reconsider only for a demonstrated interaction need. [Motion accessibility](https://motion.dev/docs/react-accessibility).

## Alternatives and consequences

| Choice | Assessment |
|---|---|
| Radix + entirely custom CSS | Viable; more initial component work and inconsistent styling risk. Keep Radix beneath owned shadcn components. |
| Stock shadcn theme | Fast but too generic; reject stock appearance in favor of the navy/light meeting design. |
| Full enterprise grid/component suite | Adds density, styling constraints and possible licensing/runtime weight without a Stage 1 requirement. |
| Motion dependency now | Viable offline; defer because CSS meets the defined transitions. |

Radix helps with roles, focus and keyboard behavior; composition and PDF widgets still need independent testing. Use Vitest/Testing Library for domain and meaningful component behavior, Playwright for Electron interaction, and real Windows display/keyboard review. [Radix accessibility](https://www.radix-ui.com/primitives/docs/overview/accessibility).

## Follow-ups

- M1: pin/review dependencies, establish local tokens/primitives and offline asset policy.
- M4–M6: compose the screens and prove PDF focus, presentation restrictions and issue navigation.
- M9: verify the display/accessibility matrix in the [design spec](../specs/2026-09-15-visual-design-system.md); do not equate accessible primitives with an accessible finished application.
