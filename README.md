# Baby Log

A small phone-friendly web app for two parents to log a baby's poop, pee and feeds.

- **Poop** — day and time, color, notes. Shows the count for the last 24 hours, broken down by color.
- **Pee** — day and time, notes. Shows the count for the last 24 hours.
- **Feed** — tap **New feed**, then what it starts with: **Left**, **Right** or **Bottle** (formula or pumped
  breast milk, in oz or ml). Each further tap adds a timed step to the same feed. Tap **Finish feed** when done; the
  log then shows the total minutes and the minutes per side. A feed left unfinished closes itself 2 hours after
  it started, without an end time. **Correct this feed** and **Add a missed feed** open the full form.
- **Next feed** — the feed card shows when the next feed is due: 3 hours after the first step of the last feed.
  The interval can be changed in Settings and is shared by both phones.
- History grouped by day with per-day totals. Tap any entry to edit or delete it.
- Works offline; entries sync when the phone is back online.

No build step: it is plain HTML/JS (`index.html`, `sw.js`, `manifest.json`).

## 1. Put it online (GitHub Pages)

1. Repository **Settings → Pages**.
2. Source: **Deploy from a branch**, pick the branch that holds these files, folder `/ (root)`, Save.
3. After a minute the app is at `https://<your-username>.github.io/poop-tracker/`.

Pages on a *private* repository needs a paid GitHub plan; on the free plan the repository must be public.
The code contains no personal data either way — the log lives in your database, not in the repository.

On each phone open the link, then **Share → Add to Home Screen** so it behaves like an app.

## 2. Share the log between two phones (free, ~5 minutes)

Without this step each phone keeps its own separate log.

1. Go to <https://console.firebase.google.com>, **Add project** (Analytics not needed).
2. **Build → Realtime Database → Create database**, start in **locked mode**.
3. Open the **Rules** tab, replace the contents with the following and **Publish**:

   ```json
   {
     "rules": {
       "families": {
         "$code": { ".read": true, ".write": true }
       }
     }
   }
   ```

4. Copy the database URL from the **Data** tab (looks like `https://your-project-default-rtdb.firebaseio.com`).
5. In the app: **Settings**, type your name, paste the URL, **Save**.
6. **Settings → Copy invite link**, and send it to the other phone. Opening it connects that phone to the same log.

### How private is this?

The app generates a long random "family code" and stores the log under it. The rules above stop anyone from
listing the database, so only someone holding your invite link can read or change the log. Treat that link
like a password. There are no accounts or logins — a deliberate trade-off to keep it simple.

## Run locally

```sh
python3 -m http.server 8000
```

then open <http://localhost:8000>.
