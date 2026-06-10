# Saved It! 💸

A tiny, just-for-fun PWA for logging money you *didn't* spend. Saw a jacket, talked yourself out of it? Open the app, tap **+ Save**, type `89` and `Jacket`, get confetti. No accounts, no bank, no servers — everything stays on your device.

## Features

- **Running total** with a celebratory count-up animation
- **+ Save** flow designed to take under 5 seconds (2 taps + typing)
- **Confetti** on every save 🎉
- **History** with relative dates, swipe-left or long-press to delete (with Undo)
- **Streaks** — current and best consecutive-day runs
- **Monthly stats** — this month, best month, 12-month bar chart
- **Export / import** JSON backups (merge or replace)
- **Currency** picker (defaults to your locale), `Intl.NumberFormat` formatting
- **Offline-first**: service worker caches the whole app shell; data lives in IndexedDB (localStorage fallback) with a durable-storage request on first save
- Dark mode, safe-area insets, springy animations — feels like an app, not a webpage

## Files

```
saved-it/
  index.html      app shell + all CSS/JS (no build step, no dependencies)
  manifest.json   PWA manifest (standalone, theme color, icons)
  sw.js           cache-first service worker
  icon-192.png    app icon
  icon-512.png    app icon (also used as maskable)
  README.md       this file
```

## Hosting

It's a static site — any static host works. Service workers require **HTTPS or localhost**, so don't open `index.html` via `file://` if you want install/offline support.

### Quickest: local server

```bash
cd saved-it
python -m http.server 8000
# then visit http://localhost:8000
```

(Or `npx serve`, `php -S localhost:8000`, etc.)

### On the internet

Drop the `saved-it/` folder onto any static host:

- **GitHub Pages**: push the folder to a repo, enable Pages in repo settings.
- **Netlify / Vercel / Cloudflare Pages**: drag-and-drop the folder or connect the repo. No build command needed; publish directory is the folder itself.

All of these serve over HTTPS, which is what you need to install on a phone.

## Install to home screen

> Tip: to use it on your phone, host it (see above) and open the URL in your phone's browser first.

### Android (Chrome)

1. Open the app URL in Chrome.
2. Tap the **⋮** menu (top right).
3. Tap **Add to Home screen** (or **Install app** if Chrome offers the install banner).
4. Confirm. The "Saved It!" icon appears on your home screen and launches fullscreen (standalone) with a splash screen.

### iOS (Safari)

1. Open the app URL in Safari (must be Safari — other iOS browsers can't install PWAs).
2. Tap the **Share** button (square with an up arrow).
3. Scroll down and tap **Add to Home Screen**.
4. Tap **Add**. The icon appears on your home screen and opens without browser chrome.

After the first load, the app works fully offline.

## Backups

Your data only exists on the device/browser where you logged it. Use **Settings → Export backup** to download a JSON file now and then, and **Import backup** to restore it (choose *merge* or *replace*). The exported file looks like:

```json
{
  "app": "saved-it",
  "version": 1,
  "exportedAt": "2026-06-10T12:00:00.000Z",
  "currency": "USD",
  "entries": [
    { "id": "abc123", "amount": 89, "label": "Jacket", "ts": 1765400000000 }
  ]
}
```

## Notes

- On first save the app calls `navigator.storage.persist()` to request durable storage; the result shows under **Settings → Storage**.
- Out of scope by design: accounts, sync, real money, budgets, notifications. It's a feel-good tracker, not a bank.
