← [Back to the README](../README.md)

# Privacy, deletion, and trust

Bupples holds shared financial records (who paid, who owes, what was split) for groups
of people who trust each other. That makes the privacy design a genuine conflict rather
than a checklist: **one person's privacy choice cannot be allowed to destroy proof that
other people depend on.**

The guiding rule: **you can leave, but you cannot delete your way out of a debt.**

Current at `1.1.0+112`. See also [the architecture deep dive](ARCHITECTURE.md#2-data-model-and-rules-posture).

## Deletion anonymises, it does not purge

Account deletion runs server-side, in `deleteAccount`, as an **anonymise, not a purge**.

It rewrites only *this* user's identity across every session they belong to, whether they
host it or not: the name becomes "Deleted user", the auth uid is dropped, the pay handles
are dropped. The expenses, transfers and receipts the *other* participants rely on stay
untouched. Deleted people remain in the history that references them, as anonymised
participants.

Pool photos the user uploaded are anonymised rather than removed: the shared memory is
kept, the identity scrubbed. A group photo from a night out belongs to everyone who was
there.

This is fraud-resistant by construction. No owner can use account deletion to erase proof
or dodge a debt, because deletion cannot reach anyone else's records. It also satisfies
App Store Guideline 5.1.1(v), including **Apple token revocation** (a real call to
`appleid.apple.com/auth/revoke`) when an Apple-linked account is removed, so Apple
severs the Sign in with Apple link too.

The action lives at the very bottom of Settings, separated from everyday controls so it
cannot be hit by accident.

## Sessions archive, they do not vanish

Removing a hangout is a **one-way soft-archive**, not a destructive delete. Records
freeze, remain **exportable** by everyone who was in it, and stay viewable read-only.

There is no client path to a hard delete of shared financial data. That is a deliberate
absence rather than a missing feature.

## Locks preserve proof

Once a receipt is added to balances it **locks**: items, amounts and title can no longer
change, so the recorded expense can never drift from its proof. This is enforced in the
app *and* in the Security Rules, because an enforcement that only exists in the client is
a suggestion.

Pre-settle edits are **owner-approved correction requests** appended to a **server-written
history**. A receipt that should never have landed is **voided**, not edited: the record
stays and is marked.

Audit log entries are written by Cloud Functions and carry a retention stamp
(`LOG_RETENTION_DAYS`, 400 days) for a TTL policy. Clients cannot write them, which is the
only thing that makes them worth reading.

## Authorisation is bound to the auth identity

- Every write is authorised against the Firebase **auth uid**, never a client-supplied
  member id. A scripted client cannot impersonate its way into a hangout.
- **Sessions cannot be enumerated.** There is no listable sessions collection: a session is
  readable only with its unguessable id, reached through a `joinCodes/{CODE}` lookup. This
  is a **capability-URL design**, where knowing the link is the permission, which is what lets
  a share link work for a friend with no account while a scraper with a valid login still
  cannot discover a hangout it was not given.
- **Joining goes through a Cloud Function.** `joinSession` runs on the admin SDK and
  enforces bans; clients cannot grow `authedUids` themselves at all.
- **Expenses are owner-scoped:** only the uid that created an expense (or the host) may
  edit or delete it. Everyone else files a change request the owner resolves.
- **`payProfiles/{uid}` is writable only by its own uid**, so "how to pay me" details
  cannot be tampered with through a session document.
- **Private data is owner-only.** Everything under `users/{uid}` (Just You state, the solo
  expense ledger, preferences) is readable and writable by that uid alone.
- **Some collections are deny-by-default on purpose.** Rate-limit buckets, scan quotas and
  Pip quotas have no rules `match` block at all; the default deny keeps clients out and
  only the admin SDK reaches them.
- **No client hard-deletes.** Sessions soft-archive one way; deleted accounts anonymise
  server-side.

The rules suite carries **294 tests**, and the rules were hardened through self-directed
adversarial review before launch.

### App Check: the honest status

App Check is plumbed through **every** callable behind a single constant, so enabling it
is one line that cannot miss a function. **That constant is currently `false`**, and
deliberately: the Android client cannot ship `firebase_app_check` while the plugin fails to
compile on AGP 9 / Kotlin 2.3, and enforcing would reject every Android call and all local
testing.

So App Check is **wired but not enforced** at `1.1.0+112`. Access control today rests on
Firebase Auth plus the 294-test rules suite. The enable path is documented in the source:
restore the plugin when it compiles, confirm attestation on TestFlight and Play internal,
flip the constant, deploy the callables by name.

Claiming "App Check protected" today would be false, so it is claimed nowhere in this repo.

## Server-authoritative trust

Clients propose; Cloud Functions and Security Rules dispose. Decided on the server, not
in the app:

- **Settlement acceptance** (`settleDirect`), verified end to end so it cannot be forged
  or self-accepted.
- **Receipt finalisation** (`finalizeReceipt`), rate-limited and idempotency-keyed so a
  retry cannot double-write money.
- **Account deletion**, as above.
- **The Pro entitlement.** `customers/{uid}` is written only by the RevenueCat webhook,
  which uses **verify-then-write**: it never trusts the event payload's entitlement
  claims, and re-reads the subscriber from the RevenueCat REST API first. Pro bubble
  frames are denied to clients in the rules; the only route onto one is a Cloud Function
  that re-checks the live entitlement.
- **Nudge cooldowns and the nudge log**, both server-enforced and server-written.

Every money callable opens with `assertNotMaintenance()`, then auth, then a validated
input schema, then a rate-limit bucket, then an idempotency key, in that order. A client
that ignores the maintenance flag still cannot write money.

## Privacy by default

- Sign-in is **anonymous by default**; linking Google or Apple is optional and exists to
  back up and sync across devices.
- A hangout's data is scoped to its membership. No one outside it can read it, and no one
  can list hangouts at all.
- **No ad SDKs, no IDFA, no ATT prompt.** Verified: no ad or attribution SDK in
  `pubspec.yaml`, and no `NSUserTrackingUsageDescription` in `Info.plist`, because there is
  nothing to ask permission for.
- **Bupples is a calculator, not a wallet.** It records that a settlement happened; it
  never moves money. There are no card or bank details in the system to leak. "How to pay
  me" details are the user's own published handles, owner-writable only.
- Uploaded images are re-encoded, which strips EXIF and GPS by default.
- Archived hangouts are excluded from Pip's context by design, and Pip is instructed never
  to discuss them.

## Trust and safety

User-generated content (receipt images, hangout cover photos) can be **reported for
moderation** through `reportHangoutCover`, backed by a terms and abuse policy built to the
App Store UGC guidelines.
