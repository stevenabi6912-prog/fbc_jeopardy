# Buzzer Jeopardy — Spec & Starter Repo
## Faith Baptist Church of Chelsea

Everything Claude Code needs to build a buzzer-driven, church-branded Jeopardy game.

## What to tell Claude Code

Point Claude Code at this folder and say:

> Read BUILD-PROMPT.md, QUESTION-FORMAT.md, and THEME.md, plus the files in
> QuizShow/ and assets/. Then build the app described in BUILD-PROMPT.md.
> Start with the TSV parser and prove it loads QuizShow/Animals.tsv
> correctly, then work through the build order. Stop after the parser and the
> board+buzz-in+lockout steps so I can test on real buzzer hardware before you
> continue. Ask me before any big architecture choice you're unsure about.

## Files

- `BUILD-PROMPT.md` — full feature spec / master prompt.
- `QUESTION-FORMAT.md` — exact format of the existing trivia files (+ optional image/audio tags).
- `THEME.md` — Faith Baptist Church branding: logo placement + exact brand colors.
- `TESTING-CHECKLIST.md` — step-by-step checklist to verify each feature in order on real hardware.
- `QuizShow/` — real + dummy `.tsv` files.
- `assets/fbc-logo.png` — church logo.
- `media/` — drop image/audio clue files here.

## Your buzzers

10 "Affordable Buzzers" + USB receiver. Receiver = plain USB keyboard.
Buzzer N types number key N. Confirmed: buzzer 1 types "1".
(Buzzer 10's key — "0" or "10" — confirm via the in-app buzzer test screen.)
