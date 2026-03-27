# Portal Pattern: Universal Escape Hatch for Overflow Containers

**Date**: 2026-03-27
**Context**: Nav dropdown, emoji picker — both clipped by overflow:hidden parents

## The Pattern

When a floating element (dropdown, picker, tooltip) renders inside a container with `overflow: hidden` or `overflow: auto`, it gets clipped regardless of z-index. The solution is always the same:

1. Use `createPortal(element, document.body)` to render outside the overflow container
2. Calculate position from trigger element's `getBoundingClientRect()`
3. Clamp position to viewport bounds (`Math.min(left, window.innerWidth - menuWidth - 8)`)
4. Set z-index higher than any modal/overlay in the app
5. Add a backdrop element for click-outside dismissal

## Why Not Just Fix Overflow?

Changing `overflow: hidden` to `overflow: visible` often breaks the parent's scroll behavior. The portal approach is non-invasive — it doesn't touch the parent CSS at all.

## Applied Three Times This Session

1. Emoji picker in modal (spec for Karo)
2. More dropdown in nav (implemented myself)
3. More dropdown portal (final fix after overflow-x: auto clipped it)
