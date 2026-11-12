# 02 — ECONOMY & MONETIZATION DESIGN
**Principle:** maximize lifetime player value by earning trust and repeat play, not by pressuring purchases. All product prices are **test hypotheses**, not verified optimal price points.

## 1. Currencies and item ownership
- **Coins:** earned from workstations, quests, co-op; buys machine upgrades, new workstations, recipe hints.
- **Catalysts:** earned from quests and generators; spent on mutation recipes.
- **Challenge Tickets:** non-purchasable event reward tokens.
- **Robux:** Roblox platform payment only; never store raw Robux balances in DataStore.
- Keep premium shop purchases cosmetic / customization / modest convenience. No paid protection against irreversible theft.
- No direct conversion between user-to-user currencies. No unofficial Robux exchanges.

## 2. Economy design targets
Example **draft** for first 15m:
| Time | Earned coins total | Key milestone | Intended feeling |
|---|---:|---|---|
| 0–1m | 0–50 | place factory | immediate agency |
| 1–3m | 50–200 | first upgrade | momentum |
| 3–6m | 200–500 | mutation lab access | surprise |
| 6–10m | 500–1,200 | first variant | ownership |
| 10–15m | 1,200–2,500 | unlock second slot | next goal |
These must be simulated from actual source/sink parameters, not manually hard-coded as rewards.

### Initial example formulas
- Factory production per collection cycle = Σ (baseOutput × tierMultiplier × min(1.10, geneBonus) × machineMultiplier).
- Machine upgrade cost for level n: round(75 × 1.45^(n-1)), with price cap and dedicated tiers.
- Offline earning = min(timeAway, 2 hours) × capped production rate × 0.35; calculated on server with lastSeen and anti-overflow protections.
- Claim must check production snapshot and never trust a client-provided amount.
- Avoid inflation by monitoring total coins created/consumed, wealthy percentile, resource stockpile growth.

## 3. Shop catalog (example price points, subject to in-game dashboard + pricing tests)
| SKU | Roblox type | Tentative price | Benefit | Lifetime / fairness |
|---|---|---:|---|---|
| VIP Factory Theme | Pass | 399 R$ | themed walls, machine materials, exclusive emotes | permanent; no exclusive power |
| Extra Display Slots +2 | Pass | 149 R$ | display only, no production multiplier | permanent |
| Neon Factory Skin | Pass | 199 R$ | unique lighting+machinery shells | permanent |
| Mutant Emote Pack | Pass | 79 R$ | 5 animations | permanent |
| Factory Banner Pack | Developer Product | 49 R$ | extra selectable decoration set; duplicates grant credits or block purchase | repeatable only if actually consumable; prefer Pass for permanent set |
| Temporary Photo Mode Filters | Developer Product | 29 R$ | specific clearly timed cosmetic filter | repeatable; expiry visible |
| Seasonal Cosmetic Bundle | Pass or Developer Product | 249 R$ | disclosed items, no randomized results | fixed, no power |
**Important correction:** permanent unique sets should normally be Passes, not repeatable developer products. Choose final SKU type once entitlement semantics are fixed.

## 4. Purchase architecture
- Game Pass: server verifies ownership using MarketplaceService and reconciles join/respawn; process purchase-finished signals without treating a client callback as ownership proof.
- Developer Products: ONLY one server `MarketplaceService.ProcessReceipt` handler, mapping known product IDs → durable entitlements. Persist idempotency journal keyed by PurchaseId before acknowledging. On transient save errors return NotProcessedYet; on success return PurchaseGranted. Have replay/reconciliation for crashes.
- Never grant products solely through a RemoteEvent or client prompt result. Enforce currency, inventory and cap validation on server.
- Product ID registry stored in Config; never invent IDs. Sandbox/playtest uses mock purchase services isolated from live receipts.
- Display localized prices when supported; do not hardcode numeric Robux prices into UI as authoritative live values.
- Record conversion funnel: shop_view, product_detail, purchase_prompt, receipt_granted; do NOT record payment data.

## 5. Random-item and region compliance
**MVP sells no random creature, egg, mutation chance, luck boost, or paid chest.** Roblox's paid random items rules cover direct and indirect Robux purchases, paid currency, chance modifiers, and fusion outcomes. If ever proposed, get policy/legal review; disclose all numerical odds and respect regional restrictions via current Roblox policy APIs. Safer choice: sell **specific known creatures only as cosmetics** or fixed appearance bundles and retain random progression entirely earnable with non-purchasable resources.
Reference: https://create.roblox.com/docs/production/monetization/paid-random-items

## 6. Value, conversion, and long-term economics
- Player segmentation: new (0–10m), activated (first mutation), returning (D1+), collector, decorator, social.
- Measure revenue per daily active user, unique payer conversion, repeat purchases, refunds, negative feedback, D1/D7 retention.
- Revenue is contingent on platform fees, DevEx eligibility and current payout rules; do not promise conversion to fiat.
- No "buy to keep stolen asset" or "pay to get refund"; no repeated aggressive pop-ups, misleading "last chance" banners, or child-targeted pressure.

## 7. Balance and transaction integrity
Server-only economy ledger entries:
`transactionId, userId, kind, itemId?, amount, source, serverJobId, createdAt, version`.
- Every grant/spend is a single validated operation. Apply ledger+inventory+currency state together within a player-profile update; do not use uncoordinated writes.
- Item ownership unique immutable UUID. Never clone two durable copies of one creature.
- When future trading ships, use two-party escrow/state machine; do not depend on independent two-user DataStore writes for atomic transfers.
- Define reconciliation scripts for questionable receipts and duplicate transaction IDs.

## 8. Economic QA scenarios
- Purchase same dev product twice, reconnect during receipt, and replay a duplicate receipt.
- Buy Pass, change server, verify entitlement once.
- Client sends negative cost, enormous amount, invalid product ID, other-player target, stale server timestamp.
- Coins at max integer/cap; interrupted upgrade; full inventory; simultaneous button taps.
- Test 1k simulated users with production distributions; inspect zero-progress players and runaway inflation.
