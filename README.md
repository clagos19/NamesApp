# Parent Roster

A small mobile app for keeping track of parents' names and contact info across the different parts of your kids' lives — school, sports, volunteering, or anything else you add.

## Using it on your iPhone

The fastest way to use this is the hosted version already published for you:
open the link Claude gave you in Safari, then tap the Share icon and choose **Add to Home Screen**. It opens full-screen like a native app, works offline, and your entries are saved automatically.

## Running it yourself

This repo is also a self-contained static site — there's no build step or server required.

- **Open directly**: double-click `index.html` (or serve the folder with any static file server) and it works right away, saving data to the browser's local storage on that device.
- **Host it** (e.g. GitHub Pages, Netlify, Vercel): deploy this folder as-is. Once it's live, visit the URL on your iPhone in Safari and use **Add to Home Screen** for an app-like icon, full-screen view, and offline support via the included service worker.

## What it does

- Add a parent's name, their child's name, an area (School, Sports, Volunteering, or a custom one you name), phone, email, and notes.
- Filter by area with the tabs at the top, or search across everything.
- Tap the phone or email icon on a contact to call or email them directly.
- Tap a contact to edit or delete it.
- Works fully offline; data stays on your device.

## Files

- `index.html` — the whole app (markup, styles, and logic in one file).
- `manifest.json` — makes the app installable on a phone's home screen.
- `service-worker.js` — caches the app shell so it works offline.
- `icons/` — home screen icons at the sizes iOS and Android expect.
