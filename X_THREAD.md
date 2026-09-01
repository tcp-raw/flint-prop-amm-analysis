# Flint — X thread (submission draft)

Every factual claim below is sourced from flint.trade or docs.flintlabs.dev.
Nothing is estimated, and no cost figures are invented.

---

### 1/ hook

Most "should you build your own prop AMM on Solana" takes argue about audit cost.

Wrong axis.

The thing that actually kills in-house prop AMMs on Solana is that **requoting is a write**, and you requote all day.

Here's the part @flint_trade_ solves that nobody talks about 🧵

---

### 2/ the real cost center

A market maker's core loop is: price moves → cancel → requote.

On an orderbook you rewrite order state. On Solana every one of those rewrites is an on-chain write competing for blockspace, burning compute units, and paying priority fees.

Your infra bill doesn't scale with volume. It scales with **how often you change your mind**.

Which, if you're any good, is constantly.

---

### 3/ what Flint actually changed

Flint's quoting primitive is **fair price + offsets**.

You install a spread ladder once. After that you shift your entire book by updating a single fair value:

```rust
QuoteBuilder::new()
    .oracle_offset("SOL", |b| b.with_fair((155., 155.)))
    .commit(&mut core).await?;
```

One cheap update. Whole ladder moves.

That's not a UX nicety, it's the unit economics of the whole desk.

---

### 4/ pro-rata, stated correctly

A lot of people will tell you Flint's pro-rata means "size wins."

It doesn't. From their own docs:

> sharper pricing still earns priority, but a step of latency no longer shuts you out

Price still leads. What's removed is the part where a 20ms disadvantage takes you to **zero fill** instead of a smaller one.

You compete on pricing, not on colocation.

---

### 5/ the capital efficiency bit

Each listed token gets one market account and vault. Inside it, every maker runs an **isolated USDC-quoted mini-book**.

Then Flint derives crosses per-maker, on demand:

`JTO/SOL = JTO/USDC ÷ SOL/USDC`

So you quote the cross without parking dedicated JTO/SOL inventory. Rebalance your SOL/USDC leg and every SOL cross reprices with it.

One inventory pool. Many pairs.

---

### 6/ distribution you'd otherwise have to earn

An in-house AMM is invisible until aggregators route to it. That's a BD problem and an ongoing integration-maintenance problem, not a coding problem.

Flint is already wired in:

Jupiter — live
DFlow — live
Titan — live
OKX DEX — in progress

Solana did $47.25B DEX volume trailing 30d, and Jupiter/DFlow/Titan have routed $1.36T all-time. (DefiLlama, as of 2026-08-07)

---

### 7/ who's behind it

Matters for infra you're routing size through:

CTO @thedavidgorski — prev Jito Labs, Jump Trading
CEO @Josh_E_Wa — prev dYdX Foundation
Engineers out of Anza, Google, Coinbase, Drift, Step Finance

Audited by Certora.

---

### 8/ the honest summary

Flint doesn't remove the hard part of market making. You still own your pricing, your risk, your inventory.

It removes the part that was never your edge: transaction landing, priority fees, aggregator integrations, and paying a write every time you change a quote.

Your alpha is your model. Not your RPC fleet.

🔗 https://flint.trade/
📄 https://docs.flintlabs.dev/

---

## Submission checklist

- [x] Single X thread
- [x] Tags @flint_trade_ (post 1)
- [x] Links https://flint.trade/ (post 8)
- [x] English
- [ ] **Posted live** — must be published before submitting on Superteam Earn

## Accuracy notes

| Claim | Source |
| --- | --- |
| fair price + offsets requote model | flint.trade — "Efficient pricing updates: Fair + offsets" |
| `QuoteBuilder` / `oracle_offset` snippet | flint.trade code sample |
| pro-rata retains price priority | flint.trade — verbatim quote, post 4 |
| isolated USDC mini-book per maker | flint.trade — "Simple USDC Quoting" |
| synthetic cross formula | flint.trade — "Per-Maker Synthetic Cross" |
| Jupiter/DFlow/Titan live, OKX in progress | flint.trade integrations panel |
| $47.25B 30d, $1.36T all-time | flint.trade, citing DefiLlama, 2026-08-07 |
| team backgrounds, Certora audit | flint.trade team + audit sections |

No cost, ROI, or savings figures are claimed anywhere. Flint does not publish
pricing, so any TCO comparison would be invented.
