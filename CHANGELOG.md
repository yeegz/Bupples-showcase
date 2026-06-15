# Changelog

All notable changes to **Bupples**, newest first.

Bupples is pre-launch: it iterates through TestFlight at marketing version
`1.0.0`, and the **build number** (`1.0.0+N`) advances each release. Build 11 was
skipped in the version sequence (10 → 12).

---

## Build 15 — `1.0.0+15`

The collaborative-receipt release, plus a broad feel + performance pass.

### Added
- **Collaborative receipts in a session.** A normal hangout can now split a bill
  the way Turbo does, but *together and in real time*: the person who paid uploads
  a receipt (scan or type it), **reviews and confirms** the scanned items, then
  **everyone claims the items they had live** by tapping the row. When the payer
  is happy, they **add it to the balances** as one exact-split expense (tax,
  service and discount included). It appears as a receipt object in the session
  with a clear status — *Review → Claiming → Settled*.
- **"Everyone's paid" cleanup nudge.** Each person can mark their share paid; once
  everyone has, the receipt owner (and, for Turbo, the host) gets a push letting
  them **delete the now-settled receipt** — its expense and breakdown stay in the
  session log if they ever need it again.
- **Receipt-uploaded push + deep links.** Confirming a receipt notifies the other
  members; tapping any receipt notification opens straight to it.
- **Onboarding theme + currency picker.** First-run setup gains a searchable
  **currency dropdown** (with a highlighted "Custom" option) and an **accent-colour**
  step, applied live, alongside the existing light/dark choice.

### Changed
- **Receipt flow reworked.** The old "owner drags each person's bubble onto their
  items" screen is **replaced** by the collaborative, real-time, tap-to-claim flow
  above — so claiming is everyone's job, not the owner's.
- **Smarter scanning.** The receipt scanner now also reads the **store/merchant
  name** (used to auto-title the receipt, e.g. *"McDonald's Receipt"*, rename-able
  anytime) and any **discount / promo** amount, which is split across claimants in
  proportion to what they had.
- **Whole-row tap to claim** in Turbo and receipts — the checkbox stays as a
  visual, but the entire item row is the tap target.

### Feel
- **iMessage / Mail-style swipe actions** on home cards — a controlled, fixed-width
  reveal of icon actions (Logs · Leave · Delete) that snaps open, closes on scroll,
  and keeps only one card open at a time.
- **Larger, social-app-style swipe** between tabs, plus tap-anywhere-to-dismiss
  the keyboard.

### Performance
- **Account-scoped avatar cache.** Profile photos are cached on-device so they
  load instantly and stop re-downloading, namespaced per account and wiped on
  sign-out so one account never sees another's images.
- **Battery-friendly animations.** Every animation across the app pauses the
  moment it is no longer in the foreground (app switcher, Control Centre,
  background) and resumes cleanly on return.

---

## Build 14 — `1.0.0+14`

Reset correctness, itemising, and the first swipe gestures.

### Fixed
- **Sign-out / account-deletion now fully reset the device immediately** — no more
  stale sessions lingering on Home until a force-quit.
- **Instant reopen** — the app seeds from its local cache and refreshes session
  membership from the cloud in the background, so it never sits on the splash.

### Added
- **Itemised "by item" expenses** in normal sessions, with a manual entry path.
- **Turbo scan-mismatch warning** when a scanned total doesn't equal items + tax +
  service.
- **Explainer cards** for settle-up, Turbo, and how splitting works.
- **Swipe-left card actions**, **edge-swipe between tabs**, and **iOS swipe-back**
  on pushed screens.

### Changed
- **One searchable currency picker** used everywhere a currency is chosen.

---

## Build 13 — `1.0.0+13`

Cross-device restore, image safety, and Turbo polish.

### Added
- **Restore your sessions from the cloud** after signing in (not just from the
  local cache), so the account's hangouts follow you to a new device.
- A **branded reboot/splash** during reset and sign-in.

### Fixed
- **Image-format validation on upload** with a graceful avatar fallback — fixes
  photos that displayed for the uploader but broke for everyone else.

### Changed
- **Deleting a Turbo split safely archives it** — records, receipts and who-paid
  stay intact; financial proof is never erased.
- **Editable tax & service** in Turbo, with an explainer of how they ride
  proportionally.

---

## Build 12 — `1.0.0+12`

The screen-redesign release.

### Changed
- Redesigned **add-expense** (clean cards + progressive disclosure), **new-hangout**
  (currency up front, grouped rules), and **settle-up** (one unified plan, single
  avatars).
- Your **profile photo is always stored to your account**; the share toggle only
  controls whether *others* see it.

### Fixed
- Hardened the **sign-out / delete reset** path so it can't be aborted by an
  unrelated failure.

---

## Build 10 — `1.0.0+10`

### Added
- **Profile photos fill your member bubble**, with the name on a photo-derived
  blur strip; profile-photo controls collapse into a tap-the-avatar popup.

### Fixed
- Complete **device-local reset** on account deletion / sign-out, plus bubble-photo
  hardening.

---

## Builds 2–9 — `1.0.0+2` … `1.0.0+9`

Foundation and early TestFlight polish — the core product:

- **Live bubble field** home, **sessions** (join by short code or scannable QR),
  and **flexible expenses** (equal · exact · percentage · shares, with tax &
  service and a 60-second undo).
- **Currency-aware balances** (never summing different currencies) and **smart
  settle-up** (minimal who-pays-whom debt simplification, request → confirm → undo,
  with optional receipt-photo proof).
- **Turbo receipt splits** with currency auto-detected from the scan, and the
  **no-app web claim** flow so friends without the app can claim items from a
  browser.
- **Transfer details** ("how to pay me"), **push notifications**, **mutual
  renames**, **host controls** (transferable ownership, kick/ban, archive-not-erase).
- **Accounts** — anonymous by default, Continue with Google or Sign in with Apple,
  and privacy-preserving account deletion (anonymise, never purge shared records).
- The **warm-paper** design system and the **Pip** mascot, plus accessibility
  (haptics, reduce-motion) and App Store compliance groundwork.

---

_Bupples · © 2026 Yousof Selim · All Rights Reserved_
