# Ellington & Vale — Email Template

## What changed in this build

Two real bugs, found by testing in the Gmail app, are fixed here.

### 1. The Lookbook no longer uses a tap-to-switch toggle

The original version relied on a CSS trick — hidden radio inputs plus a rule
that hides three of four images until one is selected. Gmail's app does not
reliably apply that hiding rule, so instead of switching between images, it
was showing all four at once, stacked with no control over it.

The fix: the Lookbook is now an intentional stacked sequence. Every look
shows, full width, in order, with its caption underneath. This is close to
what Gmail was already rendering by accident — now it's the actual design,
properly spaced, and it looks identical in every client because nothing
depends on an interaction that might not run.

The same reasoning was applied to the "note" module, which previously had
an open/close toggle. It's now always shown open.

### 2. Color — Gmail's forced dark mode

Gmail's Android and iOS apps run their own automatic dark-mode pass that
rewrites colors, and it does this even when the file explicitly says
`<meta name="color-scheme" content="light">`. Neither of the two standard
defenses reach it: `prefers-color-scheme` is inconsistently honoured in
Gmail's app, and `[data-ogsc]` only helps Outlook.com.

Two real fixes are used instead:

- **`bgcolor` as an HTML attribute**, not just CSS `background`, on every
  color-critical table cell. This measurably reduces how often Gmail
  overrides the color — it is not a guarantee, but it's the standard
  first line of defense.
- **The Practice band is now a flattened image** (`img/practice-band.jpg`)
  instead of live CSS-colored text. This was the section that broke worst
  in testing — it flipped from dark background/cream text to light
  background/dark text. Since Gmail repaints CSS backgrounds and text
  color but does not repaint pixels inside an image, this section is now
  immune. The tradeoff: to edit that copy, you regenerate the image
  rather than editing text directly.

## Folder structure

```
ellington-vale-email.html   ← the email itself
img/
  hero-collective.jpg       ← full-bleed hero
  look-ivory.jpg            ← Lookbook, look 01
  look-oxblood.jpg          ← Lookbook, look 02
  look-charcoal.jpg         ← Lookbook, look 03
  look-leather.jpg          ← Lookbook, look 04
  practice-band.jpg         ← flattened "Five disciplines" section
```

## GitHub Pages — confirmed

This build is already set for your repo:

- **Username:** `gauravmarketmaverick-cyber`
- **Repo:** `ellington-vale-email`

Every image source in `ellington-vale-email.html` already points to:

```
https://gauravmarketmaverick-cyber.github.io/ellington-vale-email/img/...
```

No find-and-replace needed — this is ready to push as-is.

**Steps, in order:**
1. Push everything in this folder — the HTML, this README, and the
   entire `img/` folder — preserving the structure above, to the
   `ellington-vale-email` repo.
2. Enable GitHub Pages for the repo (Settings → Pages → deploy from
   the branch you pushed to), if not already on.
3. Confirm each image loads by opening
   `https://gauravmarketmaverick-cyber.github.io/ellington-vale-email/img/hero-collective.jpg`
   directly in a browser before running any send test. A blank page or
   a 404 there means Pages hasn't finished deploying yet, or the repo
   is set to private — Pages needs the repo public (or GitHub Pro/Team
   for a private Pages site) to serve images to an email client.


## Before this goes to an actual send

- Every `{{PLACEHOLDER}}` needs real copy — search the file for `{{` to
  find them all.
- For production sending (not just a GitHub Pages preview), images
  generally need to live on your actual sending domain, not GitHub Pages —
  some ESPs and corporate mail filters treat GitHub-hosted images as a
  spam signal. GitHub Pages is fine for reviewing the design and for
  script-based testing; move the `img/` folder to your own domain before
  an actual campaign, and update the six image URLs the same way this
  file's URLs were set for Pages.
- `practice-band.jpg` was generated at 600×320. If you change the copy,
  regenerate at the same dimensions so it drops in cleanly.
