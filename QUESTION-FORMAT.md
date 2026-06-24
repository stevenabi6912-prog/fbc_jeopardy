# Question File Format (TSV)

The app must load the user's EXISTING trivia files without requiring any reformatting.
This document describes that exact format. A real example file is in `sample-questions/Animals.tsv`.

## Structure

- **Tab-separated values (`.tsv`)**, UTF-8 encoding (files contain curly quotes/apostrophes — handle as UTF-8, never show mojibake).
- **Each COLUMN is a category.** The number of columns = number of categories on the board.
- **Row 0 (first line) = category header row.** The category name appears in each column.
  - NOTE: In the sample, the same category name ("Animals") is repeated across all 5 columns. The app must read category names FROM THE HEADER ROW per column, not assume columns differ. Different files may have different names per column.
- **After the header, rows alternate: question row, answer row, question row, answer row, ...**
  - Odd data-rows (file rows 1,3,5,7,9...) = QUESTIONS
  - Even data-rows (file rows 2,4,6,8,10...) = ANSWERS
- Number of clues per category = (total_rows - 1) / 2.

## Board size is variable

The sample is 5 categories × 5 clues, but the app MUST auto-detect:
- categories = number of columns in the header row
- clues per category = (number of rows - 1) / 2

The app must also let the host OVERRIDE/trim this at game setup (e.g., load a 6-column file but play only 5 categories, or play only the top 4 clue rows).

## Real-world data quirks the parser MUST tolerate

These appear in the user's actual library. The app must not crash or corrupt on any of them:

1. **Truncated / cut-off answers.** e.g. one answer reads `A pair of turtledoves or two young pige` (cut off mid-word). Load as-is; the built-in board editor lets the host fix it. Do not crash.
2. **Leading/trailing whitespace** in cells (e.g. ` A pair of...`, `Lions `). Trim for display but preserve on export.
3. **Smart quotes / curly apostrophes** (’ “ ”) and ampersands (&). Render correctly.
4. **Ragged rows possible.** Some rows may have fewer cells than the header. Treat missing cells as empty, do not crash.
5. **Trailing empty line** at end of file is common.

## Export

The built-in board editor must export back to this SAME tsv format (tab-separated, header row, alternating Q/A rows, UTF-8) so the user's library stays consistent and round-trips cleanly.

## Optional media in clues (images / audio)

Real Jeopardy has picture and audio clues. Support this WITHOUT breaking the plain-text format
or the user's existing text-only files.

Approach: a question (or answer) cell may optionally contain a media tag at the start:

- `[img:filename.png] Rest of the question text...`
- `[audio:filename.mp3] Rest of the question text...`

Rules:
- Media files live in a `media/` folder next to the questions folder. The tag references the filename only.
- When a clue with `[img:...]` is revealed, show the image prominently on the board view (alongside or above the clue text).
- When a clue with `[audio:...]` is revealed, play the audio (with host play/replay control).
- A cell with NO tag is plain text and behaves exactly as today — existing files are 100% unaffected.
- The board editor must let the host attach/clear an image or audio file per clue and write the correct tag on export.
- Tolerate a missing media file gracefully (show the text, note the file wasn't found; do not crash).

## Folder loading

The user has a LARGE library of these .tsv files (one category-set per file, or multiple categories per file).
- The app should scan a `questions/` folder and list all `.tsv` files.
- The host can pick which file(s) to play, and **mix and match individual columns/categories from different files into one board** (e.g., column "Animals" from one file + "Bible Geography" from another).
