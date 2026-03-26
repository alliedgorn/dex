# Lesson: Fixed-Position Overlay for Mobile Inputs

**Date**: 2026-03-27
**Source**: T#348 mobile search dropdown fix
**Tags**: css, mobile, ux, layout

## Pattern

When a UI element (search input, filter bar, compose field) needs to appear/disappear on mobile and would cause layout reflow if placed inline, use a **fixed-position overlay** instead.

## Why

Inline expansion within a flexbox header causes the parent to recalculate widths, shifting all siblings. On mobile, this manifests as a visible "jump" that feels broken. Fixed-position elements are removed from document flow — they overlay without affecting layout.

## Implementation

```css
@media (max-width: 480px) {
  .expandableInput {
    position: fixed;
    top: 8px;
    left: 8px;
    right: 8px;
    z-index: 301;
    /* Visual emphasis to show it's an overlay */
    box-shadow: 0 4px 16px rgba(0,0,0,0.4);
    border: 2px solid var(--accent);
  }
}
```

## When to Apply

- Search inputs in headers
- Filter bars that toggle visibility
- Any mobile input that replaces a button/icon trigger
- Compose fields in bottom bars

## When NOT to Apply

- Desktop layouts with enough space for inline expansion
- Elements that should push content down (disclosure panels)
