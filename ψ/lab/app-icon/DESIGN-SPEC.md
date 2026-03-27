# Den Book App Icon — Design Spec

**Designer**: Dex
**Date**: 2026-03-27
**Status**: Draft v1 — awaiting Gorn review

## Concept

The Den paw mark — the pack's universal symbol — on the signature dark charcoal background. Warm amber gradient gives the paw warmth and depth, consistent with Den Book's UI accent color.

## Design Decisions

| Decision | Rationale |
|----------|-----------|
| Paw print symbol | Already The Den's identity (favicon, branding). Instantly recognizable. |
| Amber on charcoal | Matches Den Book theme exactly (`--accent: #d4943a` on `--bg-primary: #111110`) |
| No text/wordmark | App icons should be symbolic — text doesn't scale to small sizes |
| Subtle depth (v2) | Drop shadow + highlight layer gives the paw a pressed/embossed feel |
| Centered slightly above geometric center | Optical centering — paw looks more natural slightly high |

## Files

| File | Purpose |
|------|---------|
| `den-app-icon-v1.svg` | Clean flat version — paw with amber gradient on charcoal |
| `den-app-icon-v2.svg` | Polished version — adds drop shadow + highlight for depth |
| `den-app-icon-adaptive-foreground.svg` | Android adaptive icon foreground (paw only, 108dp) |
| `den-app-icon-adaptive-background.svg` | Android adaptive icon background (solid charcoal, 108dp) |
| `den-notification-icon.svg` | Android notification icon (24dp, white monochrome silhouette) |

## Colors Used

| Token | Hex | Usage |
|-------|-----|-------|
| `--bg-primary` | `#111110` | Background base |
| `--bg-secondary` | `#1a1917` / `#1e1c1a` | Background gradient center |
| `--accent` | `#d4943a` | Paw mid-tone |
| `--accent-hover` | `#c07d2a` / `#b07028` | Paw shadow edge |
| Light amber | `#e8a94a` / `#f0b45a` | Paw highlight |

## iOS Requirements

- **Master**: 1024×1024px (exported from SVG)
- iOS applies rounded-rect mask automatically — no need to bake corners
- Required sizes generated from master: 180×180 (60pt @3x), 120×120 (60pt @2x), 167×167 (83.5pt @2x iPad Pro), 152×152 (76pt @2x iPad)
- App Store icon: 1024×1024 (no transparency, no alpha)

## Android Requirements

- **Adaptive icon**: 108×108dp, safe zone 66×66dp centered
- Foreground: paw only (transparent background)
- Background: solid `#111110`
- Legacy: 48×48dp (mdpi), scaled to hdpi/xhdpi/xxhdpi/xxxhdpi

## Variants to Consider

1. **Light mode variant** — charcoal paw on cream/warm-white (if we add light mode)
2. **Notification icon** — white silhouette only (Android requires monochrome for status bar)
3. **Splash screen icon** — larger paw with "The Den" wordmark below (Quill is handling splash)

## Delivered

- [x] `den-notification-icon.svg` — 24×24dp monochrome white paw silhouette (Android status bar)

## Next Steps

- [ ] Gorn reviews and picks v1 or v2 (or requests changes) — queued via Sable
- [ ] Export PNG at all required sizes
- [ ] Hand off to Karo/Rax for Capacitor integration
