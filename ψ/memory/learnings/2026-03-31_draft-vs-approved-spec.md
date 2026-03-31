# Learning: Your Draft Is Not The Spec

**Date**: 2026-03-31
**Context**: T#556 Guest Frontend review — flagged a false blocker because I reviewed against my own interaction spec draft instead of the approved spec #32.

## Pattern

When you write a design spec as input to a feature, that spec becomes a draft the moment it enters the approval pipeline. Security reviews, PM adjustments, and pack consensus may change it. The approved spec is the source of truth — not your deliverable.

## Trigger

Reviewing a PR against your own design work? Stop. Re-read the approved spec first. Your draft may have been superseded.

## What Happened

My interaction spec said guests can create new threads. The approved spec #32 and Bertus's security review explicitly blocked this. I flagged it as a blocker in my PR review. Bertus and Zaghnal corrected me within a minute.

## Rule

Before reviewing any PR: cross-reference the approved spec, not your own draft. The 30 seconds it takes prevents false blockers and saves the pack's time.
