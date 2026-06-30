# Changelog

All notable changes to **Bupples**, newest first. ← [Back to the README](README.md)

Bupples is pre-launch: it iterates through TestFlight at marketing version
`1.0.0`, and the **build number** (`1.0.0+N`) advances each release.

## Latest highlights

- **Direct messages + group read receipts** — a 1:1 inbox beside the session chats,
  and WhatsApp-style sent → delivered → **Seen by N** receipts in both (Builds 47–79).
- **Turbo split, end to end** — claim your items, **pay**, and the host **confirms** —
  with your transfer details and QR carried through (Builds 47–79).
- **A theme that follows your phone** — System / Light / Dark, seven accent themes,
  and an optional gradient atmosphere as a separate toggle (Builds 47–79).
- **A chat in every session** — messages, photos, `@mentions`, and every expense,
  receipt & payment as a tappable **event card woven into the timeline** (Builds 43–46).
- **See exactly how your share was calculated** — a per-person receipt breakdown that
  reconciles to the cent, for normal *and* Turbo receipts (Build 46).
- **Friends, `@handles` & Pay Now** — add friends, start a split in a tap, and settle
  with someone's copy-to-tap details or a **DuitNow / payment QR** (Builds 31–32).
- **Multiple payers** on any bill, with cent-exact balances (Build 29).

---

## Builds 47–79 — `1.0.0+79`

Messaging grows up, Turbo gets a real pay flow, and the theme learns to follow the phone.

### Added
- **Direct messages.** A 1:1 inbox in the messages hub — message any friend, with
  photos, replies, and the same delivery/read receipts as the session chat.
- **Group read receipts.** A session message goes sent → delivered → **Seen by N**,
  with the avatars of who's read it — per-member, not per-message, so it stays cheap.
- **Turbo: mark → pay → confirm.** A claimant marks their items, sees the breakdown
  and the host's transfer details + QR, taps pay, and the **host confirms receipt**;
  the host can then delete the settled split. The host never pays or confirms their
  own share.
- **System / Light / Dark theme.** Brightness now follows the phone on **System**
  (iOS + Android) and rebuilds when the OS flips; the gradient atmosphere became a
  separate on/off toggle, independent of light/dark.
- **OS notification prompt at the right moment** — requested at the end of onboarding
  and as a shell-level catch-all, so every signed-in user is asked (Android 13+ and iOS).

### Changed
- **Payment details always reach the payer.** Your "how to pay me" default (including
  the QR) now re-syncs to every session and hosted Turbo split whenever it changes —
  so "hasn't added payment details" no longer shows when you actually have.
- **Native-feeling navigation.** Left-edge **swipe-to-go-back** app-wide, and tapping
  outside a field dismisses the keyboard.

### Fixed
- Photos in the session chat (a rule had silently rejected image/reply/mention
  messages); the raised Pip nav button sitting too low on iPhone; Pip's eyes washing
  out white-on-white in light mode; long Pip suggestions shrinking to an unreadable
  size; and a session budget that counted other currencies.

---

## Build 46 — `1.0.0+46`

A premium refinement pass — and chat becomes a living session timeline.

### Added
- **Chat event cards, woven into the conversation.** Every expense, scanned receipt,
  submitted/confirmed/rejected **payment**, and expense dispute now appears as a
  compact card at its real position in the chat timeline — never pinned at the top —
  and each one taps through to its detail (the expense, the receipt, or the transfer).
- **"How your share was calculated."** Claim items on a receipt and expand a clear,
  per-person breakdown — the items you picked, then the proportional discount / tax /
  service allocation — that **reconciles to the cent**. One shared engine powers it
  for both normal and Turbo receipts.
- **DuitNow / payment QR in your profile.** Add a payment QR once and it shows up
  whenever someone goes to pay you, alongside your copy-to-tap details.

### Changed
- **A softer, more cohesive shape language.** Cards, sheets, and chat bubbles now use
  continuous-curve "squircles" throughout, on a refined green tonal palette.
- **No buzz for a chat you're already reading.** Notifications are suppressed for the
  session you're currently viewing; other chats still notify, and messages still mark
  as read.
- **Tabs return to the top** when you switch away and back, so a tab never strands you
  mid-scroll. In-progress forms are never interrupted.

### Improved
- **Accessibility.** Each member bubble is now a labelled, focusable element for
  screen readers; avatars, amounts, and status chips carry meaningful labels; touch
  targets clear 44pt; money renders in tabular figures; and reduce-motion is honoured
  app-wide. Status colours are never the only cue.

---

## Builds 30–45 — `1.0.0+30` … `1.0.0+45`

Identity, friends, and a real-time chat — Bupples grows from a splitter into a social
money app. (Spans several TestFlight builds.)

### Added
- **A chat in every session.** Real-time messages and photos, `@mentions` that notify,
  replies, and read receipts — the conversation living right next to the split.
- **Friends & profiles.** Claim a `@handle`, build a friends list, and start a split
  with friends in a tap. Private money requests go **person-to-person**, never to a
  public feed.
- **Pay Now.** Settling someone leads with *how* to pay them — their copy-to-tap
  payment details and an optional QR — while Bupples stays a calculator, not a wallet.
- **Multi-currency.** Spend in another currency and Bupples converts at that day's
  locked-in rate, keeping the ledger exact.
- **Export & trip recap** (CSV + share) and **scheduled reminders** for aging debts.

### Changed
- **The Pip mascot, everywhere.** One animated character (a hand-rolled `CustomPainter`)
  that reacts to context — calm on money surfaces, lively on social ones.
- **Removed the iOS Control Center controls** to focus the native surface on the
  home-screen widgets.

### Improved
- **Cross-device fit & web responsiveness**, a production hardening pass (account-scoping,
  money invariants, privacy-preserving deletion), and broad performance work.

---

## Build 29 — `1.0.0+29`

Multiple payers, grouped receipts, and a faster web app.

### Added
- **Multiple payers on one bill.** _[Money-critical]_ An expense, receipt, or item
  can now be fronted by more than one person — split it equally or set exactly what
  each payer put in. Balances stay cent-exact everywhere, computed from each
  person's net position (what they paid minus their share), so a payer who covered
  exactly their own share ends up square. Existing single-payer history is
  unchanged.
- **An owner heads-up when a receipt is fully claimed.** Once everyone has claimed
  their items, the receipt's owner gets a push so they can finalize or clear it —
  separate from the "everyone's paid" alert, and fired exactly once.

### Changed
- **Several receipts stack into one tidy group.** A session with multiple receipts
  now shows a single grouped card that opens a list, instead of crowding the screen.
- **Turbo gains add and remove item** — full parity with normal-session receipts —
  and reads quantities into a structured field ("2 × Burger") rather than the name.
- **Profile actions tucked into a menu.** A person's host/remove/ban/rename actions
  now sit in a top-right overflow menu, and **Delete Account** moved to the very
  bottom of Settings, out of the way of everyday controls.

### Improved
- **A noticeably faster web app.** The web build now uses the WebAssembly (`skwasm`)
  renderer and drops the browser's single most expensive per-frame cost — the
  backdrop blur behind the nav bar and bubble field — for a near-opaque fill, with
  avatar images decoded at display size. Smoother scrolling and interaction
  throughout.

---

## Build 28 — `1.0.0+28`

Nudges, and a Settle Up you read your way.

### Added
- **Nudge someone who owes you.** A quiet **Nudge** button appears on a person's
  profile and on their settle-up row — but *only* when they actually owe **you**
  (the direct, pairwise debt, never a routed-through balance), never when you're
  square, and never for an offline guest with no device. One tap sends one polite
  reminder; they get a push that opens straight to Settle Up. A friendly
  **per-person cooldown** means no one can be spammed.
- **A nudges & reminders log.** Every nudge is recorded in the session log — who
  reminded whom, and when — written **server-side** so the record can't be forged,
  and kept even after a session is archived.

### Changed
- **Settle Up is now your view to choose.** A **Summary / Full** switch sits in the
  top-right: *Summary* nets everyone down to the fewest payments (the default),
  *Full* shows every direct who-owes-whom debt. The choice is **personal** — it
  changes only how *you* read Settle Up, never the balances or what anyone owes, and
  two people in the same session can read it differently. A small explainer makes
  that clear, and both views reconcile to the exact same balances.
- **Retired the shared "simplify debts" setting.** It used to be chosen once for the
  whole session at creation; it's replaced by the personal Summary/Full view above.
  Old sessions keep working unchanged.

### Fixed
- **A person's breakdown always adds up — live.** While a receipt is still being
  claimed, the member detail sheet now reconciles in real time: what they paid,
  their share, what they've settled, and what they owe stay in lockstep (it used to
  leave out the still-being-claimed receipt, so the numbers didn't tie out). Tapping
  a still-claiming row opens the live receipt.

---

## Build 26 — `1.0.0+26`

Travel-ready: spend in any currency, export the trip, and remind politely.

### Added
- **Add an expense in any currency.** Picked a different currency for one expense?
  Bupples converts it to your hangout's currency at **that day's exchange rate** and
  **locks the rate in**, so the totals never drift later. Each person's share
  converts cent-exactly, and the original amount + rate are kept on the expense.
- **Export a hangout.** Share a clean **summary**, or a **CSV** of every expense and
  payment, straight from the session log via the native share sheet (or a download
  on the web).
- **Friendly reminders.** Send a gentle nudge to anyone who still owes — and Bupples
  quietly reminds you about settle-up requests that have sat waiting for
  confirmation.

### Changed
- **A nudge to rate Bupples** after everyone's squared up — once, never pushy.

---

## Build 25 — `1.0.0+25`

A pre-launch security hardening pass.

### Changed
- **Settle-up requests are verified end-to-end on the server.** Recording "I paid"
  now routes through a trusted Cloud Function that checks both people really are
  members and derives the creditor itself, so a settle-up can't be forged or
  self-accepted — closing the door on ledger tampering before launch.
- **Tightened security across balances, accounts and shared data** after structured
  adversarial audits — stricter rules, rate-limited joins, trimmed logs, and
  hardened content-security headers.
- **Popups and sheets are correctly sized** on tablet and desktop web (no more stray
  full-width frame behind a sheet).

---

## Build 24 — `1.0.0+24`

Live balances, shared edits, and a What's New you can read in the app.

### Added
- **Receipt claims move everyone's balances live.** As people tick the items they
  had, the session's balances update in real time *before* the receipt is finalised
  — so you can watch a split come together, and it's counted exactly once when it
  lands.
- **Suggest a fix on a Turbo split.** Turbo gains the same editing system as session
  receipts: the owner edits items, names, quantities, prices, tax, service and
  discount; anyone else can suggest a correction the owner approves or declines — all
  in a tamper-evident log, and locked once it's paid.
- **A full What's New history** lives in Settings — every release as a collapsible
  card, newest first.

### Changed
- **Roomier layouts on tablet and desktop web** — a comfortable centred column that
  grows with the window instead of a stretched phone.
- **Every receipt's history shows in the session log** — uploaded, shared, corrected,
  added to balances, paid — so the record behind an expense is always there.
- **A branded loading screen on the web** instead of a blank page on first load.

### Fixed
- **More accurate split math** when a receipt carries extra charges.

---

## Builds 18–23 — `1.0.0+18` … `1.0.0+23`

A run of web fixes and widget refinements between the native-widgets release and
the live-balances pass:

- **Web sign-in & links that work** — Continue with Google in the browser, join
  links that drop you straight into the session, and Turbo showing your items
  correctly.
- **Profile photos on the web**, smoother responsive scaling across phones and
  tablets, and the real Bupples icon in the web app.
- **What's New, in the app** — see exactly what changed each update.
- **Crash reporting and update prompts** so issues get squashed faster and you know
  when there's something new.
- **Widget polish** — the app's typeface, a refined dark mode, and clearer handling
  of reported content.
- Tightened security and privacy under the hood throughout.

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
