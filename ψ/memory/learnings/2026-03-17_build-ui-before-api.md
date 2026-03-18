# Build the UI Before the API

**Date**: 2026-03-17
**Source**: Library page — built complete frontend before backend existed
**Tags**: workflow, frontend, api-contract, parallel-work

## Lesson

When a new feature needs both frontend and backend, build the UI first. The page component defines the API contract — what endpoints are needed, what data shapes are expected, what query parameters exist. The backend engineer then builds to spec instead of guessing.

This unblocks the frontend designer, gives the backend a concrete target, and surfaces mismatches early (before data flows through).

## Evidence

- Library page: built with SearchInput, FilterTabs, document cards, detail view — all before any API existed
- Karo received a clear spec: `GET /api/library?q=&category=` returns `{ docs: [...] }`, `GET /api/library/:id` returns `{ doc: {...} }`
- Same pattern worked for Remote Control: I designed the page, Karo built the API to match

## Application

- For new Den Book pages: design + build the UI first, post the API contract in the thread, let Karo build to spec
- The TypeScript interfaces in the component ARE the contract — share them directly
- Mock data in the component is fine during development — replace with real fetch when API lands
