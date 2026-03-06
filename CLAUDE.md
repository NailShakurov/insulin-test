# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running Locally

Open `index.html` directly in a browser, or serve it with a local server:

```bash
python -m http.server 8080
# then open http://localhost:8080
```

No build step or dependency installation is required.

## Architecture

This is a single-file static web application. Everything lives in `index.html`:

- **Lines 8–495**: CSS — dark theme using custom properties (`--bg`, `--surface`, `--accent`, etc.), responsive layout with grid/flexbox, and `fadeUp`/`fadeDown` keyframe animations.
- **Lines 497–586**: HTML structure — quiz container, progress bar, question/answer cards, and results panel.
- **Lines 588–782**: JavaScript — quiz state machine, 16 questions with 4 answer options each (0–3 points), scoring, risk-level classification, and Google Sheets result logging.

### Scoring Logic

Each of the 16 questions awards 0–3 points. Total range: 0–48.

| Score | Risk Level |
|-------|------------|
| 0–5   | Minimal    |
| 6–15  | Mild       |
| 16–30 | Moderate   |
| 31–48 | High       |

### Google Sheets Integration

Results are POSTed to a Google Apps Script endpoint (`SHEETS_URL` constant near the top of the `<script>` block). The payload includes timestamp, total score, risk level, and all 16 individual answers. The request uses `mode: 'no-cors'`.

To disable logging, remove or comment out the `fetch(SHEETS_URL, ...)` call inside `showResults()`.
