# IronLog Android-ready fitness tracker (PWA)

## What works now
- Dark black + neon green mobile interface
- Workout log with sets, reps, kg and optional RPE
- Workout history, muscle-group filter, delete records
- Exercise library with original generated SVG exercise cards (illustrative icons, not instructional photos)
- Progress chart for estimated loaded volume
- Body-weight log and best recorded sets
- Custom workout splits; start a split to prefill a session
- Rest timer with vibration/alert where supported
- Local data storage and JSON export/import
- Installable PWA with a service worker for offline shell caching

## Run on your Android phone
### Easiest local preview
1. Download and extract `ironlog_android_pwa.zip`.
2. Open `index.html` in Chrome. Some PWA features require HTTPS or localhost, and cloud auth will not work from a `file://` URL.
3. Local tracking works in a browser that supports localStorage.

### Install it like an app (recommended)
Host this folder on an HTTPS static host (for example GitHub Pages, Netlify, or Firebase Hosting). Then on Android Chrome:
1. Open the hosted URL.
2. Open Chrome menu (⋮).
3. Tap **Install app** or **Add to Home screen**.
4. Launch IronLog from the home screen.

This produces an installable web app/PWA, not a signed native APK. A native APK can be packaged later with Android Studio/Capacitor.

## Enable real login and cloud backup (Firebase setup required)
Cloud auth is intentionally not enabled until you configure your own Firebase project. This prevents sending fitness records to an unknown account.

1. Create a Firebase project at https://console.firebase.google.com/
2. Add a Web app in Project settings and copy its Firebase config.
3. In `app.js`, replace the placeholder values in `FIREBASE_CONFIG`.
4. In Firebase Console → Authentication → Sign-in method, enable **Email/Password**.
5. Create a Firestore database.
6. Add Firestore rules like the following, after reviewing for your project:

```text
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /ironlogUsers/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

7. Host on HTTPS and test Create account → Back up to cloud → Restore from cloud.
8. Keep a JSON export as a separate backup.

Firebase config is public client configuration, but Firestore security rules are essential. Never put service-account private keys in this app. The current cloud backup is user-triggered, not automatic.

## Data and privacy
- Before Firebase is configured, records stay in this browser/device.
- Clearing browser site data can erase local records; export JSON regularly.
- Cloud sync only works after you set up your own Firebase project and rules.
- Exercise visuals are original stylized icons, not form coaching or medical advice.

## Project files
- `index.html` — interface
- `app.js` — workout logic, charts, timer, Firebase placeholders
- `manifest.webmanifest` — install metadata
- `sw.js` — offline app-shell caching
- `icon.svg` — app icon
