<div align="center">

<img src="media/brand/logo.png" width="104" alt="Bupples logo — two overlapping mint bubbles forming the Pip mark" />

# Bupples

**Split costs with friends, minus the awkward.**

Scan a receipt, let everyone claim what they had live, and Bupples settles the
group to the cent — even friends without the app.

[![Flutter · Dart](https://img.shields.io/badge/Flutter-Dart-02569B?logo=flutter&logoColor=white)](https://flutter.dev)
[![Firebase](https://img.shields.io/badge/Backend-Firebase-FFCA28?logo=firebase&logoColor=black)](https://firebase.google.com)
[![App Store — live](https://img.shields.io/badge/App%20Store-live-2F6F4E?logo=apple&logoColor=white)](https://apps.apple.com/us/app/bupples/id6777620291)
[![Google Play — live](https://img.shields.io/badge/Google%20Play-live-2F6F4E?logo=googleplay&logoColor=white)](https://play.google.com/store/apps/details?id=com.bupples.bupples)
[![Browser claim — live](https://img.shields.io/badge/Browser%20claim-live-2F6F4E?logo=googlechrome&logoColor=white)](https://bupples.web.app/t)
[![License](https://img.shields.io/badge/License-Proprietary-8B6F3E)](LICENSE)

**[On the App Store](https://apps.apple.com/us/app/bupples/id6777620291)** for iPhone · **[on Google Play](https://play.google.com/store/apps/details?id=com.bupples.bupples)** for Android · friends without the app claim a Turbo Split at **[bupples.web.app](https://bupples.web.app)**, right in the browser

</div>

> A solo-designed and -built product: the Flutter app, the Firebase backend and
> Cloud Functions, the native iOS home-screen widgets, and the no-app browser claim flow. **Source is
> private while Bupples prepares for launch** — happy to share read-only access with
> reviewers, recruiters, or collaborators on request.

---

## Screenshots

| Session · bubble field | Home | Activity | Settings |
|:---:|:---:|:---:|:---:|
| ![A session's live bubble field — members as photo bubbles sized by balance, a scanned receipt mid-review, and the squad all square](media/app/session-bubble-field.png) | ![Home — net balance across every hangout, with each hangout listed](media/app/home.png) | ![Activity — what you owe and what you're owed, broken down per person](media/app/activity.png) | ![Settings — appearance and accent themes, in dark mode](media/app/settings.png) |

> The signature view is the **live bubble field** — each member a physics-driven
> bubble sized by their balance, the host crowned, with a scanned receipt mid-review
> up top. Shown in dark mode; the app ships light and dark.

## How it works

1. **Start a hangout** and share a short code or a scannable **QR**.
2. **Scan the receipt** — Gemini reads the items (with quantities), tax, service,
   discount, store, and currency.
3. **Everyone claims what they had**, live — shared dishes split evenly, tax and
   service ride proportionally, and balances move in real time as items are ticked.
4. **Settle up** in the fewest payments, with how-to-pay details attached. Friends
   *without the app* do it from the browser.

Spend in another currency and Bupples keeps each currency in its own column,
settling within it — never a guessy conversion; more than one person paid and you
add every payer, and the ledger stays exact either way.

## What makes it different

Not "a better Splitwise" — a different mechanism. Most splitters divide a bill
*evenly* and leave the chasing to you.

| | A basic bill splitter | Bupples |
|---|---|---|
| **The receipt** | A photo you attach | Scanned, then **everyone claims their items** live |
| **Tax & service** | Split evenly | Ride **proportionally** to what each person ordered |
| **Who paid** | One payer | **Multiple payers** per bill, cent-exact |
| **No-app friends** | Need an account | **Claim from a browser**, no install |
| **Staying in sync** | A separate group chat | **Chat built into the session** — every expense, receipt & payment lands as a card in the timeline |
| **Chasing** | Awkward texts | One quiet, friendly **nudge** |

## Features

- 🫧 **Live bubble field** — each member is a draggable, physics-driven bubble
  sized by their balance; tap one for a full breakdown that reconciles in real time.
- 🧾 **Scan & claim receipts** — Gemini reads the items; everyone taps what they
  had and the split lands to the cent, tax, service, and discount included.
  → [receipt splitting](docs/receipt-splitting.md)
- 👛 **Multiple payers** — one bill fronted by more than one person, split equally
  or by exact amounts, with balances that stay cent-exact.
  → [the money model](docs/receipt-splitting.md#multiple-payers-build-29)
- ⚡ **Turbo split** — a fast one-off: scan, everyone claims their items, then a
  **mark → pay → host-confirms** flow settles it — your transfer details (and QR)
  ride along, and friends without the app claim from a browser.
  → [web & native](docs/web-and-native.md#no-app-participation)
- 🤝 **Smart settle-up** — the **fewest payments** by default, or flip to the full
  who-owes-whom view (your eyes only, never the balances).
  → [settle-up views](docs/receipt-splitting.md#settle-up-views)
- 💬 **A chat in every session** — messages, photos, and `@mentions` live alongside
  the split, and every expense, scanned receipt, and payment appears as a tappable
  **event card in the timeline** (not pinned at the top), so the group reads as one
  living thread. WhatsApp-style delivery: sent → delivered → **Seen by N** with
  reader avatars, replies, and no buzz for a chat you're already in.
- ✉️ **Direct messages** — a 1:1 inbox alongside the group chats: message any friend
  from the hub, with photos, replies, and the same sent / delivered / seen receipts,
  so a quick "you around to settle?" never needs a separate app.
- 🧮 **See exactly how your share was calculated** — a per-person breakdown (items
  you picked, then the proportional tax / service / discount allocation) that
  **reconciles to the cent** — the same engine for normal and Turbo receipts.
- 👋 **Friends & profiles** — claim a `@handle`, add friends, and start a split with
  them in a tap; private money requests go person-to-person, not to a public feed.
- 💳 **Pay Now** — settling someone leads with *how* to pay them: their copy-to-tap
  details and a **DuitNow / payment QR** you scan in your own bank app. Bupples
  records the settlement; it never moves the money.
- 🔔 **Friendly nudges** — a quiet reminder that appears only on someone who
  *directly* owes you, with a per-person cooldown and a server-written log.
- ♿ **Accessible by design** — screen-reader labels across the bubble field and
  money rows, non-colour-only state, ≥44pt targets, tabular figures, and a
  reduce-motion path honoured app-wide.
- 📲 **iOS home & lock-screen widgets** — a full WidgetKit family (home and lock
  screen) with Pip drawn natively in SwiftUI, plus Android home-screen widgets. → [web & native](docs/web-and-native.md)
- 🌐 **Claim from any browser** — a friend without the app opens the Turbo Split
  link and claims their items with no install, behind the same Firebase Auth and
  Security Rules as the app. → [web & native](docs/web-and-native.md#no-app-participation)
- 🔐 **Privacy-first accounts** — anonymous by default; Google or Apple to back up;
  deletion **anonymizes instead of erasing**, so no one can delete their way out of
  a debt. → [privacy & deletion](docs/privacy-and-deletion.md)

## Engineering highlights

The decisions that were hard, and why they went the way they did. Depth lives in
[`docs/`](#documentation).

- **One money implementation, two languages.** The cent-safe split math is written
  once in Dart and ported to JavaScript for the web, **guarded by a parity test** so
  the app and the browser agree to the cent — and reused by Turbo, collaborative
  session receipts, and plain expenses alike.
- **A multi-payer ledger that doesn't double-count.** Settling multiple payers by
  spreading each consumer's gross share across payers double-counts a payer who also
  consumed; Bupples works in **net positions** instead, staying exact for any mix of
  payers and consumers and reducing to the simple case for one payer. The model is
  backward-compatible — single-payer records are byte-identical and need no
  migration. → [the money model](docs/receipt-splitting.md#multiple-payers-build-29)
- **Collaborative real-time receipts.** A receipt is a live Firestore object with a
  **per-claimant** `claims` subcollection (concurrent writes never contend); an
  unfinalized receipt contributes a dedup-guarded *preview* to the balance math, so
  the whole table's numbers move as items are ticked and then snap to the identical
  figure when it lands. → [receipt splitting](docs/receipt-splitting.md#collaborative-real-time-receipts)
- **Server-authoritative trust.** Settle-up acceptance, the item-exact "everyone's
  paid" event, and account deletion are decided **on the server** — clients propose,
  Cloud Functions and Security Rules dispose — and audit logs are written by Cloud
  Functions so clients can't rewrite them.
- **Privacy-preserving deletion.** Account deletion runs server-side as an
  **anonymize, not a purge**, leaving the shared records other people depend on
  intact, with Apple token revocation. → [privacy & deletion](docs/privacy-and-deletion.md)
- **One codebase, two app targets + a widget.** The same Flutter app compiles to
  the iPhone and Android builds and feeds a native iOS home-screen widget via a
  shared App Group with cold-start deep-link routing; a separate lightweight web
  page lets no-app friends claim a Turbo Split in the browser.
  → [web & native](docs/web-and-native.md)
- **Receipt understanding via Gemini.** A callable Cloud Function runs **Gemini 2.5
  Flash on Vertex AI**, returning structured line-items + tax / service / discount /
  total as JSON; the model fills in the numbers, the unit-tested Dart math decides
  the split. → [receipt splitting](docs/receipt-splitting.md#receipt-understanding-the-scan)

## Tech stack

| Layer | Choices |
|-------|---------|
| **App** | Flutter · Dart (iOS · Android · Web) |
| **State** | Riverpod (`StreamProvider` / `Provider.family` + controllers) |
| **Backend** | Firebase — Firestore (real-time sync), Auth (Anonymous · Google · Apple), Cloud Functions, Cloud Messaging, Cloud Storage, App Check, Analytics + Crashlytics |
| **Receipt AI** | Gemini 2.5 Flash via Vertex AI — structured receipt JSON behind a callable function; the split math that consumes it is pure, unit-tested Dart |
| **Functions** | Node.js · TypeScript (Cloud Functions v2) |
| **Web** | A lightweight JS + anonymous-auth claim page on Firebase Hosting so friends **without the app** claim their Turbo Split share from a browser (the app itself ships on iOS + Android) |
| **iOS** | Swift Package Manager (no CocoaPods); WidgetKit home-screen widgets (Pip in SwiftUI `Canvas`); Universal Links + deep links |
| **Design** | A token-driven theme — Fraunces + Hanken Grotesk, **System / Light / Dark** that follows the phone, seven accent themes, and an optional gradient atmosphere; the **Pip** mascot as an animated `CustomPainter`; a hand-rolled soft-body bubble simulation |

## Architecture

Feature-first and layered: the UI depends only on repository *interfaces*, so an
in-memory demo backend and Firestore are interchangeable, and everything that must
be trusted runs on the server. The same codebase is the iPhone app, the full web
app, and the data source behind the native iOS extension.

**[Read the architecture overview →](docs/architecture.md)** (with a system diagram)

## Documentation

| Doc | What's inside |
|-----|---------------|
| [Architecture](docs/architecture.md) | Layers, the system diagram, the serverless push pipeline, iOS + Android from one codebase |
| [Receipt splitting & the money model](docs/receipt-splitting.md) | Cent-safe math, the multi-payer net-position ledger, collaborative real-time receipts, Dart ↔ JS parity |
| [Privacy & deletion](docs/privacy-and-deletion.md) | Anonymize-not-erase, soft-archive, server-authoritative trust, Security Rules |
| [Web & native](docs/web-and-native.md) | The no-app browser claim flow, iOS widgets + Control Center |
| [App Store readiness](docs/app-store-readiness.md) | TestFlight status, compliance, quality checklist |

## Status

Build **`1.1.0+110`** ([CHANGELOG](CHANGELOG.md)) — **live on the App Store and Google Play**,
with a browser claim page for no-app friends at [bupples.web.app](https://bupples.web.app).

| Surface | Status |
|---------|--------|
| App Store (iPhone) | ✅ Live (`1.1.0+110`) |
| TestFlight (iPhone) | ✅ Live (`1.1.0+110`) |
| Browser Turbo claim | ✅ Live for no-app friends ([bupples.web.app](https://bupples.web.app)) |
| Google Play (Android) | ✅ Live (`1.1.0+110`) |
| iOS home-screen widgets | ✅ Shipping |

---

<div align="center">

**Bupples** · © 2026 Yousof Selim · All Rights Reserved · [License](LICENSE)

</div>
