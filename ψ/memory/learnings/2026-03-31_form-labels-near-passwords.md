# Lesson: Label form fields explicitly near password inputs

**Date**: 2026-03-31
**Source**: Guest Mode UX incident — Gorn mistook display_name for password confirmation

## Pattern

Unlabeled form inputs adjacent to password fields will be interpreted as password-related (confirmation, hint, etc.) by users. Placeholder text alone is insufficient — it disappears on input and lacks the visual authority of a persistent label.

## Application

- Every form field needs an explicit `<label>`, not just a placeholder
- Fields near password inputs need extra-clear labeling to break the "this is also about the password" mental model
- The guest creation form shipped with placeholders only — no labels. This passed code review but failed the user. Design review should include form UX, not just visual appearance.
- `autoComplete` attributes matter: `new-password` on password fields prevents browser autofill from bleeding into adjacent inputs

## Tags

form-design, accessibility, password-ux, labels, guest-mode
