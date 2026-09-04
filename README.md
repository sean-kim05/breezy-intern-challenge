# Breezy — Intern Code Challenge Submission

Sean Kim · skim8705@gmail.com

Everything lives in one self-contained file: `breezy-intern-test.html`.
No build tools, no server, no setup — just open it in a browser.

- **Part 1:** FAQ accordion bug fix (see explanation below and in `DECISIONS.md`)
- **Part 2:** Dark mode / light mode toggle with smooth transitions

---

## Part 1 — FAQ accordion fix

**What was wrong:** `toggleFaq()` called `classList.add('open')` on the clicked
question and its answer. `add()` is one-way — nothing ever removed the class.
So re-clicking an open item couldn't close it, and opening a new item left the
previous one open, which is exactly the "everything stacks up" behavior.

**The fix:** The function now records whether the clicked item was already open,
closes *every* open question/answer, then re-opens the clicked one only if it
wasn't the one already open. Closing everything first means there is a single
code path that can never leave the page with two items open, and re-clicking an
open item naturally closes it. I kept the existing style (vanilla JS, class
toggling) rather than rewriting the component — smallest correct change wins
for a bug fix.

## Part 2 — Dark mode toggle

### What I built and why

A dark/light mode toggle (the 🌙/☀️ button in the top navigation) with:

- **Smooth transitions** — backgrounds, text, and borders cross-fade over 0.3s
  instead of snapping.
- **Persistence** — your choice is saved in `localStorage` and survives reloads.
- **System-preference aware** — first-time visitors get their OS theme
  (`prefers-color-scheme`), and if they haven't picked a theme explicitly, the
  page follows live OS theme changes.
- **No flash of wrong theme** — a tiny script in `<head>` applies the saved
  theme before the first paint.
- **Accessible** — the toggle is a real `<button>` with an `aria-label` that
  updates to describe what pressing it will do.

I chose this feature because the site's CSS was *almost* ready for it — colors
were already CSS custom properties — but not quite: many components hardcoded
`#fff` and referenced raw palette values directly. Doing dark mode properly
meant introducing a semantic token layer, which is the same refactor a real
production site would need. It let me demonstrate CSS architecture, not just
bolt on a feature.

### How it works technically

1. **Semantic tokens.** `:root` now defines intent-based variables
   (`--bg-page`, `--bg-surface`, `--bg-card`, `--text-main`, `--text-body`,
   `--text-muted`, `--border`, `--border-soft`) whose light values point at the
   existing palette. Components reference these instead of raw palette values
   or `#fff` literals.
2. **One override block.** `[data-theme="dark"]` on `<html>` re-points those
   same tokens at dark values. The entire theme switch is ~10 lines of CSS plus
   a handful of component-specific overrides (the hero's radial glow, badge
   accent color, borders on cards that sit on dark-in-both-themes sections).
3. **`color-scheme`** is set per theme so native UI (scrollbars, the email
   input) matches.
4. **JS** is three small functions in the site's existing style: `applyTheme()`
   sets `data-theme` and updates the button icon/label, `toggleTheme()` flips
   it and persists to `localStorage`, and a `matchMedia` listener follows OS
   changes when no explicit choice was saved. The head script runs before CSS
   paints to prevent theme flash.

Sections that were already dark (testimonials, footer, newsletter box, stats
gradient) are left alone — they work in both themes by design, which is also
why the dark page background is slightly darker than those sections, so they
still read as distinct surfaces.

### Setup

None. Open `breezy-intern-test.html` in a browser. To test the
system-preference behavior, clear `localStorage` (DevTools → Application →
Local Storage → delete `breezy-theme`) and change your OS theme.

### What I'd improve with more time

- **Feature-icon chips** keep their pastel backgrounds in dark mode; I'd add
  dark-tinted variants of each chip color.
- **Reduced motion** — respect `prefers-reduced-motion` by disabling the
  cross-fade transitions.
- **A small icon animation** on toggle (sun/moon morph) instead of swapping
  emoji.
- **Contrast audit** — run the dark palette through an automated WCAG checker
  rather than eyeballing it.

### Process notes

See `DECISIONS.md` for the running decision log for both parts. Work was done
in VS Code with Claude Code as an AI pair (the way I normally work); the full
unedited transcript is included with this submission. Two pre-existing quirks
surfaced during the refactor: the CSS referenced `--slate-400` and `--sky-300`,
which were never defined in `:root` (the declarations silently fell back).
Both are now defined.
