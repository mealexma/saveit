# Build Brief: "Saved It!" — Spontaneous Savings App

## Concept
A just-for-fun app for logging money you *didn't* spend. You see a jacket you like, decide not to buy it, open the app, tap save, enter "Jacket" and 89, done. No real money or bank involved — purely a feel-good tracker of resisted temptations.

## Core flow (must be fast)
1. Open app → home screen shows big running total + prominent "+ Save" button.
2. Tap "+ Save" → minimal form: amount (numeric keypad, autofocus) + label (text, optional but encouraged).
3. Tap confirm → entry saved, confetti burst, total animates up, back to home. Target: under 5 seconds from app open to saved.

## Features (v1)
- **Running total**: big, celebratory, top of home screen.
- **History**: scrollable list below total — label, amount, relative date ("today", "3d ago"). Swipe or long-press to delete (with undo or confirm).
- **Streaks**: consecutive days with ≥1 save; show current and best streak.
- **Confetti**: on every successful save (canvas-based, no heavy lib needed).
- **Monthly stats**: simple view — total saved this month, count of saves, best month, maybe a tiny bar chart per month (pure CSS/canvas, no chart lib).
- **Export/import**: one-tap JSON export (download file) and import (file picker, merge or replace). This is the backup safety net.

## Tech requirements
- **Single-page PWA**, vanilla HTML/CSS/JS. No frameworks, no build step, no CDN dependencies — fully offline-capable.
- **Storage**: IndexedDB (small wrapper, no library). On first save, call `navigator.storage.persist()` to request durable storage; surface the result subtly (e.g., a settings line "Storage: persistent ✓"). localStorage fallback if IndexedDB unavailable.
- **Service worker**: cache-first for the app shell so it works fully offline.
- **Manifest**: `manifest.json` with name "Saved It!", standalone display, theme color, icons (192 + 512 PNG; generate programmatically — e.g., a piggy bank or coin emoji/glyph on a rounded gradient square).
- **Splash**: derived from manifest (background_color + icon) for Android; add `apple-touch-icon` and iOS meta tags so it installs nicely on iPhone too.
- **Native feel**: mobile-first layout, `viewport-fit=cover` + safe-area insets, no zoom on input focus (font-size ≥16px), touch-friendly targets, subtle haptic-style animations, `overscroll-behavior: none`. Should feel like an app, not a webpage.
- **Currency**: default to user locale formatting via `Intl.NumberFormat`; currency selectable in settings.

## File layout (deliverables)
```
saved-it/
  index.html      (all CSS + JS inline is fine, or split — keep it simple)
  manifest.json
  sw.js
  icon-192.png
  icon-512.png
  README.md       (how to host: any static host or `python -m http.server`; how to install to home screen)
```

## Design direction
Playful but clean. Dark-mode friendly (respect `prefers-color-scheme`). One accent color (e.g., mint/green = money saved). Big rounded numbers, generous whitespace, spring-y micro-animations. The save moment should feel rewarding.

## Out of scope (v1)
Accounts, sync, real money, budgets, notifications, social features.

## Acceptance checklist
- [ ] Add a save in ≤3 taps + typing; confetti fires; total updates.
- [ ] Data survives reload and browser restart.
- [ ] Works fully offline after first load.
- [ ] Installable (valid manifest + SW + icons); standalone display.
- [ ] Export produces valid JSON; import restores it.
- [ ] Streak and monthly stats compute correctly across month/day boundaries.
- [ ] No console errors; works in Chrome and Safari mobile viewports.
