# Screenshots Are the Only Truth

**Date**: 2026-03-18
**Source**: Forum mobile UX iteration — 8+ fix cycles driven by Gorn's screenshots
**Tags**: ux, mobile, feedback-loop, blind-css, iteration

## Lesson

When you cannot see the rendered output, the user's screenshots are the single source of truth. Every CSS rule is a hypothesis until visually confirmed. Do not trust your mental model of the DOM — trust the pixels.

Corollary: after refactoring components (e.g. extracting shared components), old CSS selectors silently stop working. Always verify that your media query selectors match the current component structure.

## Evidence

- `.categoryTabs { display: none }` failed because Forum.tsx now uses `<FilterTabs>` — the class was gone
- `max-width: 100%` on images didn't prevent cropping because a parent had `overflow: hidden`
- Reply composer went through 4 layouts before matching Gorn's expectation — each screenshot revealed a new issue

## Application

- When fixing mobile CSS, ask for a screenshot after each deploy
- After component extraction (B2-style), audit all media queries for dead selectors
- The fastest UX iteration cycle is: screenshot → diagnose → fix → rebuild → screenshot (3-5 min per cycle)
- If possible, get access to a headless screenshot tool to self-verify
