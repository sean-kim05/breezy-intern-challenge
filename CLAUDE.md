# Intern Code Challenge — Click Here Labs

Coding challenge for the Intern Developer position at Click Here Labs.
Candidate: Sean Kim (skim8705@gmail.com). Due within 7 days of 2026-09-04.

## The task

- **Part 1 (done):** Fix the broken FAQ accordion in `breezy-intern-test.html` —
  the `toggleFaq()` function only ever *added* the `open` class, so items never
  closed. Fixed to close all items first, then toggle the clicked one.
- **Part 2 (done):** Dark/light mode toggle — semantic CSS tokens
  (`--bg-page`, `--text-main`, etc.) overridden via `[data-theme="dark"]` on
  `<html>`, localStorage persistence, `prefers-color-scheme` default, flash
  prevention via head script. Documented in `README.md`.

## Project layout

- `breezy-intern-test.html` — the site being worked on. Self-contained single-page
  parody landing page ("Breezy", subscription air). All CSS/JS inline; no build
  tools or server — open directly in a browser.
- `Intern Code Test Instructions.docx` — the original challenge instructions.
- `DECISIONS.md` — running log of decisions and process notes (part of the
  submission; keep it updated as we go, but don't log individual prompts).

## Conventions & constraints

- Keep the site self-contained (inline CSS/JS) unless a feature genuinely needs
  separate files — the challenge values simplicity and "no setup required".
- Match the existing code style: plain vanilla JS functions, `onclick` handlers
  in markup, CSS custom properties in `:root`, `.open` class toggling.
- Submission requires process notes per part and unedited AI transcripts —
  this Claude Code conversation transcript will be attached to the submission.
- Note: the file contains leftover WordPress-ish markup (`<article id="post-2">`
  wrapper, "Home" entry-title). Not part of the bug; leave unless there's a
  reason to clean it up.
