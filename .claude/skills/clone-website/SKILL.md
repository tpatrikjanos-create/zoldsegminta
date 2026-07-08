# Clone Website Agent Specification

This document outlines a comprehensive pipeline for reverse-engineering and rebuilding websites as pixel-perfect clones using browser automation, parallel task dispatch, and incremental verification.

## Core Architecture

The process follows five sequential phases:

**Phase 1: Reconnaissance** involves capturing full-page screenshots at multiple viewports, extracting global design tokens (fonts, colors, favicons), and conducting mandatory interaction sweeps to discover scroll-driven behaviors, click handlers, and hover states before any building begins.

**Phase 2: Foundation Build** establishes shared infrastructure: typography configuration, color token system, TypeScript interfaces, SVG icon extraction, and asset downloading—all verified with `npm run build`.

**Phase 3: Component Specification & Dispatch** cycles through each page section: extract detailed styles via `getComputedStyle()`, write a comprehensive spec file, then delegate building to specialized agents working in parallel worktrees.

**Phase 4: Page Assembly** wires all sections together in the root page component, implementing layout, page-level behaviors, and interaction patterns.

**Phase 5: Visual QA Diff** compares the clone against the original side-by-side at multiple viewports and interaction states.

## Critical Extraction Principles

**Completeness over speed:** "Every builder agent must receive *everything* it needs to do its job perfectly" including exact CSS values, downloaded assets with local paths, real text content, and component structure. Guessing any property constitutes extraction failure.

**Small, focused tasks:** Complex sections break into multiple agents—each handling a single sub-component with ~150 lines of spec maximum. Monolithic briefs produce approximations.

**Real content and layered assets:** Extract actual text, images, and videos. Multi-layered sections (background watercolor + foreground mockup) require enumerating all `<img>` elements and background-images within containers, including absolutely-positioned overlays.

**Behavior before building:** Document interaction models (click-driven vs. scroll-driven) before construction begins. This determines architecture—getting it wrong requires complete rewrites.

**All states captured:** Extract every tab state, hover effect, and scroll-triggered transition—not just the default viewport state.

## Specification Files

Every component receives a mandatory spec file at `docs/research/components/<name>.spec.md` containing:

- DOM structure and computed styles with exact property values
- All states and triggers (scroll positions, click handlers, intersection thresholds)
- Per-state content for tabbed/conditional sections
- Asset references and text content verbatim from the live site
- Responsive breakpoint behavior

Builders receive spec file contents inline; the file persists as an auditable artifact.

## Key Guardrails

- Identify interaction models through scrolling observation BEFORE clicking
- Download and reference real assets rather than generating mockups
- Verify `npm run build` passes after every merge
- Test at 1440px, 768px, and 390px viewports during extraction
- Detect smooth scroll libraries (Lenis, Locomotive Scroll) that affect perceived scroll behavior
- Enumerate layered images—missing overlay images make sections look empty despite correct backgrounds

The pipeline prioritizes incremental progress with verified builds at each checkpoint over speed.
