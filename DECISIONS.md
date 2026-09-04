# Decisions & Process Notes

Running log of decisions made during the challenge. High-level only — no
prompt-by-prompt tracking (the full AI transcript is attached separately).

## Setup

- **Tools:** Working in VS Code with Claude Code (Fable 5) as an AI pair,
  the way I normally work. The full unedited transcript will be included
  with the submission per the instructions.

## Part 1 — FAQ accordion bug

- **Diagnosis:** `toggleFaq()` used `classList.add('open')` on both the
  question button and its answer. `add()` is one-way — nothing ever removed
  the class, so re-clicking an open item couldn't close it and opening a new
  item left the previous one open. That matches all three reported symptoms.
- **Fix:** Record whether the clicked item was already open, close *all*
  `.faq-q.open` / `.faq-a.open` elements, then re-open the clicked item only
  if it wasn't the one already open. This gives true accordion behavior:
  one item open at a time, and clicking the open item closes it.
- **Why this approach:** Closing everything first (instead of toggling just
  the clicked item) handles both symptoms with one code path and can never
  leave the page in a multiple-open state, even if markup changes later.
  Kept the existing style (vanilla JS, class toggling, inline `onclick`)
  rather than rewriting to `<details>` or event delegation — smallest
  correct change wins for a bug fix.

## Part 2 — feature

- **Choice:** Dark/light mode toggle. Considered form validation (too small),
  a plan-picker quiz (more product-y but harder to polish in the time), and
  dark mode. Picked dark mode because the site's CSS-variable foundation makes
  a *clean* implementation possible — and doing it right requires a semantic
  token refactor, which shows CSS architecture rather than a bolted-on feature.
- **Semantic tokens over raw palette:** Components hardcoded `#fff` and raw
  palette vars (`--slate-500` etc.). Introduced intent-based tokens
  (`--bg-page`, `--bg-surface`, `--text-main`, `--text-muted`, `--border`, …)
  so the entire dark theme is one `[data-theme="dark"]` override block instead
  of hundreds of scattered dark-mode rules.
- **`data-theme` on `<html>` + head script:** Theme is applied before first
  paint by a tiny inline script in `<head>` to avoid a flash of the wrong
  theme. Saved `localStorage` choice wins; otherwise `prefers-color-scheme`,
  with a live `matchMedia` listener while no explicit choice exists.
- **Left the already-dark sections alone:** Testimonials, footer, newsletter
  box, and the stats gradient are dark in both themes by design. Made the dark
  page background slightly darker than slate-900 so those sections still read
  as distinct surfaces, and gave them subtle borders in dark mode.
- **Transitions:** 0.3s background/color/border cross-fade on themable
  elements. Cards that already had `transition: transform ...` got the color
  properties appended to their own declaration rather than a blanket rule, so
  a later rule wouldn't clobber their hover transitions.
- **Found pre-existing bugs:** CSS referenced `--slate-400` and `--sky-300`,
  neither of which was defined in `:root` (declarations silently fell back to
  inherited values). Defined both while in there.
- **Verification:** Checked tag balance and ran `node --check` on both script
  blocks; visually verified in the browser in both themes.
