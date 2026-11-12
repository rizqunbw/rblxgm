# 01 — GAME DESIGN DOCUMENT (GDD)
**Project:** MUTANT FACTORY | **Genre:** creature collection + factory tycoon + limited social heists | **Platform:** Roblox mobile/PC/console where supported | **Spec v1.0**

## 1. Core experience and session structure
- **First 30s:** spawn on a plot; camera briefly highlights free starter machine; tap highlighted button.
- **30–90s:** starter egg hatches; creature performs an expressive entrance; feed it to the workstation.
- **90s–3m:** collect first coins, upgrade machine, reveal mutation lab.
- **3–8m:** experiment with a guaranteed starter recipe, see a clear transformation and collection milestone.
- **8–15m:** optimize factory, visit a friend and inspect collections; optional side objectives.
- **Returning sessions:** daily free challenge, changing environment condition, persistent progression, new recipes.
- Target roundless sessions: 7–15 minutes for early prototype; these are hypotheses to test.

### Core loop
```text
FREE starter → PRODUCE resources → UPGRADE machine → HATCH creature
     ↑                                         ↓
CLAIM milestone ← RECORD discovery ← MUTATE / TRAIN / ASSIGN
                                             ↓
                                DECORATE / CO-OP / OPT-IN RAID
```
No idle-only design: each 1–3 minutes provide meaningful decisions, not only waiting.

## 2. World layout
- Hub radius ~90 studs; eight plots around a central circular plaza. Distances and stud values are **prototyping defaults**, not final art requirements.
- Every plot consists of: Entry, Incubator, Resource Machine, Mutation Lab, 6 Display/Workstation Pedestals, Upgrade Terminal, Decoration Zone, Security Panel.
- Shared hub: Quest Board, Collection Museum, Trading Lounge (post-MVP), Co-op Rift Gate (post-MVP), Shop, Tutorial signage.
- Paths must accommodate mobile cameras and physical movement without occlusion. Spawning/camera focused on factory entrance; avoid flashy purchase surfaces covering onboarding.
- Use plot ownership and per-player collision groups. Spawn plots from templates at fixed anchors; persist plot *configuration*, never large instance trees.

## 3. Creature catalog: launch prototype
All named designs are original placeholders to be modeled with distinctive silhouettes; do not use Roblox/other IP characters.
| ID | Name | Silhouette | Trait | Work role | Prototype tier |
|---|---|---|---|---|---|
| MUT_001 | Blobbit | squat jelly, two tiny hornlets | springy | coin maker | Starter |
| MUT_002 | Sparkleech | coil + antenna | electric | energy maker | Common |
| MUT_003 | Cogtoad | stout frog + gear shoulder | mechanical | conveyor speed | Common |
| MUT_004 | Mossmunch | moss mane + wide jaw | botanical | compost maker | Uncommon |
| MUT_005 | Glimwing | translucent diamond wings | luminous | lab catalyst | Rare |
| MUT_006 | Prismox | prism shell + heavy legs | crystalline | bonus resource | Epic |
Creature tiers and figures are initial tuning targets only. All six must be obtainable through gameplay without Robux.

### Creature attributes
`id, speciesId, tier, level, xp, appearanceSeed, genes[3], mutationTags[], assignmentSlot?, createdAt, locked, insuredUntil?`.
- Each creature has 3 visual gene slots: primary palette, accessory structure, effect marker. Keep gameplay stat effects tightly capped.
- Distinct silhouette wins over high polygon count. Show tier with shape, color-blind-safe icon, and label.
- Level 1–10 launch; 10–20 later. No level purchases necessary.
- Creature stage: EGG → HATCHED → ASSIGNED → RESTING → MUTATING → ASSIGNED. Never destroy a paid item or owned creature on a failed experiment.

## 4. Machines and production
| Machine | Input | Output | Starter interval | Notes |
|---|---|---|---|---|
| Incubator | earned egg ticket | new creature | 15 s | first ticket free, tutorial guaranteed Blobbit |
| Coin Press | assigned Blobbit / Cogtoad | coins | tick aggregated every 5 s | upgrades increase cap/output |
| Gene Lab | earned catalysts + two recipe traits | mutated creature appearance | 20 s | first recipe guaranteed |
| Bio Generator | assigned Mossmunch | catalysts | 30 s | post-MVP |
| Fusion Chamber | duplicates + resources | controlled fusion | 60 s | post-MVP; explicit confirmation |

Server computes resource production from timestamps/last claim and bounded caps, NOT one server loop per creature. Use per-player aggregation and bounded offline production (e.g. up to 2h, balancing TBD). Prevent clock spoofing by using server time.

## 5. Mutation design
Core differentiator: players experiment with **visible recipes** and partial clues instead of pure luck.
- Starter guaranteed recipe: Blobbit + earned Blue Catalyst → Azure Blobbit (cosmetic variant + 5% capped output bonus).
- Discovery recipes: player unlocks clues by playing events and using gene lab; reveal outcome pools *before* committing.
- Gene combination example: Sprout trait + Spark trait + 2 catalysts → Lumen Mossmunch.
- Rarity reward pacing initially Common 65%, Uncommon 25%, Rare 8%, Epic 1.8%, Secret 0.2% for **free earned eggs only**. These sample values require balancing and actual probability tests; do not add paid random items without platform compliance.
- Mutation result must be computed server-side using audited weighted selection and journaled transaction.
- Pity-like progress milestones should be presented as deterministic progress rewards rather than misleading "almost got it" visual tricks.
- Cosmetic variations can be plentiful, but stat deltas remain tightly controlled (max prototype 10%).

## 6. Quest system
**Tutorial chain:** (1) Claim plot (2) Place incubator (3) Hatch Blobbit (4) Assign station (5) Collect 50 coins (6) Upgrade press (7) Craft first mutation (8) claim collection badge.
**Daily:** claim 3 production cycles; feed 2 creatures; finish 1 recipe; visit museum. Rewards are earned, not purchased.
**Weekly:** complete 6 unique recipes, join 2 co-op events, save 1 friend from event hazard.
Quest definitions are data-driven, server verifies completion; awards must be idempotent.

## 7. Social and raids (post-MVP, explicitly gated)
Raids are a **short opt-in challenge**, never destructive theft by default.
- Attacker opens Raid Board and enters a contract; defenders enroll via Security Panel or accept a friend challenge.
- Defender sees intrusion warning; a 60–90s escape/defense mini-game starts. Use asynchronous NPC defense if defender is offline only after explicit consent.
- Attacker may claim **one raid token or a replica of an unlocked creature**, not permanently remove the victim's creature. Preserves player trust.
- Defender receives consolation resources and skill rewards. No real-money item can be raided or lost.
- 3 contracts/day default, 10m cooldown; prevent repeat targeting and harassment; server validates participants and contract IDs.
- PvP alternatives: friendly racing of factory output, leaderboard by collections, cooperative boss.
- Trading remains OFF until duping defenses, transaction logs, anti-fraud, and moderation exist.

## 8. Events and viral moments
- Rotating 15-minute "Mutant Weather" (e.g., Neon Storm, Pollenfall, Crystal Eclipse) changes *free* discovery options, not gambling pressure.
- Weekly co-op boss requires 3–8 players; rewards earned challenge tickets/cosmetics; scale by active players.
- Every big reveal creates a 3-second non-blocking camera-safe celebration: creature jumps, lab lighting pulses, confetti, and shareable scene.
- Never automate posting to social networks; provide an optional photo mode and screenshot-friendly visual framing.

## 9. UX and accessibility
- Primary HUD: coins top-left, next goal top-center, shop icon top-right, build and collection bottom buttons. Keep thumb zones free of Roblox CoreGui/chat.
- On mobile, large tap targets, contextual hints, no walls of text, no shop prompts in first tutorial phase.
- No reliance on red/green alone. Reduced effects toggle, camera shake toggle, SFX/music controls, text labels on icons.
- Every action has feedback: sound, small motion, toast, failed action reason, network retry.
- Show purchase price in Robux and precise permanence/expiration; no ambiguous "free" prompts.

## 10. User stories and acceptance
1. New player can complete tutorial with touch only in 3–5 minutes; never forced to purchase.
2. Player reconnecting retains owned creatures, currency and upgrades with no duplication.
3. Player with low FPS can disable expensive effects without missing relevant mechanics.
4. Player cannot modify other plots via client-side RemoteEvents.
5. Collector can view missing creatures and acquisition methods.
6. All monetized cosmetic entitlements survive server switch.
7. A server shutdown mid-reward leads to no duplicate item or lost Robux product after reconciliation.

## 11. Out of scope until retention validated
Open-world zones, user-generated scripting, blind paid lootboxes, permanent asset theft, player-to-player Robux trades, cross-experience inventory, global auction markets, 100+ creatures at launch, complex combat, native external web services.
