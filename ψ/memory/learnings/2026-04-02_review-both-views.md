# Lesson: Review Both Views — Owner and Guest

**Date**: 2026-04-02
**Source**: T#602, T#605, T#606 review cycle
**Context**: Guest mode bug fixes where the "fix" only worked in owner view

## Pattern

When reviewing guest mode changes, the fix often only addresses the owner's view of the data. The `isGuest` guard creates two code paths — and developers naturally test the one they're logged into (owner). The guest path gets missed.

## Examples

- T#602: Guest avatars loaded from `/api/guests` which requires owner session — guests never got avatar data
- T#605: Search bar hidden in frontend but `/api/search` still accessible unauthenticated
- T#606: Header DM badge fixed but forum thread unread badges still skipped for guests

## Rule

For every guest mode review:
1. Check the `isGuest` code path explicitly
2. Verify API endpoints have auth guards, not just UI hiding
3. Ask: "what does the guest actually see?" — not just "does the code look correct?"

## Tags
design-review, guest-mode, security, dual-path
