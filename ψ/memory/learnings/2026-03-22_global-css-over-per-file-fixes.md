# Lesson: Global CSS Rules Over Per-File Fixes

**Date**: 2026-03-22
**Context**: iOS auto-zoom fix — started fixing individual CSS files, then realized a single global rule covers everything.

## Pattern

When a CSS fix applies to all instances of an element type (like "all inputs need font-size >= 16px on mobile"), write ONE global rule in the root stylesheet instead of hunting through every component CSS file.

## Example

```css
/* index.css — one rule, all inputs fixed */
@media (max-width: 768px) {
  input, textarea, select {
    font-size: 16px !important;
  }
}
```

vs. editing Forum.module.css, DirectMessages.module.css, Search.module.css, Board.module.css... one at a time.

## Why

- Fewer edits = fewer rebuild cycles = faster iteration
- No risk of missing a file
- Future inputs automatically covered
- `!important` is justified here because it's a platform constraint, not a style preference

## When to Apply

- Platform-level constraints (iOS zoom, touch target sizes, safe area insets)
- Universal accessibility rules
- NOT for design choices that may vary per component
