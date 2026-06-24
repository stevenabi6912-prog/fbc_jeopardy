# Build Prompt: Buzzer Jeopardy Game

Build a **Jeopardy-style trivia game** that runs locally on a Mac and reads physical quiz buzzers.
Read `QUESTION-FORMAT.md` and the example file `QuizShow/Animals.tsv` before writing the parser — the app MUST load the user's existing trivia library in that exact format without reformatting.

## Hardware / input model (IMPORTANT — this is how buzzing works)

- The user has 10 physical "Affordable Buzzers" wireless buzzers + a USB receiver.
- The receiver acts as a **plain USB HID keyboard**. Pressing buzzer N types the **number key N** (buzzer 1 → "1", buzzer 2 → "2" … buzzer 10 → "0" or "10"; confirm: key "0" most likely = buzzer 10).
- So buzz-in detection = **listening for number keypresses 1,2,3,4,5,6,7,8,9,0**. No drivers, no serial, no special API. Just keyboard events.
- This must work even when the host screen isn't a text field — capture keydown globally on the game page.

## Platform / architecture

- Build a **local web app** the host runs in a browser on the Mac. Strongly prefer this over a native app (easy to project, easy to restyle, easy to edit questions).
- Keep setup dead simple: a single command to start (e.g. a small local server), or openable as static files if feasible. Document exactly how to run it in a README.
- Two views:
  1. **Board view** — big, projector-friendly. Shows the category grid with dollar values, the revealed clue full-screen, current scores, and a giant "PLAYER N BUZZED IN" indicator.
  2. **Host control view** — buzz status, Correct/Wrong buttons, score adjustments, navigation, game setup. Can be the same screen with host controls, or a second window/panel — host's choice, but make it usable by one person running the game.

## Core game rules (locked spec)

- **Board:** category columns × clue rows, **size auto-detected from the file(s)**, with a manual override at setup to trim categories/rows.
- **Point values:** default **200 / 400 / 600 / 800 / 1000** by row, **editable** before a game. (If more/fewer rows, scale sensibly and let host edit.)
- **Buzz-in:** host reveals a clue → **buzzers are open immediately** → **first valid number key wins** and **locks out all others**. Show who buzzed (big number + sound).
- **Host judges:** Correct or Wrong buttons.
  - **Correct:** add clue value to that player/team; clue closes; control returns to board; that player/team picks next (track "control of the board").
  - **Wrong:** **subtract the clue value** from that player/team (real-Jeopardy scoring), then **re-open buzzers to everyone EXCEPT players/teams who already missed this clue.** If everyone misses or no one buzzes, host can reveal the answer and close the clue with no score change to remaining players.
- **Timers:** per-clue countdown (host-configurable seconds) for how long players have to buzz after reveal, and a separate answer-countdown after buzzing. Auto-scoring ties to these (timeout on a buzzed player = treated as wrong). Make timer durations configurable; allow disabling.
- **Modes (switchable at setup):**
  - **Individual:** up to 10 players, one per buzzer.
  - **Team:** host assigns buzzers to teams (e.g., buzzers 1–3 = Team A). First valid buzz from any buzzer on a team buzzes in that team; lockout applies to the whole team for that clue. Team names + colors editable.
- **Daily Doubles:** host can mark specific cells as Daily Doubles during setup (or randomize a couple). When a Daily Double clue is selected, only the controlling player/team plays it; they enter a **wager** (up to their current score, or a configurable max) before the clue is revealed; correct adds the wager, wrong subtracts it. No buzzing on a Daily Double.
- **Final Jeopardy (TOGGLEABLE — host can turn it OFF entirely):** if enabled, after the main board: show a single category, every player/team secretly writes a wager (host enters each), reveal the clue, host marks each player/team correct/wrong and applies their wager ±. Since buzzers only send numbers, Final is **host-run / paper-based** (players write answers on paper; host enters rulings). Make it cleanly skippable.

## Required extra features for v1

1. **Folder-based question loading + mix/match.** Scan a `questions/` folder, list all `.tsv` files, let the host build a board by picking individual categories (columns) from any file(s) and combining them. This is the #1 feature for reusing the existing library.
2. **Built-in board editor.** Edit category names, questions, answers, and values in-app; fix truncated/typo'd cells; **export back to the same TSV format** (see QUESTION-FORMAT.md). Also allow creating a new set from scratch with arbitrary rows/columns.
3. **Buzzer test screen.** A pre-game screen showing buzzers 1–10; pressing each lights up its number so the host can confirm all 10 work before guests arrive. Show the raw key received (helps confirm buzzer 10 mapping).

## Media clues (images + audio)

- Clues may include images or audio (like real Jeopardy picture/audio clues). See QUESTION-FORMAT.md for the `[img:...]` / `[audio:...]` tag syntax and the `media/` folder convention.
- On reveal: display the referenced image prominently on the board view; for audio, play it with host play/replay controls.
- Existing text-only files must keep working untouched — media is optional per clue.
- The board editor must let the host attach/clear image or audio per clue and write the correct tag on TSV export.
- Missing media file = show text + a small "media not found" note; never crash.

## Branding (REQUIRED)

- The game must be visually themed for **Faith Baptist Church of Chelsea** per `THEME.md`.
- Use the church logo (`assets/fbc-logo.png`) on intro/setup, board (small), Final Jeopardy think screen, and winner screen.
- Use the exact brand palette in THEME.md (slate gray `#616a70`, brand blue `#0093ce`, deep blue `#008fcc`, light gray `#bdc2c9`, white). Replace the generic Jeopardy blue-and-gold with this palette.
- Projector-first: large fonts, high contrast, readable across a room.

## Sound + polish

- Sound effects: buzz-in sound, correct ding, wrong buzzer, time's-up, Daily Double reveal, and optional background/think music (Final Jeopardy think music especially). Use simple bundled audio or web-audio tones; make sounds toggleable and volume-adjustable.
- Clean, readable, high-contrast board styling that looks good projected on a TV/projector. Big fonts. Classic Jeopardy blue-and-gold is a fine default but keep it themeable.

## Host quality-of-life

- **Manual score adjustment:** host can add/subtract points from any player/team at any time (for rulings/disputes).
- **Undo last action** if reasonable.
- **Game state safety:** don't lose scores on accidental refresh if you can persist in-memory/session reasonably (note: if this is a static file with no backend, use in-page state; do NOT rely on anything that breaks projection).
- Clear setup flow: choose mode → choose/build board → set values/timers/Daily Doubles → (optional) configure teams → start.

## Deliverables

- Working app, runnable on macOS with clear README instructions (how to install/run, where to put `.tsv` files, how the buzzers map).
- The parser must successfully load `QuizShow/Animals.tsv` and any same-format file of arbitrary size.
- Keep the codebase organized so it can be extended later (separate concerns: parsing, game state, UI/views, audio).
- Include a couple of sample `.tsv` files (use the provided Animals.tsv; add 1–2 more dummy sets so mix/match can be demonstrated).

## Build order suggestion

1. TSV parser + sample load (prove it reads Animals.tsv correctly, any size).
2. Board view + clue reveal + keyboard buzz-in + lockout.
3. Scoring (correct/wrong, subtract-on-wrong, re-open to non-missed).
4. Setup flow: mode, board build (folder pick/mix), values, timers.
5. Teams.
6. Daily Doubles + wagering.
7. Buzzer test screen.
8. Board editor + TSV export.
9. Final Jeopardy (toggleable).
10. Sound + projector polish + README.

Confirm the buzzer-10 key mapping early (is it "0" or does it send "10"?) via the buzzer test screen.
