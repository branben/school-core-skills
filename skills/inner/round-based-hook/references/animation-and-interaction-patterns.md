# Animation and interaction patterns

## Easing curves

- **Spring (state changes, micro-interactions):** `cubic-bezier(0.34, 1.56, 0.64, 1)` — slight overshoot for playful, responsive feel
- **Standard ease-out (transitions, reveals):** `cubic-bezier(0.23, 1, 0.32, 1)` — smooth deceleration
- Avoid linear easing for UI motion — it feels mechanical

## Staggered reveals

For lists of items (checks, files, cards):
```css
.check { opacity: 0; transform: translateY(8px); }
.check.is-visible {
  opacity: 1;
  transform: translateY(0);
  transition: opacity 400ms, transform 400ms;
}
```
Apply via JS with increasing delays (0ms, 100ms, 200ms...).

## State feedback animations

- **Success/confirmation:** Green background slides in from left, fades to transparent. Use `@keyframes` with `background: var(--green-soft)` → `background: transparent`
- **Stamp flip:** Rotate + scale overshoot: `rotate(2deg) scale(1)` → `rotate(-4deg) scale(1.05)` → `rotate(-1deg) scale(1)` with spring easing
- **Callout exit:** Collapse via `max-height` + `opacity` + `padding` transition (not `display:none`) so it animates closed

## Mobile-first breakpoints

- **Desktop-first threshold:** `@media (max-width: 800px)`
- **Key mobile rules:**
  - Single-column reflow (grid → `grid-template-columns: 1fr`)
  - Touch targets ≥48px (`min-height: 48px` on buttons)
  - Bottom-sheet dialogs (`align-items: flex-end` on backdrop, `border-radius: 14px 14px 0 0`)
  - Full-width buttons and verify controls
  - Reduce font sizes proportionally
  - Remove horizontal overflow

## Dialog patterns

- **Backdrop:** `position: fixed; inset: 0;` with semi-transparent background + blur
- **Dialog:** `width: min(560px, 100%); max-height: calc(100vh - 32px); overflow-y: auto;`
- **Open animation:** Scale from `.98` to `1` + `translateY(8px)` to `translateY(0)` with spring easing
- **Close:** Escape key, backdrop click, explicit close button — all restore focus to trigger
- **Mobile:** Slide up from bottom (`align-items: flex-end`), full-width actions in column-reverse

## Accessibility

- **Skip link:** Fixed top-left, hidden until focused
- **Focus-visible:** 3px solid outline with offset on all interactive elements
- **Reduced motion:** `@media (prefers-reduced-motion: reduce)` disables all animations
- **ARIA:** Dialogs get `role="dialog"` + `aria-modal="true"` + `aria-labelledby`
- **Touch targets:** Minimum 44-48px on all buttons

## Prose quality checklist

Before delivering any prose:
1. Lead with the request/decision/conclusion
2. Replace abstract nouns with verbs
3. Delete filler phrases
4. Use concrete specifics (names, dates, numbers)
5. One main outcome per message
6. End when the work is done