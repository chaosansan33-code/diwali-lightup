# 🪔 Light Up Diwali — NTHU Diwali 2026

A mobile-shake collaborative lamp-lighting game. Players see personal scores and rank; a projector shows the top ten, total energy, and progressively illuminated diyas.

## Files
- `index.html`: all three views, Firebase client, styling and animation.

## Step 1 — Create Firebase project
1. Visit https://console.firebase.google.com/ and create a project (e.g. `nthu-diwali-lightup`). Analytics is optional.
2. In **Project settings → General → Your apps**, register a **Web** app. Copy its `firebaseConfig` object.
3. Under **Build → Realtime Database**, create a database. Copy its `databaseURL` from the Realtime Database console. Choose a geographically suitable region.
4. Under **Realtime Database → Rules**, for a *brief private test only*, use:

```json
{
  "rules": {
    "rooms": {
      "$room": {
        ".read": true,
        ".write": true
      }
    }
  }
}
```

**SECURITY WARNING:** These rules permit *any internet visitor* to edit game scores, reset the room, or trigger the finale. The `HOST_PIN` in the HTML is not secure because visitors can view page source. **Do not use these test rules for the public event.** For public deployment, implement Firebase Authentication and enforce role-based rules, server-validated score increments, and host-only phase changes (e.g. with Cloud Functions / a trusted backend). The current project is a functional prototype, not production-secure.

## Step 2 — Edit config
Open `index.html` in a text editor. Near the beginning of `<script type="module">`, replace the placeholder fields in `firebaseConfig` with the exact values from Firebase. Keep `databaseURL` exact. Set `HOST_PIN` to a non-default string (convenience only, not security).

## Step 3 — Publish with GitHub Pages
1. Create a **public** GitHub repository (e.g. `diwali-lightup`).
2. Upload `index.html` at the repository root (not `index.html.html`).
3. **Settings → Pages → Build and deployment → Deploy from a branch → main / (root) → Save.**
4. Wait for the Pages URL: `https://YOUR_USERNAME.github.io/diwali-lightup/`.
5. Open on a phone over HTTPS. Motion access on iPhone requires tapping **Enable Motion** and granting permission.

## Three URLs
- **Player:** `https://YOUR_USERNAME.github.io/diwali-lightup/?room=diwali2026`
- **Projector:** `https://YOUR_USERNAME.github.io/diwali-lightup/?mode=host&room=diwali2026`
- **Host controls:** `https://YOUR_USERNAME.github.io/diwali-lightup/?mode=control&room=diwali2026&pin=YOUR_PIN`

The projector generates a QR code pointing to the player URL via an external QR-image service. If that service is blocked, create and print a QR code for the player URL separately.

## Suggested event workflow
1. On a laptop, open projector URL and connect to the venue display.
2. On another tab/device, open host controls. Let participants scan the projector QR and enter nicknames.
3. Once joined, players press **Enable Motion**. A tap option is available as backup.
4. Host presses **START 60s**. Players shake or tap for one minute.
5. The host can press **LIGHT UP** to trigger the finale, or let the timer end automatically.
6. **RESET** clears all players and scores. Ask everyone to rejoin after resetting.

## Important limitations / test checklist
- Test 5–10 devices first; test 100–120 concurrently before the event.
- Scores are uploaded about once per second, and leaderboard may lag a little.
- Avoid rapidly switching tabs or locking the phone: browsers may pause motion sensing or timers.
- A late-joining participant may join during the game, but should ideally join before START.
- iPhone Safari may require motion permission; Android behavior varies.
- The central lights are emoji-based and depend on the system's emoji font.
- `ENERGY_TARGET` is 18,000; adjust after testing. For 120 players, that's about 150 points each (15 valid shakes).
- Host control relies on client-side PIN and is NOT secure; the public event needs proper authentication and database rules.
- The scoreboard and aggregate energy are stored separately. Network interruption between writes can briefly make totals inconsistent.
- Server-side validation, rate limiting, robust rejoin/identity, and production security are future work.
