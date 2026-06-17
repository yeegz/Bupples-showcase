← [Back to the README](../README.md)

# Privacy, deletion, and trust

Bupples holds shared financial records — who paid, who owes, what was split — for
groups of people who trust each other. The design problem is that one person's
privacy choices can't be allowed to destroy proof that *other* people depend on.
The guiding rule is: **you can leave, but you can't delete your way out of a
debt.**

## Deletion anonymizes, it does not erase

Account deletion runs **server-side as an anonymize, not a purge**. A transaction
rewrites the leaving user's member entries to "Deleted user" and drops their uid
and pay handles, while leaving the expenses, transfers, and receipts the *other*
participants rely on untouched. Deleted people show up as anonymized participants
in the history that still references them. This is fraud-resistant by design — no
one can erase shared proof or dodge a debt — and it satisfies App Store Guideline
5.1.1(v) account deletion, including **Apple token revocation** when an
Apple-linked account is removed.

## Sessions archive, they don't vanish

Removing a session is a **one-way soft-archive**, not a destructive delete:
records are frozen, remain **exportable** by everyone who was in it, and stay
viewable as read-only. There is no client path to a hard delete of shared
financial data.

## Locks preserve proof

Once a receipt is added to balances it **locks** — items, amounts, and title can
no longer change — so the recorded expense can never drift from its proof,
enforced in the app and in the Security Rules. Pre-settle edits are **owner-
approved correction requests** appended to a **server-written history**.

## Authorization is bound to the auth identity

- Every write is authorized against the Firebase **auth uid**, never a
  client-supplied id.
- Firestore rules are **membership-scoped**: no enumeration, members-only writes,
  amounts validated server-side, and claim writes locked to the claimant.
- **No client hard-deletes** — sessions soft-archive one way, and deleted accounts
  anonymize server-side.
- **App Check** attests that requests come from the real app (reCAPTCHA on the
  web), and settle-up acceptance is verified end-to-end by a Cloud Function so it
  can't be forged or self-accepted by a client.

These rules were hardened through self-directed adversarial reviews of the
Security Rules and Cloud Functions before launch.

## Privacy by default

- Sign-in is **silent and anonymous** by default; linking Google or Apple is
  optional and only there to back up and sync across devices.
- Profile photos are opt-in and **mutual**: turn the feature off and you see no
  one's photo and no one sees yours.
- A group's data is scoped to its session; no one outside it can read it.
- Bupples is a calculator, not a wallet — it never handles real money, so there are
  no card or bank details to leak.

## Trust and safety

User-generated content (receipt images) can be **reported for moderation**, backed
by a terms and abuse policy built to App Store UGC guidelines.

## Settings

Account controls live in Settings; as of build 29 the **Delete Account** action
sits at the very bottom of Settings, separated from everyday controls so it can't
be hit by accident.
