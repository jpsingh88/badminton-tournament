# Going online with Firebase — setup & hosting guide

This turns the tournament app from single-device into a **shared, real-time app**: any player opens a link on their own phone, anyone can enter results, and everyone sees updates live. It also unlocks the **all-time leaderboard** across tournaments.

**How it fits together:** the app page stays hosted on **GitHub Pages** (your existing URL). **Firebase (Firestore)** is used only as the shared cloud database. You do a one-time ~10-minute Firebase setup, paste 6 values into the app, and re-upload it.

> **Before setup:** the app already works. With no Firebase config it runs in **Offline mode** (badge at the top says "○ Offline") — fine for a single scorer on one phone. Everything below switches it to **Online mode** ("● Online (synced)").

---

## Part 1 — Create the Firebase project (one time)

1. Go to **https://console.firebase.google.com** and sign in with a Google account.
2. Click **Add project** (or **Create a project**). Give it a name like `badminton-tournament`. You can **disable Google Analytics** — not needed. Click **Create project**, wait, then **Continue**.

## Part 2 — Register a Web App and get your config

3. On the project home, click the **web icon** `</>` ("Add app" → Web).
4. Give it a nickname (e.g. `badminton-web`). **Do NOT** tick "Firebase Hosting" (we're hosting on GitHub Pages). Click **Register app**.
5. Firebase shows a code snippet containing a `firebaseConfig` object. **Copy the 6 values** — you'll need these:

   ```js
   const firebaseConfig = {
     apiKey: "AIza…",
     authDomain: "your-project.firebaseapp.com",
     projectId: "your-project",
     storageBucket: "your-project.appspot.com",
     messagingSenderId: "1234567890",
     appId: "1:1234567890:web:abcdef…"
   };
   ```
   (If you closed it: **Project settings** ⚙️ → **General** → scroll to **Your apps** → **SDK setup and configuration** → **Config**.)

## Part 3 — Create the database

6. Left menu → **Build → Firestore Database** → **Create database**.
7. Choose a **location** (pick the region closest to you) → **Next**.
8. Start in **Production mode** (we'll set exact rules next) → **Create**.

## Part 4 — Set the security rules

9. In Firestore, open the **Rules** tab, replace everything with the block below, and click **Publish**:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /tournaments/{id} {
         allow read, write: if true;
       }
       match /meta/{doc} {
         allow read, write: if true;
       }
     }
   }
   ```

   > **Important:** the `meta/{doc}` block is required for the **player roster** (dropdown, name merges, and removals) to save. If it's missing, tournaments still work but every merge/removal is lost on reload. If you set up Firebase before this line existed, go back to **Firestore → Rules**, add the `meta` block, and **Publish**.

   **What this means (read this):** these rules let *anyone who has your project details* read and write the `tournaments` data. That's the simplest setup and is fine for a casual friends' app where the only "secret" is the tournament link. The trade-off is there's **no login and no protection** against someone deliberately tampering if they get your config. For a badminton night that's an acceptable trade. If you ever want it locked down, that needs Firebase Authentication added later — ask and I'll wire it up.

## Part 5 — Paste your config into the app

10. Open `index.html` in a text editor. Near the top of the `<script>` you'll find:

    ```js
    var FIREBASE_CONFIG = {
      apiKey: "",
      authDomain: "",
      projectId: "",
      storageBucket: "",
      messagingSenderId: "",
      appId: ""
    };
    ```

11. Fill in the 6 values you copied in Part 2 (keep the quotes). Save the file.

That's it — the app is now online-capable.

---

## Part 6 — Host it (GitHub Pages)

You're keeping GitHub Pages, so this is the same as before:

1. In your `badminton-tournament` repo, upload the updated `index.html` (replace the old one). Keep the filename `index.html`.
2. If Pages isn't on yet: **Settings → Pages → Source: Deploy from a branch → `main` / `/ (root)` → Save**.
3. Wait ~1 minute. Your app is live at `https://<your-username>.github.io/badminton-tournament/`.

Open it — the badge at the top should read **● Online (synced)**. If it says Offline, the config wasn't filled in correctly (see Troubleshooting).

---

## How to use it live

- **Start:** open the app → **New Tournament** → enter 6 names → **Start Tournament**. This creates the tournament in the cloud and puts a `?t=CODE` on the URL.
- **Share:** tap **🔗 Share** on the match screen to copy/send the link. Anyone who opens that link joins the *same* tournament and sees it live.
- **Score from any phone:** anyone with the link can enter a match result. Within a second it appears on everyone's screen (points, standings, history all update).
- **Leaderboard:** from the Home screen → **🏅 All-time Leaderboard**. It scans every completed tournament and ranks players by **total match wins** (with tournament titles 🏆 and events played shown alongside). Players are matched **by name**, so keep spellings consistent (e.g. always "Yash", not "yash Y").

**A note on concurrent editing:** since anyone can edit, if two people record the *same* match at the *same second*, the last save wins. In practice you record one match at a time, so this is a non-issue — just avoid two people entering different results simultaneously.

---

## Storing Tournament #1 (the one you already played)

Your first tournament was run on the old logic and its details are in your head/notes, not the app. Two ways to get it into the leaderboard:

1. **Easiest:** once the online app is live, open it and **replay** Tournament 1 — enter the 6 names and tap through the 9 results (and the final) as they actually happened. It'll be saved as a completed tournament and counted in the leaderboard.
2. **Manual:** send me the full details (names, each match's teams + winner + score, the tie/knockout, and the final result) and I'll format it as a database record you can import directly. Note the old tournament used a different fixture, so option 1 (replaying results) is cleaner for leaderboard counting.

---

## Cost

Firebase's free **Spark** plan is far more than enough for this: 50,000 reads and 20,000 writes per day. A full tournament is a few dozen writes. You will not pay anything.

---

## Troubleshooting

- **Badge says "○ Offline" after adding config:** double-check all 6 values are pasted with quotes and no typos, especially `projectId`. Hard-refresh the page.
- **"Missing or insufficient permissions":** the Firestore rules in Part 4 weren't published, or were pasted wrong. Re-publish them.
- **Changes don't sync to another phone:** confirm both phones opened the *same* `?t=CODE` link and both show "● Online". Check you have internet.
- **Leaderboard empty:** it only counts **completed** tournaments (through the final). Finish one first.
- **Want to wipe test data:** Firestore console → **Data** tab → delete documents in the `tournaments` collection.
