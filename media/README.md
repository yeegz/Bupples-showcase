← [Back to the README](../README.md)

# Media

What is in here, and what it is for.

| Folder | Holds | Status |
| --- | --- | --- |
| [`brand/`](brand/) | The Bupples logo and the Pip mark | ✅ Current, safe to use |
| [`archive-build-46-79/`](archive-build-46-79/) | Screenshots from the build 46-79 era | ⚠️ **Quarantined. Do not use.** |

## Why the screenshots are quarantined, not deleted

The app captures in `archive-build-46-79/` are real screenshots of a real build, and
they are kept as a record of that era. They must **not** be used to present Bupples,
because at `1.1.0+112` they no longer show the product and, more seriously, they
contradict rules the app now enforces in code:

- **They show a sign prefix on money** (`+RM84.00`). The current money law carries
  direction with an arrow, the colour and words. A `+`/`−` prefix is opt-in and is not
  the default.
- **They render figures in Fraunces**, the display serif. Money is now always JetBrains
  Mono and always tabular, so a column of amounts aligns and a changing digit does not
  shift its neighbours.
- **The widget shot renders the balance in white**, not mint. Mint means owed to you;
  the colour is the direction, and it is not decorative.
- **The accent throughout is a lilac** that predates the current dark-first
  glassmorphic brand and the mint / coral / gold money palette.
- **`web/home-web.png` shows a product that no longer exists.** The full Flutter web app
  was removed; `/app` now 302-redirects to the landing page. Bupples ships iOS and
  Android, and the only web surface left is the Turbo-claim page for friends without the
  app.

A screenshot that argues against [the money law this project documents](../docs/ARCHITECTURE.md#6-the-money-law)
is worse than no screenshot, so none of these are linked from the README.

## What the showcase uses instead

The README presents Bupples with **hand-made SVGs** in [`../assets/`](../assets/),
not screenshots. They stay accurate because they are drawn to the current design
tokens rather than captured from a build that keeps moving.

## If you want fresh captures

They need a real device and are not currently a blocker for anything. Should they be
made, the bar is: dark mode, current build, and every figure in JetBrains Mono with no
sign prefix and the correct mint / coral by direction. Anything that fails that check
belongs in the archive folder, not in the README.
