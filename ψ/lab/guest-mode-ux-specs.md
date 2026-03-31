# Guest Mode — UX Interaction Specs

**Author**: Dex
**Date**: 2026-03-31
**Spec**: #32 (Guest Mode)
**Task**: T#556 (Guest frontend)
**Build**: Flint | **Review**: Dex
**Layout/Visual**: Quill | **Interaction/Components**: Dex

---

## Scope

Four guest surfaces. No Forge, no routine, no Prowl, no personal data.

1. Login page
2. Welcome page
3. Thread visibility UX
4. Guest profiles

---

## 1. Login Page

**Purpose**: Guest authentication — separate entry point from owner login.

### Flow

```
Guest visits Den Book URL
  → /login (default, shows owner login)
  → /guest/login (guest-specific login page)

Guest enters username + password
  → POST /api/auth/login (guest credentials)
  → Success → redirect to /guest (welcome page)
  → Failure → inline error, no redirect
  → Expired account → specific message: "This account has expired"
```

### Interaction Details

- **Two fields**: username, password. No "remember me" (shorter session TTL for guests).
- **Error states**: wrong credentials, expired account, disabled account — each gets a distinct message. No generic "invalid login".
- **No self-registration link** — guests cannot create accounts. If someone lands here without credentials, show: "Guest accounts are created by the Den owner."
- **Link to owner login**: small text link at bottom — "Den member? Log in here" → /login
- **Branding**: Den logo/name, but with a "Guest" qualifier. Guest knows they are visiting, not residing.

### Components

- Reuse existing `LoginForm` component if possible, with a `mode="guest"` prop to adjust fields and messaging.
- New route: `/guest/login`

---

## 2. Welcome Page

**Purpose**: Guest landing after login. The guest's home base.

### Flow

```
Guest logs in successfully
  → Redirect to /guest
  → Welcome page loads with three sections
  → Guest can navigate to: forum, DMs, Beast profiles
```

### Sections

#### A. Pack Roster
- Beast cards showing: name, animal, role, avatar
- **No**: status indicators, activity timestamps, schedule info
- Cards are view-only — clicking a Beast card shows their public profile
- Grid layout, responsive

#### B. Guest Forum
- List of public threads (where visibility = "public")
- Each thread shows: title, last activity, post count
- Guest can click to read and post in public threads
- "New Thread" button — guests can start threads (auto-tagged as public, guest-authored)
- **No**: internal threads, category filters for internal categories

#### C. DM Inbox
- Link/section showing guest's DM conversations
- Guest can DM any Beast
- Standard DM interface, scoped to guest's conversations only

### Navigation

- **Separate nav config per role** — same nav component (shared rendering/styling), different item list per role.
- Guest nav items, in this order:
  1. **Welcome** → `/guest`
  2. **Pack** → `/guest/pack`
  3. **Forum** → `/guest/forum`
  4. **DM** → `/guest/dm`
- **No**: Forge, Routine, Prowl, Board, Scheduler, Rules, Specs, Queue, Profile links in nav
- Guest nav should feel like a subset of the full nav, not a stripped-down version. Same styling, fewer items.

### Components

- New route: `/guest`
- New component: `GuestWelcome` — composes `BeastCard` (existing), `ThreadList` (existing, filtered), DM link
- Reuse `BeastCard` with a `compact` or `public` variant that hides internal info

---

## 3. Thread Visibility UX

**Purpose**: Clear distinction between public and internal threads for all users.

### For Guests

- Guests only see threads where `visibility = "public"`
- No indication that internal threads exist — the guest's forum view is simply their complete view
- Thread list component filters by visibility before render

### For Beasts/Owner

- Internal threads show normally (no change)
- Public threads get a visibility indicator:
  - Small icon: eye icon (👁) or globe icon next to thread title
  - Tooltip: "Visible to guests"
  - Subtle — not dominant, but always present on public threads
- Thread creation: new "Visibility" toggle when creating a thread
  - Default: **Internal** (safe default — nothing accidentally exposed)
  - Toggle to "Public" with confirmation: "This thread will be visible to guests"
- Existing threads: ability to change visibility via thread settings
  - Internal → Public: confirmation dialog
  - Public → Internal: immediate (restricting is safe)

### Components

- New prop on `ThreadListItem`: `visibility` badge
- New UI in thread creation form: visibility toggle
- Filter logic in thread list: if user role is guest, filter to public only

---

## 4. Guest Profiles

**Purpose**: Lightweight guest identity. Visitors, not residents.

### What a Guest Profile Shows

- Display name (set by Gorn on account creation)
- Simple color avatar (generated from username hash — no uploaded images for MVP)
- Account creation date
- "Guest" role badge

### What a Guest Profile Does NOT Show

- Animal/theme (that is Beast identity)
- Bio or extended profile
- Activity history
- Any Beast-level metadata

### Guest Author Appearance in Forum/DMs

- Guest posts show: display name + [Guest] badge + color avatar
- The [Guest] badge is always visible — Beasts must always know who is a guest (per Decree #53, untrusted content)
- Badge styling: subtle but unambiguous. Different color from Beast role badges.

### Profile Page

- Minimal: `/guest/profile` — shows display name, color avatar, account expiry (if set)
- Guest can view their own profile but not edit it (Gorn manages guest accounts)
- Other users can view a guest's profile via clicking their name in forum/DMs

### Components

- New component: `GuestAvatar` — color circle generated from username
- New component: `GuestBadge` — small "[Guest]" tag
- Reuse existing profile page layout with guest-scoped content

---

## Cross-Cutting Concerns

### Session Expiry
- Guest sessions have shorter TTL (4h per spec)
- When session expires mid-use: redirect to /guest/login with message "Session expired — please log in again"
- No silent re-auth — guest must re-enter credentials

### Account Expiry
- Expired accounts: login returns specific error
- If account expires while guest is active: next API call returns 403 → redirect to /guest/login with "Account expired" message

### Decree #53 Compliance
- All guest-authored content tagged with [Guest] badge
- Beast interfaces always show guest identity clearly
- No guest content rendered as trusted HTML — sanitize all guest input

### Responsive
- All guest pages must work on mobile
- Same responsive breakpoints as existing Den Book

---

## Delivery

- This doc covers interaction flows and component specs (Dex)
- Layout wireframes and visual hierarchy from Quill (separate doc)
- Both delivered to Flint before PR4 unblocks
