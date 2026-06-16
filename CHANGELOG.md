# Changelog

All notable changes to **Bupples**, newest first.

Bupples is pre-launch: it iterates through TestFlight at marketing version
`1.0.0`, and the **build number** (`1.0.0+N`) advances each release. Build 11 was
skipped in the version sequence (10 → 12).

---

## Build 17 — `1.0.0+17`

A web release and a hardening pass: Bupples now runs in the browser, the iOS
Control Center actions and widgets work as intended, and settle-up
notifications are exact down to the item.

### Added
- **Bupples on the web.** The full app now runs in a browser at
  *bupples.web.app/app* — sign in, see your hangouts and balances, join a normal
  session or a Turbo split from a link, and settle up. A shared hangout link
  opens the app if you have it, and otherwise hands off cleanly into the web
  version (the app is still the better experience, and the site says so).
  Protected with Firebase App Check (reCAPTCHA) like the mobile app.
- **iOS Control Center controls now work end-to-end.** *Quick Turbo Split* and
  *Quick Add Expense* open Bupples straight into that flow — from the Lock
  Screen, Control Centre, or the Action Button — including a cold start, where
  the app launches, restores your session, and then routes.

### Fixed
- **Settle-up notifications are now exact to the item.** Marking a single item
  paid (from the web or the app) no longer triggers a "everyone's paid" alert.
  Payment is tracked **per item**, and the "fully settled" nudge only fires when
  *every* owed item in a receipt or split has actually been paid — checked on
  the server so a partial payment, or items still unclaimed, can never trip it.
- **Pip looks like Pip in the widgets.** The home-screen and Lock-Screen widgets
  now draw the real Pip — proportioned, sharp, and expressive by balance state —
  instead of an off-model approximation.
- **Onboarding no longer collides with the keyboard.** On the name step, Pip
  tucks away and the page indicator steps aside while you type, so nothing
  overlaps — on every iPhone size.

### Changed
- **Widgets feel like Bupples.** A confident hero-balance treatment, warm-paper
  surfaces with quiet hairline rows, the app's accent colour, and Pip woven in —
  across Small, Medium, Large, and the Lock-Screen accessories.
- **Calmer onboarding motion.** The intro pages now settle in with a gentle,
  staggered entrance (bubble, then title, then body) instead of a quick fade —
  slower and more deliberate, and it still respects Reduce Motion.

---

## Build 16 — `1.0.0+16`

The native-widgets release, plus a deep correctness pass across money, privacy,
notifications and deleted-session proof.

### Added
- **iOS home-screen & Lock-Screen widgets.** A full WidgetKit family — Small,
  Medium, Large, Extra-Large (iPad), and the Lock-Screen Circular / Rectangular /
  Inline accessories, with a StandBy-friendly layout. Each shows your balance at a
  glance — net, who you owe, who owes you, recent hangouts and quick
  Activity / Join / Create taps. **Pip** is drawn natively (crisp at every size)
  with an expression for your state: worried when you owe, curious when you're
  owed, happy when settled. The widgets theme to your selected accent, work in
  light and dark, and carry a subtle "Updated" timestamp.
- **Control Center controls (iOS 18).** Two one-tap controls you can add to
  Control Centre, the Lock Screen, or the Action Button — **Quick Turbo Split**
  and **Quick Add Expense** — each opening Bupples straight into that flow.
- **Receipt quantity detection.** The scanner now reads item **quantities**
  ("2× Burger", "Qty 2", or separate qty / unit-price / total columns) and shows
  them — "2 × Burger" — while the split stays cent-exact.
- **Collaborative receipt editing.** The person who paid can fix an item's name,
  amount or quantity before it is settled; anyone else can **suggest a
  correction** that the owner approves or declines. Every change is recorded in a
  tamper-evident **edit history** kept with the receipt.
- **Join a hangout from the web.** The homepage gains a "Have a code? Join a
  hangout" entry that hands off into the app.

### Fixed
- **No more misleading "everyone's paid" notifications.** A claim-toggle bug could
  re-fire the settle-up push; it now fires **exactly once**, only when every
  debtor has truly paid, and never to yourself — with server-side de-duplication
  so a retry or app reopen can't replay it.
- **Strict account privacy.** A second account on the same device can never see
  the previous account's sessions — not even for a frame — enforced right at
  render time. The home-screen widget is account-scoped too and **clears
  instantly** on sign-out or account deletion.
- **Paid receipts are locked.** Once a receipt is added to balances, its items,
  amounts and title can no longer be changed (enforced in the app and in the
  security rules), so the recorded expense can never drift from its proof.
- **Archives stay put.** Deleting a session or a Turbo split no longer makes the
  record flicker away and reappear — it settles immediately into a read-only
  archived row, and **Turbo archives now reliably persist** across reopens.

### Performance
- **Smoother Turbo on the web** — fewer redraws (a no-op render guard) and a
  cleaned-up realtime listener so a backgrounded tab stops doing work.

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
