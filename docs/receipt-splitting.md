← [Back to the README](../README.md)

# Receipt splitting and the money model

This is the subsystem the whole product rests on: every split, in every flow,
reconciles to the cent. The same Dart math runs in the iPhone app and the web
app (via a parity-tested JavaScript port), and it is reused by Turbo, by
collaborative session receipts, and by plain expenses, so every path agrees down
to the last cent.

## Cent-safe arithmetic

Money is integer cents end to end — no floats. Splitting an amount across weights
uses a single deterministic distributor: it floors each share, then hands the
leftover cents out largest-weight-first, so the parts always sum back to the
exact total. That one routine is the trust anchor; everything below composes it.

## Multiple payers (build 29)

A single expense, receipt, or item can be **fronted by more than one person** —
split equally or with exact per-payer amounts. The model stays
backward-compatible: an expense carries an optional `payersMap` (member → cents
fronted); when there is only one payer the field is omitted entirely, so existing
single-payer records are byte-identical and need no migration. The `payers`
getter returns `payersMap ?? {paidBy: amount}`, which **always sums to the
expense total**, so the rest of the system can treat every expense as multi-payer
without special cases.

The hard part is settlement. The naive approach — spread each consumer's gross
share proportionally across the payers — **double-counts a payer who also
consumed**: someone who paid exactly their own share must end up settled, owing
and owed nothing. The ledger instead works in **net positions**: for each expense
it computes `net = paid − share` per member, then fills debtors against creditors
in a deterministic order, in integer cents. That reduces to "everyone owes the
lone payer" for the ordinary single-payer case, and stays exact for any mix of
payers and consumers. Firestore rules can't cheaply sum a map, so they validate
the map's shape and size and the **client enforces the sum**, with the server
re-deriving balances it acts on.

## Item-level splitting with tax, service, and discount

When a bill is split by item, each line is divided only among the people who
claimed it (shared lines split evenly), and **tax and service ride
proportionally** to what each person ordered, with any **discount** subtracted the
same way. Remainder cents are distributed deterministically so the per-person
totals reconcile to the receipt total exactly. This is the routine reused by both
Turbo and collaborative session receipts.

## Collaborative real-time receipts

Inside a normal session a receipt is a **live Firestore object** with a per-member
`claims` subcollection:

- The person who paid is the **owner** (edits are rules-gated to them).
- The scan is **reviewed and confirmed** before it goes live.
- Everyone **claims the items they had** by tapping the row; claims are
  **per-claimant documents** so concurrent writes never contend.
- On finalize, the receipt **materializes into one exact-split expense**, counted
  exactly once.

**Live balance previews.** While a receipt is still being claimed it contributes a
*preview* expense to the balance math, built from the same settlement the
finalizer uses and dedup-guarded so it is counted once. The whole table's
balances — and each person's paid / share / owed breakdown — move in real time as
items are ticked, then snap to the identical figure when the receipt lands.

**Edits, locks, and history.** Before a receipt is settled the owner can fix an
item's name, amount, or quantity; anyone else can **suggest a correction** the
owner approves or declines. Every change is appended to an **edit history written
through a Cloud Function** (so the record is server-written and clients can't
rewrite it). Once a receipt is added to balances it **locks** — items, amounts,
and title can no longer drift from the recorded proof, enforced in the app *and*
in the Security Rules.

**Grouped receipts (build 29).** When a session accumulates several receipts they
**stack into one card** that opens a list, instead of crowding the screen.

## Item-level paid tracking

Settle-up is recorded **per line, not per person**: a claim carries the set of
item ids actually paid, and the "fully settled" state is decided **server-side** —
true only when every split item is claimed *and* every debtor-claimed item is
marked paid (with a legacy whole-share fallback for old data). So marking one item
paid from the web or the app can never trip a false "everyone's paid" push, and a
retry or reopen can't replay one (notify-locks de-dupe).

Build 29 adds an owner **"fully-claimed" notification**: a Cloud Function
(`onReceiptFullyClaimed`) tells the receipt's owner once every item has been
claimed, so they can finalize or clear it — distinct from the "everyone's paid"
event, and de-duplicated the same way.

## Receipt understanding (the scan)

A callable Cloud Function runs **Gemini 2.5 Flash on Vertex AI**, returning
structured line-items plus tax / service / discount / total, the store name, and
the currency as JSON via a response schema. Quantities are read into a structured
field ("2 × Burger") rather than buried in the item name. This replaced a brittle
Cloud Vision OCR + text-heuristics pass that misread amounts and tax. The split
math that consumes the scan is pure Dart, shared with the web — the model only
fills in the numbers; it never decides the split.

## One implementation, two languages

The split math is written once in Dart and **ported to JavaScript for the web**,
where friends without the app claim Turbo items in the browser. A **parity test**
runs the same fixtures through both and asserts they agree to the cent, so the app
and the browser can never quietly diverge.

## Settle-up views

The group nets to the **fewest payments** (at most N−1 transfers) by default. A
personal **Summary / Full** switch flips to the exact who-owes-whom breakdown —
*your* view only, never anyone else's and never the balances. Both views are pure
read-side transforms over the one ledger, so they reconcile exactly.

**Nudge eligibility always reads the direct pairwise debt**, never the simplified
plan, so you can't nudge someone a routed balance only *appears* to send your way.
Each nudge is appended to a **client-unwritable event log by the reminder Cloud
Function**, so the who-nudged-whom record is server-written like the rest of the
audit trail.
