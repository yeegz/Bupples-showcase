← [Back to the README](../README.md)

# App Store readiness

Bupples is **live on TestFlight** at build `1.0.0+29` and preparing for App Store
submission. This is the launch-readiness checklist, with honest status on each
item.

## Status

| Area | State |
|------|-------|
| iPhone app | ✅ Live on TestFlight (`1.0.0+29`) |
| Web app | ✅ Live at [bupples.web.app/app](https://bupples.web.app/app) |
| iOS home-screen + Lock-Screen widgets | ✅ Shipping |
| iOS 18 Control Center controls | ✅ Shipping |
| App Store submission | 🚧 Preparing |
| Android | 📋 Planned |

## Compliance

- **Account deletion** — in-app, runs a server-side **anonymization (not a purge)**
  across shared records, leaving the records other participants depend on intact
  (Guideline 5.1.1(v)). See [privacy and deletion](privacy-and-deletion.md).
- **Sign in with Apple** — offered alongside Google, with server-side **Apple token
  revocation** on account deletion.
- **User-generated content** — receipt images can be reported for moderation,
  backed by a terms and abuse policy built to the UGC guidelines.
- **App Check** — all callable functions and web traffic are attested (reCAPTCHA on
  web).

## Quality and platform polish

- **Push notifications** with deep-link payloads that open straight to the relevant
  receipt, split, settle-up, or session.
- **Crash reporting and analytics** (Crashlytics + Analytics) wired across the
  funnel.
- **Accessibility** — haptics and full **Reduce Motion** support; a bundled type
  system for offline-safe, flash-free first frames.
- **Dark mode** across the app and the native widgets.
- **Responsive web** — comfortable centered layouts that scale from phone to
  desktop and tablet.
- **Force-update gate + maintenance flag** — a server-driven config can require a
  minimum build or show a maintenance state before launch issues reach users.
- **Server-driven What's New** — the in-app release history is published from
  Firestore, so notes can ship without an app update.

## Tests

The split math and the settlement ledger are covered by an automated test suite
(**255 passing tests** at build 29, plus a Dart ↔ JavaScript parity test that keeps
the app and the web in agreement to the cent).
