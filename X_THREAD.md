# Flint — X thread (copy-paste ready)

Every post is under X's 280-char limit. Post in order as a thread.
Every factual claim is sourced from flint.trade or docs.flintlabs.dev.
No cost, ROI, or savings figures are claimed anywhere.

---

### 1/

Most "should you build your own prop AMM on Solana" takes argue about audit cost.

Wrong axis.

What actually kills in-house prop AMMs is that requoting is a write — and you requote all day.

Here's the part @flint_trade_ solves that nobody talks about 🧵

---

### 2/

A market maker's loop: price moves → cancel → requote.

On Solana every rewrite is an on-chain write. Blockspace, compute units, priority fees.

Your infra bill doesn't scale with volume. It scales with how often you change your mind.

Which, if you're any good, is constantly.

---

### 3/

Flint's quoting primitive is fair price + offsets.

Install a spread ladder once, then shift the whole book with a single value:

.oracle_offset("SOL", |b| b.with_fair((155., 155.)))

One cheap update. Entire ladder moves.

That's not UX. That's the unit economics of the desk.

---

### 4/

People will tell you Flint's pro-rata means "size wins."

It doesn't. From their own docs:

"sharper pricing still earns priority, but a step of latency no longer shuts you out"

Price still leads. What's gone is 20ms costing you the entire fill instead of part of it.

---

### 5/

Each token gets one market account and vault. Every maker runs an isolated USDC-quoted mini-book inside it.

Crosses are derived per-maker, on demand:

JTO/SOL = JTO/USDC ÷ SOL/USDC

So you quote the cross without parking JTO/SOL inventory.

One pool, many pairs.

---

### 6/

An in-house AMM is invisible until aggregators route to it. That's BD plus ongoing integration upkeep, not code.

Flint is already wired in:

Jupiter — live
DFlow — live
Titan — live
OKX DEX — in progress

Solana did $47.25B DEX volume in 30d (DefiLlama, 2026-08-07)

---

### 7/

Matters for infra you're routing size through:

CTO @thedavidgorski — prev Jito Labs, Jump Trading
CEO @Josh_E_Wa — prev dYdX Foundation
Engineers out of Anza, Google, Coinbase, Drift, Step Finance

Audited by Certora.

---

### 8/

Flint doesn't remove the hard part of market making. You keep your pricing, risk and inventory.

It removes what was never your edge: tx landing, priority fees, aggregator integrations, and paying a write every time you change a quote.

Your alpha is your model.

https://flint.trade/

---

## Submission checklist

- [x] Single X thread
- [x] Tags @flint_trade_ (post 1)
- [x] Links https://flint.trade/ (post 8)
- [x] English
- [x] All 8 posts under 280 chars
- [ ] **Posted live** — publish before submitting on Superteam Earn

## Accuracy notes

| Claim | Source |
| --- | --- |
| fair price + offsets requote model | flint.trade — "Efficient pricing updates: Fair + offsets" |
| `oracle_offset` / `with_fair` snippet | flint.trade code sample |
| pro-rata retains price priority | flint.trade — verbatim, post 4 |
| isolated USDC mini-book per maker | flint.trade — "Simple USDC Quoting" |
| synthetic cross formula | flint.trade — "Per-Maker Synthetic Cross" |
| Jupiter/DFlow/Titan live, OKX in progress | flint.trade integrations panel |
| $47.25B 30d DEX volume | flint.trade, citing DefiLlama, 2026-08-07 |
| team backgrounds, Certora audit | flint.trade team + audit sections |
