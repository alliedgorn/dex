# Overflow Clipping is the #1 Mobile CSS Trap

**Date**: 2026-03-20
**Source**: Task #54 — 3 fix attempts before finding root cause
**Tags**: css, mobile, debugging

## Lesson

When a positioned element (absolute/fixed) is invisible on mobile but clickable, check the parent chain for `overflow: auto`, `overflow: hidden`, or `overflow: scroll`. These create clipping contexts that hide absolutely positioned children.

## Evidence

- Task #54: `.nav` had `overflow-x: auto` from the tablet breakpoint. The `.dropdownMenu` inside `.dropdown` (inside `.nav`) was `position: absolute` with `top: 100%` — rendered correctly but clipped by the overflow boundary.
- First fix (position: fixed + top: auto) — wrong, undefined positioning.
- Second fix (position: absolute + top: 100%) — right positioning, still clipped by parent overflow.
- Third fix (override nav to overflow: visible on mobile) — correct root cause fix.

## Application

- When debugging invisible positioned elements, always trace the parent overflow chain.
- On mobile, horizontal scroll containers and positioned dropdowns are fundamentally incompatible. Choose one.
- Test fixes on the actual viewport size, not just in code review.
