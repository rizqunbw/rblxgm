# 07 — ANALYTICS, CONTENT & LIVE OPERATIONS
## 1. Event taxonomy (privacy-minimizing)
| Event | Properties | Why |
|---|---|---|
| session_start | buildVersion, deviceClass, playerCohort | acquisition mix |
| tutorial_step | stepId, success, elapsedSeconds | onboarding friction |
| plot_assigned | availablePlots, loadTimeMs | capacity |
| machine_placed | machineType, level | activation |
| creature_hatched | speciesId, tier, sourceType=earned | collection engagement |
| mutation_crafted | recipeId, outcomeId, earnedCost | reveal engagement |
| session_end | durationSeconds, state | session depth |
| shop_view | surface, cohort | discoverability |
| purchase_prompt | skuId, source | monetization funnel |
| receipt_granted | skuId, receiptDeduped | purchase reliability |
| error | errorCode, subsystem, buildVersion | reliability |
Avoid recording names, chat content, full IP, payment data or other unnecessary personal data. Use Roblox-supported services and compliant privacy practices, never send external telemetry without explicit applicable compliance review.

## 2. KPIs and hypotheses
- D1 retention: fraction of day-0 cohort who return next day; define exact time window consistently.
- D7 retention: day-0 cohort who return 7 days later.
- Tutorial completion = completed tutorial / players started tutorial.
- First mutation activation = users mutating within initial 10m / first-session players.
- Median session duration, sessions/player/day, payer conversion, Robux revenue per DAU, successful receipt percentage.
- Quality: crash frequency, save failures, remote validation rejects, FPS by device tier.
- Primary MVP validation question: "Do users voluntarily craft a second mutation after their first?"

## 3. Experiments
- A: tutorial 6 vs 8 steps; success = higher completion with no loss in understanding.
- B: first egg hatch 10s vs 15s; success = higher D1 and satisfaction.
- C: collection index shown at 3m vs 5m; success = voluntary collection interactions.
- Never A/B test deception, undisclosed odds, or harmful pressure.
- Use stable cohort assignment based on userId and experiment key (server-side); do not alternate unpredictably between sessions.

## 4. Live-ops schedule (only after stable launch)
| Cadence | Content | Cost |
|---|---|---|
| Daily | 3 earned quests, rotating recipe hint | low |
| Weekly | 1 co-op boss + thematic weather | medium |
| Biweekly | 1–2 original creature variants | art medium |
| Monthly | cosmetic factory theme + challenge track | high |
| Quarterly | new modular factory sector | high |
Every event must have clear start/end UTC timestamps, timezone-aware display, fair access, tested rollback and support notes. No fake time-limited offers.

## 5. Community, support and moderation
- User-generated text appears only through Roblox filtering and platform-appropriate systems.
- Report/disable griefing, raids are strictly opt-in, block/ignore respected where possible.
- In-game Help shows support instructions and known issues. No promises of off-platform gifts or Robux to minors.
- Incident runbook: detect → disable affected feature → preserve logs → rollback if safe → restore entitlements → communicate factual status.

## 6. Growth content
- 10-second vertical clip format: (0–2s) visible mystery egg (2–5s) recipe setup (5–8s) mutation reveal (8–10s) playful payoff.
- Showcase mechanical novelty rather than purchased advantages; avoid targeting or manipulating children.
- Creative experiments: unusual gene combinations, defense layout contests, co-op boss teamwork, world-record factory design.
- Record organic creator engagement and voluntary invites; no coercive follow/like mechanics.

## 7. Post-launch decision rules
- If tutorial completion is poor, fix onboarding before increasing acquisition.
- If D1 is weak despite completion, add meaningful early goals rather than more shopping prompts.
- If purchases succeed but refunds/complaints spike, remove misleading offerings and review balance.
- If retention is solid but revenue modest, improve transparent cosmetic value and personalization; never trade player trust for temporary revenue.
