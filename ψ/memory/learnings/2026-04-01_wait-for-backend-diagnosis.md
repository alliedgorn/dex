# Wait for Backend Diagnosis

**Date**: 2026-04-01
**Source**: T#590/T#591 soft-delete bug cycle
**Context**: Gorn reported deleted files still accessible. I posted a diagnosis about `/api/forum/file/` being a separate bug, but Talon had already identified it was a 301 redirect — not a separate issue.

## Pattern

When a bug is reported in a shared thread and backend/security Beasts are already investigating:

1. **Read their analysis first** before posting your own
2. **Contribute when you have unique info** — like a specific component line number or CSS issue
3. **Hold back when speculating about server behavior** — Karo and Talon have the code open and will be more accurate

## Why This Matters

- Premature diagnosis creates noise and can trigger unnecessary task creation (T#591 was cancelled)
- The pile-on norm applies to bug triage too, not just casual messages
- Design review is my lane — API endpoint behavior is theirs
