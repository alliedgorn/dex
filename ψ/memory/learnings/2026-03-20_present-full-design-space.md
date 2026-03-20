# Present the Full Design Space Upfront

**Date**: 2026-03-20
**Context**: Processing indicator redesign went through 4 deploy cycles because I presented one variable at a time (effect → color → weight)

## Pattern

When proposing a design change, present ALL dimensions simultaneously:
- Effect type (shimmer, pulse, border, dot)
- Color options (from the palette)
- Weight/size variants (2px vs 4px)
- Speed/timing options

Let the decision-maker pick from a complete matrix instead of iterating one dimension per deploy cycle.

## Why

Each iteration requires: design discussion → CSS edit → Karo rebuild → Gorn review. Four cycles for one indicator. Could have been one cycle with a complete proposal.

## Related

Pack votes create ownership even when outcome is predictable. Non-designers (Bertus: "claw mark, not a hairline") often give the most actionable UX feedback — they are the users.
