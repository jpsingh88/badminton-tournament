# 🏸 Badminton Tournament

A single-file web app to run a **6-player badminton doubles tournament** on one court in about two hours. Enter six names, tap the winner of each match, and the app handles the schedule, points, seeding, and the final. It runs entirely in the browser — no internet needed once the page has loaded — so it works at courtside on any phone.

---

## What it does

- Runs a fixed, pre-balanced **9-match league** followed by a **seeded final**.
- Every player plays 6 matches and rests 3 — automatically enforced by the schedule.
- One tap records a match winner; points and standings update instantly.
- Shows live standings any time, seeds the final for you, and crowns the champions.
- Keeps a running **match history** — every completed match with the winning pair, the losing pair, and who rested.
- Auto-saves progress, so a locked phone or an accidental refresh won't lose the tournament.

---

## The format

**6 players, doubles (2 vs 2), 2 players rest each match.**

Players are entered in order and labelled **A–F** to match the schedule below.

### League schedule (9 matches)

| Match | Team 1 | Team 2 | Resting |
|:-----:|:------:|:------:|:-------:|
| 1 | A + B | D + E | C, F |
| 2 | A + F | C + E | B, D |
| 3 | B + E | C + D | A, F |
| 4 | A + C | D + F | B, E |
| 5 | B + D | E + F | A, C |
| 6 | A + E | B + C | D, F |
| 7 | A + D | C + F | B, E |
| 8 | B + E | D + F | A, C |
| 9 | A + C | B + F | D, E |

### Scoring

- Each player on the **winning** team gets **2 points**.
- Losers get **0**.
- Play each match to **11 points**, or use a **10-minute cap** to stay inside two hours.

### The final

After 9 matches, players are ranked by total points. The final is:

> **Seed 1 + Seed 4  vs  Seed 2 + Seed 3**

This keeps the final balanced instead of stacking the two best players together. **The pair that wins the final wins the tournament** (the winner-takes-all format).

**Tiebreaks for seeding:** if players are level on points, the app ranks them by **head-to-head** result first, then by a **coin flip** if still tied — so the final teams are never ambiguous.

### Why it's fair (verified)

- Each player plays 6 and rests exactly 3.
- All 15 possible partner pairings occur; only three repeat once — **A+C, B+E, D+F**.
- Every player faces every other player 2–3 times.

---

## How to use the app

1. **Enter names.** Type all six players (in your preferred A–F order) and tap **Start Tournament**.
2. **Play a match.** The screen shows Team 1, Team 2, and who's resting. Play it on court, then **tap the card of the team that won**. The app moves to the next match automatically.
3. **Check standings any time.** Tap **Standings ▾** at the top to expand the live leaderboard.
4. **Review past matches.** A **Match history** panel builds up below as you go — each completed match shows the winning pair (in green), the pair they beat, and who rested. It's visible at any time and appears again on the final results screen.
5. **Fix a mistake.** Tap **↩︎ Undo last result** to reverse the previous match (it's removed from the history too).
6. **Running short on time?** Tap **⏭ Go to final now** to skip remaining league matches and seed the final from matches played so far.
7. **The final.** After match 9 the app shows the seeding and the final matchup. Tap **Play the Final**, then tap the winning team.
8. **Champions.** The winners are crowned, with the full league table and match history shown below.
9. **New event.** Tap **New Tournament** to start over.

The 2-hour clock at the top is a **guide only** — it counts down and turns red in the last 10 minutes, but it won't end the tournament on its own. You control the pace.

---

## Put it online (GitHub Pages)

Host it free so any phone can open it from a link:

1. Create a new repository (e.g. `badminton-tournament`) and set it to **Public**.
2. Upload `index.html` to the repo — **keep the filename `index.html`**. (You can also add this `README.md`.)
3. Go to **Settings → Pages**.
4. Under **Source**, choose **Deploy from a branch**, select branch **`main`** and folder **`/ (root)`**, then **Save**.
5. Wait about a minute. Your app will be live at:

   ```
   https://<your-username>.github.io/badminton-tournament/
   ```

6. Open that URL on any phone. For a full-screen, app-like feel, use the browser's **Add to Home Screen** option.

---

## Run it without GitHub

You don't need to host it at all — the file is self-contained:

- **On a phone:** send yourself the `index.html` file (email, AirDrop, messaging) and open it in your browser.
- **On a computer:** double-click `index.html` to open it in any browser.

---

## Files

| File | Purpose |
|------|---------|
| `index.html` | The entire app — HTML, CSS, and JavaScript in one file. This is all you need. |
| `README.md` | This file. |

---

## Technical notes

- **No build step, no dependencies, no backend.** Plain HTML/CSS/JavaScript.
- **Works offline** after the first load.
- **Progress is saved** in the browser's local storage, so refreshing or locking the phone mid-tournament prompts you to resume.
- **Data is local to the device.** Clearing browser data or using a different phone starts fresh.

---

## Customising the final

The final is set to **winner-takes-all** (the final decides the champion; the two lowest seeds sit it out). An alternative is to award **+4 points to each final winner and add it to the league total**, so every match counts toward the result. If you'd prefer that version — or a setup toggle to choose on the day — it's a small change to make.
