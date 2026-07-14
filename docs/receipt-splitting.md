← [Back to the README](../README.md)

# Receipt splitting and the money model

The subsystem the whole product rests on. Every split, in every flow, reconciles to the
sen. Current at `1.1.0+112`.

For the wider system see [the architecture deep dive](ARCHITECTURE.md); this doc is the
money math.

## Integer sen, and one distributor

Money is integer minor units end to end. No float touches an amount, in Dart, in the
TypeScript functions, or in the Swift widgets.

Splitting an amount across weights uses a single deterministic distributor: floor each
share, then hand the leftover sen out largest-remainder-first, so the parts always sum
back to the exact total. That one routine is the trust anchor and everything below
composes it.

Formatting follows the same discipline. RM ledgers read tight (`RM12.50`); every other
ledger reads ISO-code style with a non-breaking space (`SGD 109.90`, `USD 42.00`).
There is no dollar sign in this app, including for dollars, because `$42.00` in a room
where three currencies are in play is a number that means nothing.

## Currencies never blend

Each currency is an independent ledger, everywhere. There is no exchange rate in
Bupples, so a blended figure would be a guess presented as a fact.

This is not a display rule that a helper function enforces at the last moment; it goes
all the way down. `Ledger.settlementsByCurrency` builds debts *within* each currency and
returns them keyed by code. The nudge engine flattens those per-currency debts, so a
debtor who owes you THB stays nudgeable even when the cross-currency net happens to be
zero. A real THB debt must never be cancelled out by an RM credit. An expense carries
its own currency code, blank meaning "the session's".

## Multiple payers, and the double-count

A single expense, receipt or item can be **fronted by more than one person**, split
equally or with exact per-payer amounts.

The model stays backward-compatible by omission. An expense carries an optional
`payersMap` (member → sen fronted); with one payer the field is absent entirely, so
existing single-payer records are byte-identical and need no migration. The `payers`
getter returns `payersMap ?? {paidById: amountCents}`, which **always sums to the
expense total**, so the rest of the system treats every expense as multi-payer with no
special cases and no branch to get wrong.

The hard part is settlement. The obvious approach, spreading each consumer's gross share
proportionally across the payers, **double-counts a payer who also consumed**. Someone
who paid exactly their own share must end up settled, owing and owed nothing; the naive
method has them both owing and being owed.

So the ledger works in **net positions**. For each expense it computes `net = paid −
share` per member, then fills debtors against creditors in a deterministic order, in
integer sen. That reduces to "everyone owes the lone payer" for the ordinary
single-payer case, and stays exact for any mix of payers and consumers.

Firestore rules cannot cheaply sum a map, so they validate the map's shape and size, the
client enforces the sum, and the server re-derives the balances it acts on. Three
layers, each doing the part it is actually able to do.

## Item-level splitting, tax, service, discount

When a bill is split by item, each line is divided only among the people who claimed it
(shared lines split evenly, and an item can carry a fractional share), and **tax and
service ride proportionally** to what each person ordered, with any whole-receipt
discount subtracted the same way. Remainder sen are distributed deterministically so the
per-person totals reconcile to the receipt total exactly.

The invariants are stated in the source and property-tested:

- `sum(memberItemSubtotals) + unclaimedPot == sum(item amounts)` **exactly**, for any
  input, with over-claims clamped deterministically first.
- Proportional tax and service conserve `taxCents + serviceCents` exactly when anything
  is claimed, by largest remainder over claimed subtotal.
- Split-rest-equally conserves the pot exactly, the extra sen going to the first member
  ids in sorted order.

Note the first one carries "for ANY input". Over-claiming is not an error path to
reject; it is clamped, deterministically, and conservation still holds. A bill where
four people each claim the same dish is a real Tuesday, not a malformed request.

## The Dart ↔ TypeScript twin

`functions/src/receipt_math.ts` is the deliberate server-side twin of the Dart claim
math. The client previews shares with the Dart copy; `finalizeReceipt` writes money with
the TypeScript one; the two must agree to the sen.

Both work in integer sen and use exact rational arithmetic (bigint on the server) so
rounding is deterministic on both sides rather than merely similar. Maintaining two
implementations of money math in two languages is a real, ongoing cost, taken on
purpose. The alternative is trusting a client with the write.

A third port exists for the browser: `turbo-ledger.js` runs the split math on the
no-app Turbo claim page, with a parity test over shared fixtures.

## Collaborative real-time receipts

Inside a hangout a receipt is a **live Firestore object** with a per-claimant `claims`
subcollection:

- The person who paid is the **owner**; edits are rules-gated to them.
- The scan is **reviewed and confirmed** before it goes live.
- Everyone **claims the items they had** by tapping the row. Claims are **per-claimant
  documents**, so concurrent writes never contend. The table is tapping at the same
  time, which is the entire point of the feature and would be a write-contention
  disaster on a single shared document.
- On finalise, the receipt **materialises into one exact-split expense**, counted
  exactly once, through the rate-limited and idempotency-keyed `finalizeReceipt`.

**Live balance previews.** While a receipt is still being claimed it contributes a
*preview* expense to the balance math, built from the same settlement the finaliser uses
and dedup-guarded so it counts once. The whole table's balances move in real time as
items are ticked, then snap to the identical figure when the receipt lands. Identical,
not merely close, because it is the same math.

**Edits, locks, and history.** Before a receipt is settled the owner can fix an item's
name, amount or quantity; anyone else can **suggest a correction** the owner approves or
declines, through `onChangeRequestCreated` / `onChangeRequestUpdated`. Once a receipt is
added to balances it **locks**: items, amounts and title can no longer drift from the
recorded proof, enforced in the app *and* in the Security Rules. A receipt that should
not have landed is **voided** (`voidReceipt`), not edited: the record stays, marked.

**Fully-claimed notification.** A trigger tells the receipt's owner once every item has
been claimed, so they can finalise. It is distinct from the "everyone's paid" event and
de-duplicated the same way.

## Receipt understanding: the scan

`scanReceipt` is a callable Cloud Function running **Gemini 2.5 Flash on Vertex AI**,
returning structured line-items plus tax / service / discount / total, the store name
and the currency as JSON via a response schema. Quantities are read into a structured
field ("2 × Burger") rather than buried in the item name.

This replaced a brittle Cloud Vision OCR plus text-heuristics pass that misread amounts
and tax. The `@google-cloud/vision` dependency has since been removed from the
functions package entirely.

The division of labour is strict, and it is the same one described for Pip in the
[architecture deep dive](ARCHITECTURE.md#5-pip): **the model fills in the numbers it
read off the paper; it never decides the split.** The split math that consumes the scan
is pure, unit-tested Dart mirrored by pure, unit-tested TypeScript. Vertex AI is reached
by application default credentials, so there is no Gemini API key anywhere in the
codebase to leak.

There is a per-user scan quota, held in a collection with no rules `match` block at all,
so the default deny keeps clients out and only the admin SDK can touch it.

## Settle-up views, and the trap underneath them

The group nets to the **fewest payments** (at most N−1 transfers) by default. A personal
**Net / Direct** switch flips to the exact who-owes-whom breakdown: *your* view only,
never anyone else's, and never the balances themselves.

Both views are pure read-side transforms over one ledger, so they always reconcile. And
`netBalances` derives from the *same* accepted-payment-clamped pairwise debts the direct
settle uses, so an over-applied payment (a double-accept, or a bill edited down after
its payment was accepted) can never invent a reverse debt.

But the simplifying variant is a trap for any pair figure. `Ledger.settlements(...)`
takes a required `simplify` flag; with `simplify: true` a greedy min-cash-flow solver
reroutes real pair debts through third people, which is correct for a settle plan and
**wrong for every you-to-them figure in the app**. It can hide a debt that exists or
show one pointing the wrong way. An adversarial review caught this shipping on the
offline-member screen at build 103.

The rule: **any you-to-them balance uses `simplify: false`**. Nudge eligibility reads the
direct debt specifically, so you cannot nudge someone over a balance that only *appears*
to point your way through a routing artefact. Full detail in
[the architecture deep dive](ARCHITECTURE.md#the-direct-settlement-trap).

## Settling, server-side

Settle-up is recorded per line where it can be, and the trusted decisions live on the
server:

- **`settleDirect`** verifies settlement acceptance server-side, so it cannot be forged
  or self-accepted by a client.
- **Partial payments** are supported: pay an amount now, the rest later.
- **`autoConfirmSettlements`** (scheduled) runs the 72-hour auto-confirm window, which is
  **off by default** per user. A payment silently auto-confirming is a defensible
  default only if the user chose it.
- **`writeOffDebt`** forgives a debt explicitly, and there is a dispute path.
- **`nudgeToSettle`** enforces the per-target cooldown and appends the who-nudged-whom
  record to a **client-unwritable event log**, so the audit trail is server-written like
  the rest.

The balance gate sits at the source: a pending or rejected expense is excluded from all
money math in one place (`_counting`), so every derived figure (net positions, settle
plans, per-currency ledgers, viewer projections, exports) agrees automatically, and
conservation still holds (the sum over counted expenses nets to zero).
