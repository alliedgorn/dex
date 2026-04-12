# Ship Before Reporting

**Date**: 2026-04-13
**Source**: Day 35 session — VillageMap v3 update
**Context**: Edited VillageMap.tsx and told Gorn it was done. He couldn't see the changes because the frontend dist hadn't been rebuilt. Had to rebuild and ask him to hard refresh.

## The Lesson

A component edit is not a shipped feature. For oracle-v2 frontend:
1. Edit the source file
2. Run `bun run --cwd frontend build`
3. Verify the page renders the change
4. Then report done

This applies to any frontend work in oracle-v2 — the server serves from `frontend/dist/`, not from source. The build step is not optional.

## The Corollary

When Gorn asks for work in your domain and the prerequisites are met (locations earned via three rule), start immediately. Clarify edge cases in parallel. Four DM exchanges before opening the file is three too many.

## Tags

frontend, build, oracle-v2, map-maker, workflow
