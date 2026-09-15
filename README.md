# NamesApp

A small mobile app for keeping track of parents' names and contact info across the different parts of your kids' lives — school, sports, volunteering, or anything else you add. Contacts can have multiple parents, multiple children, any number of categories, and a tag for which family member they connect to.

## Using it on your iPhone

The fastest way to use this is the hosted version already published for you:
open the link Claude gave you in Safari, then tap the Share icon and choose **Add to Home Screen**. It opens full-screen like a native app, works offline, and your entries are saved automatically.

## Running it yourself

This repo is a self-contained static site — there's no build step or server required.

- **Open directly**: double-click `index.html` (or serve the folder with any static file server) and it works right away, saving data to the browser's local storage on that device.
- **Host it** (e.g. GitHub Pages, Netlify, Vercel): deploy this folder as-is. Once it's live, visit the URL on your iPhone in Safari and use **Add to Home Screen** for an app-like icon, full-screen view, and offline support via the included service worker.

By default, data saved this way lives only in that browser's local storage — it won't show up on another device or for another person. Set up Firebase (below) to share one shared list of contacts between devices and people.

## Sharing contacts with someone else (Firebase setup)

NamesApp can sync contacts through your own free Firebase project instead of saving them only on one device. Once set up, you and anyone else you share the config with see the same live list.

### 1. Add a web app to your Firebase project

1. Open the [Firebase console](https://console.firebase.google.com) and select your project.
2. Click the gear icon → **Project settings**, scroll to **Your apps**, and click the web icon (`</>`) to add a web app (any nickname is fine; you don't need Firebase Hosting).
3. Firebase shows you a `firebaseConfig` object with `apiKey`, `authDomain`, `projectId`, `storageBucket`, `messagingSenderId`, and `appId`.

### 2. Paste your config into `index.html`

Open `index.html` and find `FIREBASE_CONFIG` near the top of the `<script>` block:

```js
var FIREBASE_CONFIG = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

Replace the placeholder values with the ones from your Firebase console. As soon as a real `apiKey` is in place, the app uses Firebase automatically — no other code changes needed.

### 3. Turn on Firestore

1. In the Firebase console, open **Build → Firestore Database** → **Create database**.
2. Choose **Start in production mode** (the security rules below handle access) and pick any location close to you.

### 4. Turn on Email/Password sign-in and create your accounts

NamesApp shows a sign-in screen and only lets in the specific people you create accounts for — nobody else, even if they find the URL.

1. Open **Build → Authentication → Sign-in method**.
2. Enable the **Email/Password** provider.
3. Go to the **Users** tab (still under Authentication) → **Add user**. Add one entry for yourself and one for your wife, each with an email and a password you choose. These don't need to be real inboxes — they're just credentials for this app.

### 5. Set Firestore security rules

Open **Build → Firestore Database → Rules** and use, with your two real emails from step 4 in place of the placeholders:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null
        && request.auth.token.email in ["you@example.com", "wife@example.com"];
    }
  }
}
```

This is what actually keeps the data private: only someone signed in as one of those two email addresses can read or write anything, regardless of who has the URL or has seen the Firebase config in `index.html`. Click **Publish** after editing.

### 6. Share it

Host `index.html` (see "Running it yourself" above) and send the URL to your wife along with the login you created for her in step 4. Opening the URL shows a sign-in screen; after signing in, you both see and edit the same live list of contacts and categories. Signing out (from the pencil icon → Categories → Sign out) returns to that screen.

## What it does

- Add one or more parents and one or more children per contact, along with phone, email, and notes.
- Tag a contact with any number of categories (School, Sports, Volunteering, or ones you add yourself) — add or remove categories any time from the pencil icon next to the filter tabs.
- Tag a contact with which family member (Sara, Ben, Connor, or Cris) it's associated with.
- Filter by category with the tabs at the top, or search across everything.
- Tap the phone or email icon on a contact to call or email them directly.
- Tap a contact to edit or delete it.
- Works fully offline; syncs through Firebase when configured, otherwise saves to the device only.

## Files

- `index.html` — the whole app (markup, styles, and logic in one file).
- `manifest.json` — makes the app installable on a phone's home screen.
- `service-worker.js` — caches the app shell so it works offline.
- `icons/` — home screen icons at the sizes iOS and Android expect.
