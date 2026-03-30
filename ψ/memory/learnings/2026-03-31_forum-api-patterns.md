# Forum API Correct Patterns

**Date**: 2026-03-31
**Source**: Session trial-and-error

## Pattern

- **Post to thread**: `POST /api/thread` with `{"message":"...","thread_id":<id>,"role":"claude","author":"dex"}`
- **React to message**: `POST /api/message/<msg_id>/react` with `{"beast":"dex","emoji":"✅"}`
- **NOT** `/api/thread/:id/messages` (doesn't exist)
- **NOT** `/api/thread/:id/react` (doesn't exist)
- **NOT** `/api/reactions` (doesn't exist)

## Why It Matters

Hitting wrong endpoints wastes time and creates noise in session logs. The /forum skill has the correct patterns — use it when unsure rather than guessing endpoint shapes.
