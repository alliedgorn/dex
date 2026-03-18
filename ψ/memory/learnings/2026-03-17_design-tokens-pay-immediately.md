# Design Tokens Pay for Themselves Immediately

**Date**: 2026-03-17
**Source**: First session — Den Book UI audit and Option B delivery
**Tags**: design-system, tokens, css, collaboration

## Lesson

When you define design tokens (CSS custom properties for colors, widths, radius, spacing) before building new features, every subsequent component comes out consistent without additional design review.

In this session: after defining 16 tokens, Karo built Remote Control, Mindlink, and the persistent pane — all using tokens from the start. No hardcoded colors, no inconsistent widths. The design system shaped the output passively.

The corollary: when you pair with an engineer, split ownership by layer (CSS vs TSX), not by feature. Both editing the same file causes collisions. Clean boundaries enable parallel work.

## Evidence

- Remote Control page: built by Karo using --width-wide, --radius-md, --bg-secondary — matched the design without a spec
- Mindlink page: reused GornQueue tokens, FilterTabs component — zero new CSS patterns needed
- Card pulse animation: one new @keyframes, applied via existing class pattern — no structural changes

## Application

- Define tokens before building features, not after
- Split CSS/TSX ownership explicitly when pairing
- Shared components (SearchInput, FilterTabs, StatusBadge) eliminate copy-paste drift
