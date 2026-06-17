← [Back to the README](../README.md)

# Web and native: one codebase, three surfaces

The same Flutter codebase ships as the iPhone app, the full web app, and the data
source behind a native iOS widget + Control Center extension. Shared session and
Turbo links **hand off** intelligently: open the installed app via a deep link, or
continue in the web app.

## The full web app

The **full Flutter web app** runs in the browser at **bupples.web.app/app** —
sign in, see your hangouts and balances, join a normal session or a Turbo split
from a link, and settle up — guarded by **App Check** (reCAPTCHA v3) like mobile.
A shared link opens the app if you have it and otherwise hands off cleanly to the
web version.

**Web performance (build 29).** The web build is compiled with the **WebAssembly
(`skwasm`) renderer** (with the canvaskit build as a fallback), and the runtime
was tuned for the browser's real cost model: the per-frame `BackdropFilter` blur —
CanvasKit's single most expensive operation — is skipped on web for the nav bar
and bubble field in favor of a near-opaque fill, avatar images are decoded at
display size via `ResizeImage` cache widths, and the bubble simulation runs at a
lighter segment count. The result is a noticeably smoother browser experience.

## No-app participation

Friends **without the app** can still claim a **Turbo split** from a plain
browser: a lightweight static page (Firebase **JS SDK + anonymous auth**) lets a
non-user pick a name, claim their items, and see what they owe — no install — with
a gentle nudge to get the app for the rest.

Every write from that page is **scoped by Security Rules**: a guest can only append
their own membership and write their own claim, never edit the receipt or another
person's choices. The page runs the **same split math** as the app through the
parity-tested JavaScript port (see [receipt splitting](receipt-splitting.md)), so
the browser and the app always agree to the cent.

## iOS widgets and Control Center

A separate **WidgetKit + App Intents** extension puts Bupples on the home screen
with **no Flutter on the home screen**:

- A full widget family — Small · Medium · Large · iPad Extra-Large, plus
  Lock-Screen Circular / Rectangular / Inline, StandBy-friendly — shows your net at
  a glance, who you owe / who owes you, recent hangouts, and quick
  Activity / Join / Create taps.
- **Pip is redrawn natively in a SwiftUI `Canvas`** — bubbles and Bézier eyes by
  mood — to stay on-model and crisp at every size, themed to your accent in light
  and dark.
- Two iOS 18 **Control Center controls** — Quick Turbo Split and Quick Add
  Expense — sit on Control Center, the Lock Screen, or the Action Button and open
  Bupples straight into that flow.

The app writes a compact balance **snapshot to a shared App Group**, which the
widgets read to render. The Control widgets fire an `OpenURLIntent` that the app
routes through a native → Flutter MethodChannel, **including a cold start**: the
launch URL is stashed, replayed once Flutter is ready, and the router restores the
session before navigating.

## CocoaPods-free by constraint

The iOS side is **Swift Package Manager only** — no CocoaPods. Every dependency
ships a `Package.swift`; the widget and Control Center work, the image cache, and
the Firebase stack all had to fit that constraint, with the embed/sign build phases
ordered to avoid a build cycle. The web upload path is `dart:io`-free so receipt
and transfer-proof images upload to Cloud Storage from the browser too, gated by
Security Rules.

## Deep links

Universal Links and a custom-scheme fallback route shared session and Turbo links,
push-notification payloads, and the Control Center controls to the right screen —
restoring session state first when the app cold-starts.
