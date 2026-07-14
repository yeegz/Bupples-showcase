# Changelog

All notable changes to **Bupples**, newest first. ← [Back to the README](README.md)

Bupples is **live on the App Store and Google Play** at marketing version `1.1.0`. The
**build number** (`1.1.0+N`) advances each release. Engineering detail lives in
[`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md).

## The arc so far

`1.1.0` is "The Native update", and it is three things at once: a top-to-bottom
redesign, a private money space, and Bupples learning where it is.

- **A redesign with a doctrine.** The visual rework (builds 100 to 112) came with an
  enforced money law, a motion token system with a ratchet test, and an ambience
  governor that trades decoration for thermal headroom.
- **Just You**: a private personal-money space with a deterministic money-left engine,
  mirrored in scheduled UTC+8 Cloud Functions so the server and the client agree.
- **Widgets**: six iOS families including the Lock Screen, plus Android RemoteViews,
  from one shared payload.
- **Native region**: your region sets your currency and the correct local payment
  rails.
- **The What's New rebuild**: release notes served from Remote Config, so the history
  ships without an app update.

---

## `1.1.0+112` (2026-07-14)

Onboarding polish, and two dead systems removed.

### Changed
- **Just You onboarding** animates its steps, centres the short ones, and gives feedback
  on every tappable.
- **Cover to background** works on every backdrop now: the tint is primed when the cover
  is set and settles smoothly instead of recomputing per render.
- **Appearance** dropped the Avatar ring and Profile cover sections.

### Removed
- **The legacy What's New system and profile covers**, both fully superseded and now
  dead code.

### Fixed
- The Home `HANGOUTS` header overflowed by 39px at text scale 1.3.
- A debug or profile build could permanently lower a device's ambience ceiling through
  the self-heating audit. Neither Low Power Mode nor a stale device class may freeze the
  app either.
- iOS: added `NSLocationAlwaysAndWhenInUseUsageDescription` (ITMS-90683).

---

## `1.1.0+111`

The Just You server half, and the widget currency pass.

### Added
- **Just You scheduled Cloud Functions**: monthly rollover, bill reminders, budget
  check and month-end summary, all running in **Asia/Kuala_Lumpur (UTC+8, no DST)** so
  the month boundary, the due-day maths and quiet hours read Malaysia wall clock. Each
  push type carries its own idempotency guard, so a retried delivery cannot double-send.
  Where a figure cannot be computed honestly (income in a different currency from the
  month's dominant spend), the nudge is skipped rather than approximated.
- **The Just You hangout bridge**: a hangout expense mirrors **only your share** into
  your private ledger, deduplicated by expense reference. Settlements never touch it.
- **Just You recommendation settings**: frequency, types, aggressiveness, quiet hours
  and opt-outs, mirrored to the server so a user who never opens the screen still gets
  the documented defaults.
- **The Pip narrative validator and the rule-based insight engine.** The deterministic
  engines own every figure; the validator is the fail-closed guardrail that rejects any
  monetary figure Pip did not receive in its payload, proven by an adversarial test.

### Changed
- **Widgets differentiate currencies**, show a real Pip at XL, drop the "as of" line,
  and gained a Pro-gated Just You widget.
- **Balances** give each person their own card, with pay-now / pay-later on the card.
- **Receipts** claim on the whole row, and a bill splits across multiple payers.
- **Settings**: payment and notification preferences moved to the user; the default
  split stays per-hangout.

### Fixed
- A cosmetic frame now restores from the cloud profile on a fresh device.
- Owe / owed arrows on the widgets pointed the wrong way.

---

## `1.1.0+110`

Widgets land, Bupples learns where it is, and the release gate runs.

### Added
- **Home and lock-screen widgets** ([detail](docs/ARCHITECTURE.md#9-the-widget-bridge)).
  One Dart payload through an App Group feeds **six iOS WidgetKit families** (small,
  medium, large, plus the three Lock Screen accessories) and **Android RemoteViews**.
  Money figures are marked privacy-sensitive so amounts redact when the device locks.
  No widget ever makes a network call.
- **Region packs**: eleven regions, each setting the currency, the correct payment QR
  rail (DuitNow, PayNow, PromptPay, QRIS, UPI, PIX, or honestly `null` where no consumer
  QR rail exists) and the right bank-field schema. Served from Remote Config, so a new
  region ships without an app release.
- **Internationalisation** and a region-aware payment-details rebuild.
- **The remote control plane baseline**: kill switches for Pip, receipt scan, nudges and
  the paywall; maintenance mode; force and soft update gating; announcements. All
  fail-open with baked-in client defaults.
- **Free tier set to 3 hangouts** (was 6), with a contextual paywall.
- **Hangout location suggestion**, auto-set and dismissible.

### Changed
- **Light-mode semantic theming**: per-brightness glass, and money shades re-tuned to
  clear WCAG AA on warm paper. The vivid dark mint and coral cannot pass AA on paper, so
  light mode resolves to deeper, more saturated values. Deeper light themes want more
  saturation, not less.
- **Friend profile** rebuilt as a ledger with a face.
- **The Customise screen** and a custom accent wheel with the money guardrail built into
  the picker, so an out-of-bounds accent cannot be chosen in the first place.
- **Motion system v2**: tokens, catalogue, degradation matrix, and the ratchet test that
  fails the build on a new raw `Duration()` literal outside the token files.
- Display-name changes propagate to live surfaces.

### Fixed
- **A real Reduce Motion crash**, found by running the release-readiness gate as an
  adversarial pass. A skeleton shimmer held its `AnimationController` in a `late final`
  field, so the low-tier early return left it unconstructed and `dispose()` triggered the
  initialiser, building a Ticker at teardown and doing an ancestor lookup on a
  deactivated element. Any skeleton shown then torn down under Reduce Motion threw.
- Per-currency residue across logs, archived hangouts, recap and Pip.
- **The iOS "Cycle inside Runner" build failure.** Reordering the Runner target's build
  phases so Embed Foundation Extensions runs before Thin Binary fixed it. The real cycle
  was Embed ↔ Thin Binary ↔ `Info.plist`, not the App Intents metadata it appeared to be.

### Removed
- The `@google-cloud/vision` dependency and 11 verified-dead files, in a cleanup sweep.

---

## `1.1.0+106` to `1.1.0+109`

Gifting, multi-currency settle-up, and honest defaults.

### Added
- **Pip Pro gifting**: a subscriber can gift Pro, granted as a RevenueCat promotional
  entitlement. Gift recipients cannot re-gift: the check requires a live real-store
  subscription, not a promotional one.
- **Session report export**, and activity-log export.
- **Multi-currency settle-up** with per-currency balance surfaces and a bubble currency
  row.
- **Growth analytics instrumentation** (funnel and North Star events).

### Changed
- **Auto-confirm is off by default**, client and server. A payment silently
  auto-confirming is a defensible default only if the user chose it.
- **Pip differentiates currencies** and will not blend them into one figure.
- **Restore hardening** for subscriptions.

---

## `1.1.0+100` to `1.1.0+105`: the redesign

The full redesign, shipped in lettered phases. Backend rules, indexes and functions
deployed by name.

### Added
- **The Ambience Governor** ([detail](docs/ARCHITECTURE.md#the-ambience-governor)): one
  33ms ambient clock for all decorative motion, and a four-rung tier ladder
  (Lively → Calm → Still → Static) resolved as `min(deviceCeiling, heatCeiling,
  powerCeiling)`, floored under Reduce Motion. Down is immediate; up is earned over 90
  continuous clean seconds. A self-heating audit can permanently lower a device's
  ceiling.
- **Bupples Solo**, the private "Just you" tracker, Pro-gated, later rebuilt into the
  full Just You budget tool.
- **Expense approval flow**, roles management, and hangout settings.
- **Recurring expenses**, weights, share-out, export, and a per-hangout activity log.
- **A source-artifact backbone**: a server-written audit log with a retention stamp,
  roles, and receipt provenance.
- **The hints engine**, and a first run that teaches.
- **Badges**, the Passport profile, and the wardrobe.
- **Three font themes** app-wide, and settings that sync across devices.
- **A dedicated password-reset flow.**

### Changed
- **Money clarity pass**: the money law made enforceable: integer sen, protected
  mint / coral / gold roles the user's accent can never reach, no sign prefixes by
  default, and no screen that blends two currencies into one figure.
- **Receipt claiming rebuilt** around the claimant.
- **Home is the do tab, Groups is the talk tab.**
- **A paywall that survives review**, and server-authoritative Pro equips: Pro frame ids
  are denied to clients in the rules, and the only route onto one re-checks the live
  entitlement.
- **Deletion completeness pass**, TTL, and abuse guards.

### Fixed
- **A security wave**: rules proven, money paths made idempotent, the RevenueCat webhook
  switched to verify-then-write so it never trusts the event payload's entitlement
  claims.

---

## `1.1.0+96` to `1.1.0+98`: settlement

### Added
- **Partial payments**: pay an amount now, the rest later.
- **72-hour auto-confirm**, a write-off (forgive) path, and a dispute path.
- **Settle-Up net header** with a per-user Direct / Simplified mode chip, and an ambient
  progress-to-settled bar.
- **A colour guardrail** separating the pay-action and destructive lanes from the money
  lanes on money screens.

---

## `1.0.0+78` to `1.1.0+95`

Groups became a social hub, the profile became a passport, and Pip Pro shipped.

### Added
- **Pip Pro subscriptions** via RevenueCat, with a webhook that syncs entitlements.
- **Groups as a social hub**: friends and hangouts in one place.
- **The Passport profile**: your bubble, your stats, and a QR to add friends.
- **Direct messages**, and read receipts in both DMs and hangout chat.
- **Auth redesign**, Pip chat with a streaming reveal, and DM typing and read-state.

---

## Earlier

Builds `1.0.0+1` to `1.0.0+79` covered the original product: hangouts, the live bubble
field, receipt scanning via Gemini on Vertex AI, the multi-payer net-position ledger,
friends and `@handles`, Pay Now, nudges, and the Turbo split with a browser claim page
for friends without the app.

> **A note on this file's history.** Earlier revisions described a full Flutter web app
> at `bupples.web.app/app` and iOS 18 Control Center controls. Both were **removed** from
> the product: `/app` now 302-redirects to the landing page, Bupples ships iOS and
> Android, and the widget extension is a plain `StaticConfiguration` with no App Intents.
> The only web surface left is the Turbo-claim page for friends without the app. The
> entries above are reconciled against the app repository's git history rather than the
> older changelog text.
