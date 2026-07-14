# 🏸 Badminton Tournament

A single-file web app to run a **6-player badminton doubles tournament** on one court in about two hours. Enter six names, record the score of each match, and the app handles the schedule, points, seeding, tie-breaks, and the final. It runs entirely in the browser — no internet needed once loaded — so it works at courtside on any phone.

> **Version 2.** Adds a rest-balanced schedule (no one plays 3 in a row), required score entry, point-differential tie-breaks, match timing, and a live match history. Online database + real-time multi-phone sync are planned for v3.

---

## What it does

- Runs a fixed, pre-balanced **9-match league** followed by a **seeded final**.
- Every player plays 6 and rests 3 — and **no one ever plays more than 2 matches in a row**.
- Records the **score of every match** (winner auto-detected); scores drive tie-breaks and show in history.
- Breaks ties fairly with **point differential**, falling back to a **1-vs-1 singles playoff** only when two players are still dead-even. No coin tosses.
- Times each match and runs a short **changeover countdown** between games.
- Shows a running **match history** with scores, durations, and who rested.
- Auto-saves progress, so a locked phone or refresh won't lose the tournament.

---

## The format

**6 players, doubles (2 vs 2), 2 players rest each match.** Players are entered in playing order and labelled **A–F**. The **first four names play Match 1; the last two (E & F) rest first.**

### League schedule (9 matches)

| Match | Team 1 | Team 2 | Resting |
|:-----:|:------:|:------:|:-------:|
| 1 | A + B | C + D | E, F |
| 2 | C + E | D + F | A, B |
| 3 | A + E | B + F | C, D |
| 4 | A + C | B + D | E, F |
| 5 | C + F | D + E | A, B |
| 6 | A + F | B + E | C, D |
| 7 | A + D | B + C | E, F |
| 8 | C + D | E + F | A, B |
| 9 | A + B | E + F | C, D |

This fixture is mathematically balanced: everyone plays 6 / rests 3, **max streak of 2 matches in a row for every player**, all 15 possible partner pairings occur (only 3 repeat), and everyone faces everyone 2–3 times.

### Scoring

- Enter both teams' scores each match (e.g. **21** and **18**). The higher score wins; equal scores are blocked (badminton can't tie).
- Each player on the **winning** team gets **2 points**; losers get 0.
- Play to 11 or 21 — the app just records whatever you enter.

### Ranking & tie-breaks

After 9 matches, players are ranked by **total points** (2 per win).

If players are **level on points** in a way that affects the final (e.g. tied around the seed 3/4/5 line), the app runs a **guided knockout** instead of deciding it by a formula:

- It names exactly who's tied and which seeds are at stake.
- It gives you a **coin-toss helper** (flip a coin, or pick a random extra player to make up a doubles four).
- You play the knockout however you like — doubles, singles, whatever — then **tap the tied players in their finishing order**.
- The app seeds the final from the order you enter and records it in the match history.

Ties that don't change anything (e.g. seeds 2 and 3, who partner each other anyway, or seeds 5 and 6) are left alone — no pointless extra matches. Because points land on even numbers, expect a knockout in most tournaments.

### The final

The top four seed the final:

> **Seed 1 + Seed 4  vs  Seed 2 + Seed 3**

This keeps the final balanced instead of stacking the two best players together. **The pair that wins the final wins the tournament** (winner-takes-all). The final score is recorded too.

---

## How to use the app

1. **Enter names** in playing order and tap **Start Tournament**.
2. **Play the match** shown, then enter both scores using the **+ / –** buttons or by typing, and tap **Record result**. The winner is detected automatically and points are awarded.
3. **Changeover.** A ~18-second countdown runs before the next match auto-starts — tap **Start now** to skip it. Each match is timed from start to when you record the result.
4. **Standings & history.** Tap **Standings ▾** for the live leaderboard (points + differential). The **Match history** panel below shows every completed match with scores, durations, and resters.
5. **Fix a mistake.** **↩︎ Undo last result** reverses the previous league match.
6. **Running short on time?** **⏭ Go to final now** skips remaining league matches and seeds the final from what's played.
7. **Tie-break knockout (only if needed).** If players are level on points where it affects the final, the app opens the knockout screen: use the coin-toss helper, play it out, and tap the finishing order. It seeds the final from that.
8. **The final.** After seeding, tap **Play the Final**, enter the score, and the champions are crowned with the full standings and history below.
9. **New event.** Tap **New Tournament** to start over.

The 2-hour clock at the top is a **guide** — it counts down and turns red in the last 10 minutes but won't end the tournament on its own.

---

## Put it online (GitHub Pages)

1. Create a **Public** repository (e.g. `badminton-tournament`).
2. Upload `index.html` — **keep the filename `index.html`** (you can add this `README.md` too).
3. **Settings → Pages → Source: Deploy from a branch → `main` / `/ (root)` → Save.**
4. Wait ~1 minute. Live at `https://<your-username>.github.io/badminton-tournament/`.
5. Open on any phone; use **Add to Home Screen** for a full-screen feel.

To update an existing site, just re-upload the new `index.html` over the old one.

---

## Run it without GitHub

The file is self-contained: email/AirDrop `index.html` to your phone and open it, or double-click it on a computer.

---

## Online mode (v3 — Firebase)

The app now supports an optional **online mode** with a shared cloud database, real-time multi-phone sync, and an all-time leaderboard:

- **Online database (Firebase Firestore):** results stored in the cloud, not just one device.
- **Real-time multi-phone sync:** any player opens a shared link and can enter results; everyone sees updates live.
- **All-time leaderboard:** ranks players by total match wins across every completed tournament (players matched by name).

Online mode is **off until you add Firebase config** — until then the app runs offline on a single device exactly as before. To enable it, follow **`FIREBASE-SETUP.md`** (about 10 minutes of Firebase console setup, then paste 6 values into `index.html`). Hosting stays on GitHub Pages.

---

## Files

| File | Purpose |
|------|---------|
| `index.html` | The entire app — HTML, CSS, and JavaScript in one file. |
| `README.md` | This file. |

---

## Technical notes

- No build step, no dependencies, no backend (v2). Plain HTML/CSS/JavaScript.
- Works offline after first load. Progress is saved in the browser and resumes on reload.
- Data is local to the device until the v3 backend lands.
