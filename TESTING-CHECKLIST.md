# Testing Checklist — verify each step on real hardware before moving on

Work through these IN ORDER. Don't let Claude Code race ahead to later features until the
earlier ones pass on the actual buzzers and (ideally) the actual projector. Check each box
as it passes. If something fails, fix it before continuing — bugs in early steps cascade.

---

## STEP 1 — TSV parser
- [ ] App loads `QuizShow/Animals.tsv` without error.
- [ ] Board shows 5 categories × 5 clues from that file.
- [ ] Category name reads from the header row ("Animals").
- [ ] Loads `BibleBooks.tsv` and correctly shows a DIFFERENT size (4 categories × 2 clues) with different per-column category names.
- [ ] The truncated answer ("...two young pige") loads without crashing.
- [ ] Smart quotes / apostrophes (Doesn't, Lazarus') display correctly — no garbled characters.
- [ ] A file with a trailing blank line still loads fine.

## STEP 2 — Board view + buzz-in + lockout  ← TEST WITH REAL BUZZERS
- [ ] Clicking a dollar tile reveals the clue full-screen.
- [ ] After reveal, pressing a buzzer registers a buzz-in and shows WHICH player (big number).
- [ ] **Test all 10 buzzers** — each one registers as its correct number.
- [ ] Confirm buzzer 10's key (is it "0" or "10"?). Note the answer here: __________
- [ ] First buzz LOCKS OUT the others — a second buzzer press does nothing until reset.
- [ ] Buzz-in plays a sound and is clearly visible across the room.
*** STOP HERE and confirm with the host before building further. ***

## STEP 3 — Scoring
- [ ] Correct adds the clue's value to the right player.
- [ ] Wrong SUBTRACTS the clue's value (real-Jeopardy scoring).
- [ ] After Wrong, buzzers RE-OPEN to everyone EXCEPT the player who already missed.
- [ ] If everyone misses / no one buzzes, host can reveal answer and close with no further change.
- [ ] Correct gives that player "control" to pick the next clue.
- [ ] A used clue is marked done and can't be re-selected.

## STEP 4 — Setup flow (mode, board build, values, timers)
- [ ] Can choose Individual vs Team mode before the game.
- [ ] Can pick categories from the folder and MIX columns from different files into one board.
- [ ] Can set/edit point values (default 200–1000).
- [ ] Can set the buzz-in timer and answer timer durations; can disable timers.
- [ ] Timeout after buzzing counts as Wrong automatically.

## STEP 5 — Teams
- [ ] Can assign buzzers to teams (e.g. 1–3 = Team A, 4–6 = Team B).
- [ ] First buzz from any team member buzzes in the whole team.
- [ ] Lockout applies to the whole team for that clue.
- [ ] Team names and colors are editable and show on the score area.

## STEP 6 — Daily Doubles + wagering
- [ ] Host can mark cells as Daily Doubles in setup (and/or randomize).
- [ ] Selecting a Daily Double = only the controlling player/team plays it (no buzzing).
- [ ] Player enters a wager (capped at their score or a set max).
- [ ] Correct adds the wager; Wrong subtracts it.

## STEP 7 — Buzzer test screen
- [ ] A pre-game screen shows buzzers 1–10.
- [ ] Pressing each buzzer lights up its number AND shows the raw key received.
- [ ] Use this to confirm every buzzer before guests arrive.

## STEP 8 — Board editor + TSV export
- [ ] Can edit category names, questions, answers, and values in-app.
- [ ] Can fix the truncated turtledove answer and save.
- [ ] Can attach an image or audio file to a clue.
- [ ] Export produces a valid TSV in the SAME format (reload it to confirm round-trip).

## STEP 9 — Media clues
- [ ] A clue tagged `[img:somefile.png]` shows the image on reveal.
- [ ] A clue tagged `[audio:somefile.mp3]` plays the audio, with a replay button.
- [ ] A missing media file shows the text + a "not found" note (no crash).

## STEP 10 — Final Jeopardy (toggleable)
- [ ] Final Jeopardy can be turned OFF entirely and the game ends normally.
- [ ] When ON: shows category, host enters each player/team wager, reveals clue, marks each correct/wrong, applies wagers.

## STEP 11 — Branding + sound + projector polish
- [ ] Church logo appears on intro, board, Final, and winner screens.
- [ ] Brand colors (slate gray / brand blue) are used throughout — not generic Jeopardy colors.
- [ ] All sounds work and are toggleable / volume-adjustable.
- [ ] Board is readable when projected on the actual TV/projector from across the room.
- [ ] Manual score adjustment works (add/subtract points for any player/team).

---

## FINAL DRESS REHEARSAL (do this BEFORE a live event)
- [ ] Plug in the receiver on the GAME computer (not a different Mac).
- [ ] Run the buzzer test screen — all 10 buzzers confirmed working.
- [ ] Project onto the real screen; confirm readability and that nothing is cut off.
- [ ] Play one full short game start to finish with a couple of volunteers.
- [ ] Confirm sounds are audible through the room's speakers.
- [ ] Save/back up your question files and any edits.
