← [Back to the README](../README.md)

# Media

How the public assets for this showcase are organized, and what is still being
captured.

| Folder | Holds | Status |
|--------|-------|--------|
| [`brand/`](brand/) | Logo and the Pip wordmark | ✅ Final |
| [`screenshots/`](screenshots/) | Warm-paper phone mockups of the core flows | ✅ Placeholder mockups (real captures pending) |
| `app/` | Real in-app captures (light + dark), framed | 🚧 Capturing for the next build |
| `widgets/` | Home-screen + Lock-Screen WidgetKit family | 🚧 Capturing |
| `web/` | Desktop and tablet web (`bupples.web.app/app`) | 🚧 Capturing |
| `demo/` | ~30s screen-recorded walkthrough (GIF / MP4) | 🚧 Recording |

## Screenshots in this repo

The four files in [`screenshots/`](screenshots/) are hand-built, on-brand
**warm-paper mockups** (SVG) of the core screens — the live bubble field, add
expense, settle up, and invite. They stand in until device-framed captures of
the shipping build are ready, at which point they will be replaced with real
PNGs under `app/`.

## Capture checklist

Real media to add before App Store launch, in priority order:

- [ ] **Demo GIF** — create a session → scan a receipt → everyone claims their
      items → settle up (the one artifact recruiters watch first).
- [ ] **Receipt claiming** — the live, tap-to-claim receipt with balances moving.
- [ ] **Multiple payers** — an expense split across more than one payer.
- [ ] **Turbo split** — snap → claim → who-owes-whom.
- [ ] **Settle Up** — the personal Summary / Full view toggle.
- [ ] **Bubble field** — light and dark, with Pip reacting to balance.
- [ ] **iOS widgets** — Small / Medium / Large + Lock-Screen accessories.
- [ ] **Web** — desktop and tablet layouts.

Captures should be device-framed (not raw simulator grabs) and shown in both
light and dark where the screen differs.
