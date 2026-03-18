# Den Book UI Audit — Dex

**Date**: 2026-03-17
**Auditor**: Dex (Beast #10, UX/UI Design)
**Codebase**: `/home/gorn/workspace/oracle-v2/frontend/`
**Status**: First audit — full surface scan

---

## 1. What Exists Today

### Architecture
- **Framework**: React 19.2 + React Router DOM + Vite
- **Styling**: CSS Modules (26 `.module.css` files) + 1 global `index.css`
- **Fonts**: Bitter (headings), Inter (body), JetBrains Mono (code) — loaded via Google Fonts
- **Pages**: 21 routes, each with its own component + CSS module

### Design System (index.css)
A solid foundation exists:
- Dark charcoal base palette (`#111110` → `#2a2825`)
- Earth-tone accent (warm amber `#d4943a`)
- 7 beast-specific colors defined
- CSS custom properties for backgrounds, text, borders, accents
- Custom scrollbars, selection color
- Philosophy is clear: *"Underground den, not corporate dashboard"*

### Navigation
- Sticky header with logo, 13 nav links + Tools dropdown + session stats
- Three-zone layout: logo | nav links | session info
- Responsive breakpoints at 1100px, 900px, 480px

---

## 2. Issues Found

### CRITICAL: Hardcoded Colors (Design Token Drift)

The design system defines CSS custom properties, but **5 files bypass them entirely** with hardcoded hex values — 28+ occurrences of Tailwind/GitHub gray palette colors:

| File | Hardcoded Colors | Should Be |
|------|-----------------|-----------|
| `PackView.module.css` | `#e5e7eb`, `#9ca3af`, `#6e7681`, `#58a6ff` | `--text-primary`, `--text-secondary`, `--text-muted`, `--accent` |
| `PackView.tsx` (terminal) | `#0d1117`, `#c9d1d9`, `#58a6ff` (blue!) | Own scope OK, but selected state should use `--accent` |
| `GornQueue.module.css` | `#e5e7eb`, `#9ca3af`, `#374151`, `#6b7280`, `#d97706` | Design system vars |
| `Forum.module.css` | Multiple hardcoded grays | Design system vars |
| `Forum.tsx` | Hardcoded fallback colors | Design system vars |

**Impact**: If someone changes `--text-primary`, half the UI updates and half doesn't. The den has two color languages fighting each other.

### CRITICAL: Missing Beast Colors

The design system defines 7 beast colors but the pack has **10 beasts**. Missing:

| Beast | Animal | Needs Color |
|-------|--------|-------------|
| Pip | Otter | — |
| Nyx | Crow | — |
| **Dex** | **Octopus** | — |

### CRITICAL: Missing Octopus Emoji

`utils/animals.ts` maps animal names to emoji. **Octopus is not in the map.** Dex will render with no emoji or a fallback.

```typescript
// Current — no octopus
{ hyena: '🐾', horse: '🐴', alligator: '🐊', bear: '🐻',
  kangaroo: '🦘', lion: '🦁', raccoon: '🦝', otter: '🦦', crow: '🐦‍⬛' }
```

### HIGH: Layout Width Chaos

Every page picks its own `max-width` with no system:

| Width | Pages |
|-------|-------|
| 1400px | PackView |
| 1200px | Forum, DMs, SidebarLayout, Playground (partial) |
| 1100px | DocDetail (partial) |
| 1000px | Activity, Groups |
| 900px | Overview, Traces |
| 800px | GornQueue, Playbook, Playground |
| 720px | Search, DocDetail, Forum (partial) |
| 640px | BeastProfile, Settings, Playground (partial) |
| 400px | Login |

**9 different max-widths** across 21 pages. No width tokens, no layout scale.

### HIGH: Blue Accent Contamination

The design language is earth-tone amber (`#d4943a`), but the PackView uses **GitHub-blue** (`#58a6ff`) for:
- Selected beast card border and background
- Terminal cursor
- Send button
- Input focus state

This creates a jarring blue island in an otherwise warm interface.

### MEDIUM: Inconsistent Border Radius

- Header nav links: `8px`
- Dropdown menu: `8px`
- Dropdown items: `6px`
- Beast cards: `10px`
- Terminal panel: `12px`
- Profile card: `16px`
- Buttons: `6px`

No radius scale defined. Each component invents its own.

### MEDIUM: Duplicated Patterns

Common UI patterns are re-implemented per page instead of shared:
- Search inputs (Forum, DMs, Groups all have their own)
- Card/list item styles (repeated across Feed, Forum, Activity, Queue)
- Sidebar + content layouts (Forum and DMs both build from scratch)
- Filter/tab buttons (Activity, Queue, Forum all different)

### LOW: Font Hardcoding

`GornQueue.module.css` line 12: `font-family: 'Bitter', serif` — should use `var(--font-heading)`.

### LOW: No Spacing Scale

Padding and gaps are ad-hoc: 4px, 6px, 8px, 10px, 12px, 14px, 16px, 20px, 24px, 32px, 48px. No rhythm. No tokens.

---

## 3. What Works Well

- **The dark earth-tone palette is excellent.** Warm, distinctive, feels like a den.
- **Bitter + Inter + JetBrains Mono** is a strong font stack. Personality in headings, clarity in body, precision in code.
- **CSS Modules** prevent style leaks between pages — good architecture.
- **Responsive breakpoints exist** on most pages (768px, 480px).
- **The beast color concept** is strong — just needs completing.
- **Scrollbar and selection styling** shows attention to detail.

---

## 4. Design Direction — Three Options

### Option A: "Token Discipline" (Conservative)
**What**: Fix the token drift. Make every page use CSS custom properties. Add missing beast colors. Define width/radius/spacing scales. No visual changes.

- Effort: Medium
- Risk: Low
- Result: Same look, but maintainable and consistent

### Option B: "Den Refresh" (Moderate)
**What**: Everything in Option A, plus: extract shared components (SearchInput, FilterBar, Card, SidebarLayout), unify the PackView terminal to use den colors instead of GitHub-blue, add subtle beast-color accents to each beast's UI presence.

- Effort: High
- Risk: Medium (touches many files)
- Result: Cohesive visual identity, less code duplication, every beast has a color signature

### Option C: "The Octopus Touch" (Ambitious)
**What**: Everything in A + B, plus: design a proper component library (`den-ui/`), light mode support, animation system (subtle transitions for beast status changes, thread updates), beast avatar system (SVG portraits instead of emoji), and a style guide page (`/design`) that documents the system.

- Effort: Very High
- Risk: High (large surface area, needs Karo heavily)
- Result: A design system worthy of the pack. Scalable to 20+ beasts.

---

## 5. Recommendation

**Start with Option A immediately** — the token fixes are pure cleanup with zero visual risk. Then move to **Option B** for the real payoff.

Option C is the destination, but we walk there — we don't teleport.

### Immediate Fixes (can ship today)
1. Add octopus to `ANIMAL_EMOJI` map → `octopus: '🐙'`
2. Add missing beast colors to `:root` → otter, crow, octopus
3. Replace hardcoded hex colors with CSS custom properties in PackView, GornQueue, Forum

### Next Sprint
4. Define width scale tokens (`--width-narrow: 640px`, `--width-medium: 900px`, `--width-wide: 1200px`)
5. Define radius scale (`--radius-sm: 6px`, `--radius-md: 10px`, `--radius-lg: 16px`)
6. Extract shared SearchInput, FilterBar, Card components
7. Replace blue accent in PackView with amber

---

**Dex, Beast #10**
*Eight arms. Every pixel.*
