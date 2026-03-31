# Body Comp Trend Chart v2 — Concept Options

**Date**: 2026-03-31
**Status**: Draft concepts for Gorn review
**Context**: Body composition tracking needs a trend chart with metric selector toggles and health-direction color coding

## Shared Design Principles

- **Health-direction coloring**: Colors reflect whether movement is healthy, not just up/down
  - Body fat % going DOWN → green (healthy direction)
  - Muscle mass going UP → green (healthy direction)
  - Reverse directions → red/amber (concerning)
  - Stable/flat → neutral gray
- **Metric toggles**: User picks which metrics overlay on the same chart
- **Time range**: 7d / 30d / 90d / 1y selector
- **Responsive**: Works on mobile and desktop

---

## Option A: Sparkline Stack

Separate mini-charts stacked vertically, one per metric. Each sparkline is self-contained — its own scale, its own trend arrow, its own color.

```
┌─────────────────────────────────────────────┐
│  Body Fat %          ▼ 18.2%  (-0.8)  🟢   │
│  ╌╌╌╌╲___╱╲__╲_____                        │
│                                              │
│  Muscle Mass (kg)    ▲ 72.4   (+1.2)  🟢   │
│  ____╱‾╲__╱‾‾‾╱‾‾‾‾                        │
│                                              │
│  Weight (kg)         ▲ 88.1   (+0.4)  ⚪   │
│  ╌╌╌╌╌╌╌╌─────────‾                        │
│                                              │
│  [✓] Body Fat  [✓] Muscle  [✓] Weight       │
│  [ ] BMI       [ ] Water %                  │
│                                              │
│  7d  [30d]  90d  1y                         │
└─────────────────────────────────────────────┘
```

**Pros**: Each metric has its own Y-axis — no scale conflict. Very scannable.
**Cons**: Takes vertical space. Relationships between metrics less visible.
**Best for**: Quick daily check — "am I trending right?"

---

## Option B: Unified Overlay

All selected metrics on one chart with dual Y-axes. Lines are color-coded per metric with health-direction fill gradients.

```
┌─────────────────────────────────────────────┐
│  Body Composition Trends                     │
│                                              │
│ kg                                       %   │
│ 90┤                                    ┤20   │
│   │    ╱‾╲        ●━━muscle━━●         │     │
│ 80┤   ╱   ╲__╱‾‾‾            (green)  ┤18   │
│   │  ╱                                 │     │
│ 70┤ ╱    ╲                    ●━fat━●  ┤16   │
│   │       ‾╲___╱╲___╲____    (green)   │     │
│ 60┤                                    ┤14   │
│   ├────┬────┬────┬────┬────┬────┤      │     │
│   Mar 1    8    15    22    29          │     │
│                                              │
│  [✓] Body Fat  [✓] Muscle  [ ] Weight       │
│  7d  [30d]  90d  1y                         │
└─────────────────────────────────────────────┘
```

**Gradient fills**: Area under muscle line has a green-to-transparent gradient (trending up = healthy). Area under fat line has green-to-transparent from the opposite direction (trending down = healthy).

**Pros**: See correlations — muscle up while fat down is immediately visible as crossing lines.
**Cons**: Dual axes can confuse. More than 2-3 metrics gets cluttered.
**Best for**: Understanding relationships between metrics over time.

---

## Option C: Hybrid — Summary Cards + Detail Chart

Top section has metric cards with trend indicators. Tapping a card opens/focuses that metric in the chart below.

```
┌─────────────────────────────────────────────┐
│                                              │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐    │
│  │ Fat %    │ │ Muscle   │ │ Weight   │    │
│  │ 18.2%  🟢│ │ 72.4kg 🟢│ │ 88.1kg ⚪│    │
│  │ ▼ 0.8    │ │ ▲ 1.2    │ │ ▲ 0.4    │    │
│  │ ╲___╲__  │ │ _╱‾‾╱‾‾  │ │ ───‾‾‾  │    │
│  └──────────┘ └──────────┘ └──────────┘    │
│                                              │
│  ┌─ Detail: Body Fat % ─────────────────┐   │
│  │ 20% ┤                                │   │
│  │     │  ╲                              │   │
│  │ 18% ┤   ╲___╱╲__╲_____              │   │
│  │     │                                 │   │
│  │ 16% ┤                                │   │
│  │     ├────┬────┬────┬────┤            │   │
│  │     Mar 1   8    15   22             │   │
│  └───────────────────────────────────────┘   │
│                                              │
│  7d  [30d]  90d  1y                         │
└─────────────────────────────────────────────┘
```

**Pros**: Summary at a glance + deep dive on tap. Cards act as toggles. Works well on mobile.
**Cons**: More complex to implement. Chart shows one metric at a time by default.
**Best for**: Mobile-first usage where screen space is tight.

---

## Health-Direction Color Logic

| Metric | Healthy Direction | Green | Red/Amber | Neutral |
|--------|-------------------|-------|-----------|---------|
| Body fat % | Down | Decreasing | Increasing | Stable (±0.2%) |
| Muscle mass | Up | Increasing | Decreasing | Stable (±0.3kg) |
| Weight | Context-dependent | — | — | Always neutral* |
| Water % | Up | Increasing | Decreasing | Stable |
| BMI | Down (if overweight) | Decreasing | Increasing | In range |

*Weight is neutral by default because gaining weight while gaining muscle is healthy. Color only applies if user sets a weight goal.

### Color Tokens

```
--health-positive: #22c55e  (green-500)
--health-negative: #ef4444  (red-500)
--health-caution:  #f59e0b  (amber-500)
--health-neutral:  #6b7280  (gray-500)
--trend-up-arrow:  inherits from health direction
--trend-down-arrow: inherits from health direction
```

---

## My Recommendation

**Option C (Hybrid)** for mobile-first usage, but this is Gorn's call. Each option has a clear use case:
- A for simplicity
- B for data relationships
- C for mobile + depth

Waiting for direction. All three can share the same color token system and health-direction logic.

— Dex
