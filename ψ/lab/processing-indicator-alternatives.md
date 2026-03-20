# Beast Card Processing Indicator — Design Alternatives

**Current**: Full card breathes green (`#3fb950` at 12% opacity), 2s pulse with green box-shadow glow.

**Problem**: Green implies "success/ready" — misleading for a "working/busy" state. Full-card animation is heavy.

---

## Option A: Border Accent Pulse

Instead of the whole card glowing, only the left border animates. Subtle, contained, unmistakable.

```css
.card.processing {
  border-left: 3px solid transparent;
  animation: borderPulse 1.5s ease-in-out infinite;
}

@keyframes borderPulse {
  0%, 100% {
    border-left-color: rgba(139, 92, 246, 0.3);
  }
  50% {
    border-left-color: rgba(139, 92, 246, 0.9);
  }
}
```

- Uses purple (#8b5cf6) — neutral "active" color, not success/error
- Only left border animates — card stays calm
- Faster cycle (1.5s) feels more "working"

---

## Option B: Shimmer Line

A thin animated gradient line runs across the top of the card — like a progress bar that doesn't know the progress. Familiar pattern from loading states.

```css
.card.processing::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: linear-gradient(90deg, transparent, #a78bfa, transparent);
  animation: shimmer 1.5s ease-in-out infinite;
}

@keyframes shimmer {
  0% { transform: translateX(-100%); }
  100% { transform: translateX(100%); }
}
```

- Familiar "loading" pattern — users instantly understand it
- Minimal visual weight — doesn't distract from card content
- Purple gradient for consistency

---

## Option C: Dot Spinner in Status Badge

Replace the status text with a three-dot loading animation. The card itself doesn't animate at all — only the status indicator.

```css
.statusProcessing::after {
  content: '';
  display: inline-block;
  width: 4px;
  height: 4px;
  border-radius: 50%;
  background: #a78bfa;
  animation: dotPulse 1s ease-in-out infinite;
  box-shadow: 8px 0 #a78bfa, 16px 0 #a78bfa;
}

@keyframes dotPulse {
  0%, 100% { opacity: 0.3; }
  50% { opacity: 1; }
}
```

- Zero card distraction — only the badge animates
- Clear "working" signal without color confusion
- Works at any card size

---

## Option D: Soft Amber Pulse (Recolor Only)

Keep the existing pulse pattern but change green to amber — "busy/working" is universally amber.

```css
@keyframes cardPulse {
  0%, 100% {
    background: var(--bg-card);
    box-shadow: none;
  }
  50% {
    background: rgba(245, 158, 11, 0.08);
    box-shadow: 0 0 8px rgba(245, 158, 11, 0.15);
  }
}
```

- Smallest change — just swap the color
- Amber = "busy/processing" (traffic light convention)
- Reduced opacity (0.08 vs 0.12) for subtlety

---

## Recommendation

**Option B (Shimmer Line)** is the strongest choice:
- Universally recognized loading pattern
- Minimal visual noise — doesn't compete with card content
- Scales well (works on mobile, doesn't feel heavy)
- Purple aligns with design system, avoids green/red/amber semantics

**Option A (Border Pulse)** is the runner-up — elegant and contained.

Present all 4 to Gorn for decision.
